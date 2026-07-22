---
title: "Product Brief: eval-quality"
status: hypothesis-validation
created: 2026-07-17
updated: 2026-07-22
---

# Product Brief: eval-quality

## Current Decision: Validate Before Building

The original brief assumed `eval-quality` should build a standalone TypeScript evaluation engine. That assumption was reversed on 2026-07-22 after a corrected competitor review identified [`agentevals-dev/agentevals`](https://github.com/agentevals-dev/agentevals), which already provides most of the runner, trace, golden-set, CI, reporting, and custom-evaluator machinery described here. Promptfoo covers a second mature baseline.

The current execution authority is [HYPOTHESIS_VALIDATION_PLAN.md](../../../../HYPOTHESIS_VALIDATION_PLAN.md). Do not implement the standalone engine scope in this brief before that plan reaches a decision.

Current product boundary:

- `eval-quality` is the single name for the agents, rules, methodology, governance, and any semantic evaluator pack that passes validation. The former name "AE" is retired.
- Existing open-source engines provide execution, trace ingestion, standard assertions, repeated runs, reports, and CI integration.
- The independent black-box evaluator model remains unproven. TEA compiles a sealed Eval Contract before implementation; the evaluator receives only that contract and black-box access.
- Four proposed semantic capabilities remain unproven: claim-to-evidence lineage, semantic checkpoints, process and outcome separation, and first material error attribution.
- Prototype only those capabilities through public engine extension points.
- If the hypotheses fail, `eval-quality` remains the methodology and governance layer over existing tools.

The remainder of this brief preserves the original product hypothesis and its research rationale. Where it conflicts with this section or the validation plan, the validation plan controls.

## Executive Summary

`eval-quality` is an open-source evaluation methodology for AI agents and agent-built software, plus a conditional pack of semantic evaluators for AI-agent traces. It turns runtime behavior into evidence for a ship/don't-ship decision and runs on established evaluation engines.

The bet behind eval-quality is a claim about where value lives in AI-assisted delivery: **planning and validation are the expensive, high-value ends; code generation in the middle is cheap and getting cheaper.** Teams have mature evidence sources at the validation end for deterministic software: unit, contract, E2E, performance, observability. Agent behavior is the part of validation that stays hardest to pin down, because agents are non-deterministic and a right answer reached by luck is a different failure from a wrong answer reached by good reasoning on bad data. Output-only scoring cannot tell those apart.

The current hypothesis is that existing tools still miss valuable semantics: **claim-to-evidence lineage**, **semantic checkpoints that accept valid alternate paths**, **process and outcome separation**, and **first material error attribution**. Market research supports testing these ideas. It does not establish product value. The validation plan must prove incremental value against existing tools before any evaluator becomes production scope.

## The Problem

Agents are shipping into production faster than teams can build confidence in them. When a team asks "is this agent good enough to ship?", the honest answer today is usually a vibe check plus a benchmark score, and neither survives contact with reality:

- **Benchmark numbers hide the failure that matters.** "The agent scored 87%" says nothing about *why* the 13% failed, or whether the 87% was reached by sound reasoning or by luck. A gate needs to know the difference.
- **Output-only grading is blind to the trajectory.** An agent can fabricate a confident answer that happens to be right, or reach a wrong conclusion through a sound process on stale data. Grading only the final string treats these identically.
- **Exact-match trajectory checks are brittle.** Asserting the agent called exactly these tools in exactly this order punishes valid alternative paths and breaks on every prompt tweak, so teams abandon it.
- **Judging the reasoning prose is a trap.** Chain-of-thought is frequently *unfaithful* - the stated reasoning does not reflect the computation that produced the answer - so scoring the narrative rewards good storytelling, not good behavior.
- **Semantic diagnosis remains inconsistent.** Mature tools cover execution, traces, tool checks, and outcome grading. The open question is whether they reliably connect claims to observed evidence, accept alternate sound paths, separate process from outcome, and localize the first material error.

The cost of the status quo: teams either ship agents on faith, or they over-invest in bespoke, unmaintainable eval harnesses. Either way, agent behavior stays outside the evidence-based release process that governs the rest of their software.

## The Solution

eval-quality has two layers under one name:

1. **Methodology and governance:** agents and rules identify risks, propose appropriate evals, generate configuration, interpret evidence, and apply a PASS / CONCERNS / FAIL policy.
2. **Conditional semantic evaluator pack:** custom evaluators run through an established engine's public extension points. Only evaluators that pass the validation plan enter this layer.

The established engine supplies agent execution, trace ingestion, standard assertions, repeated runs, cost tracking, reports, CLI behavior, and CI integration.

The primary workflow hypothesis targets human-on-the-loop and dark-factory delivery. TEA recognizes eval-relevant work and compiles a sealed Eval Contract from the product spec. A separate evaluator agent receives the contract, scoped test resources, and black-box system access. It receives no original spec, source code, repository, or builder context. It chooses its own evaluation actions from runtime observations. The [hypothesis validation plan](../../../../HYPOTHESIS_VALIDATION_PLAN.md) defines this provisional research boundary.

The proposed evaluator pack contains four hypotheses:

- **Claim-to-evidence lineage:** trace each material answer claim to supporting observed evidence and identify unsupported or contradicted claims.
- **Semantic checkpoints:** model required dependencies using accepted paths or a dependency graph, then accept valid alternate routes and reject materially unsound ones.
- **Process and outcome separation:** distinguish lucky success, sound failure caused by bad evidence, unsound failure, and ordinary success.
- **First material error attribution:** identify the earliest trace event that caused the later material failure.

## What Makes This Different

eval-quality makes a testable bet about which semantics may add value beyond existing engines:

- Test whether an isolated black-box evaluator finds material defects missed by builder self-evaluation and pre-canned tests.
- Test whether claim-to-evidence lineage finds material failures the baselines miss.
- Test whether semantic checkpoints accept valid alternate paths while catching unsound dependencies.
- Test whether process and outcome separation changes diagnosis beyond existing scorers.
- Test whether first material error attribution localizes failures accurately enough to improve remediation.
- Keep exact sequence matching and reasoning-prose scoring outside the proposed differentiator.

**Corrected landscape.** The original research missed `agentevals-dev/agentevals`, a framework-neutral, local-first Python engine with OpenTelemetry trace ingestion, golden sets, CI gates, reports, and language-neutral custom evaluators. Promptfoo also covers mature trajectory assertions and CI execution. These products remove the case for rebuilding the commodity engine. Research such as TRACE and CORE remains useful as a design prior for the semantic hypotheses. It does not prove production value.

**What we reuse.** The selected engine supplies the runner, trace model, baseline assertions, reports, and CI contract. eval-quality prototypes remain custom evaluators and a small normalization shim. Any engine limitation is recorded as experiment evidence. It does not authorize a replacement engine during validation.

**What may make it different** is a proven combination of semantic evidence, risk-driven evaluator selection, traceable findings, and explicit gate governance. The experiment determines which parts deserve that claim.

## Who This Serves

- **Primary: teams shipping AI agents** who already run evidence-based release gates for deterministic software and want agent behavior held to the same standard.
- **Primary: teams operating human-on-the-loop software factories** who need an evaluator independent from the builder and its context.
- **Primary: platform / QA / test-architect engineers** building the release gate itself, who need a machine-readable evidence artifact they can wire into CI alongside unit/contract/E2E/perf checks.
- **Secondary: agent framework authors and OSS contributors** who need reusable evaluation methodology and semantic evaluators across frameworks.
- **Secondary (early): the author's own projects** - `couture-cast` and a BMAD skill's own evals are the initial dogfooding targets.

Success for these users looks like: replacing "I think the agent is fine" with a defensible PASS / CONCERNS / FAIL backed by a trajectory they can inspect.

## Success Criteria

- **Semantic incremental evidence:** prototypes correctly catch at least three real, material failures missed by both baseline tools, spanning at least two semantic hypotheses and two agent systems.
- **Independent evaluation:** the black-box evaluator catches at least three real, material defects missed by both self-evaluation and deterministic tests, spanning an agentic system and a conventional system, with zero context-boundary breaches.
- **Safety:** every packaged evaluator reaches at least 0.85 precision and 0.80 recall, with zero false FAIL verdicts on valid behavior.
- **Stability:** judge-backed verdicts agree across repeated runs at least 90 percent of the time.
- **Decision value:** at least one finding changes a real ship decision or materially changes remediation.
- **Integration:** passing evaluators run through public extension points without an engine fork.
- **Demand:** after a technical pass, at least three organizations commit traces, engineering time, or a design-partner pilot.

The complete corpus, labeling protocol, metrics, and decision rules live in the validation plan.

## Scope

**In the validation phase:**

- A locked, human-labeled trace corpus from at least two agent systems.
- A 12-task comparison of deterministic tests, builder-context self-evaluation, and isolated black-box evaluation across one agentic system and one conventional system.
- Strong baseline configurations for `agentevals-dev/agentevals` and Promptfoo.
- Disposable prototypes for the four semantic hypotheses.
- Comparative results, repeated-run stability evidence, and a recorded BUILD THIN PACK, NARROW AND REPEAT, or METHODOLOGY ONLY decision.
- The `eval-quality` methodology and governance layer under the same product name.

**Out during validation:**

- A new runner, provider adapter framework, assertion DSL, pass@k implementation, cost engine, reporting framework, CLI, or CI engine.
- Production API stabilization or npm publication.
- Hosted service, dashboard, or UI.
- Claims that any semantic capability is differentiated before it passes the gate.

## Vision

If validation succeeds, eval-quality becomes a reusable methodology and thin semantic evaluator pack that turns agent traces into defensible release evidence. Established engines remain underneath it. If validation fails, eval-quality still encodes how to select, generate, calibrate, interpret, and govern agent evals using those engines.

---

## Appendix: Downstream Depth
*(Vision and architecture context beyond the 1-2 page brief.)*

### Broader direction: one gate, many evidence producers (context, not v0)

The larger frame: AI-assisted delivery has an expensive-planning end and an expensive-validation end, with cheap code-gen in the middle. The validation-end story is a single ship/don't-ship gate fed by many evidence producers:

- unit tests
- contract tests
- E2E tests
- performance tests
- observability / production signals
- **agent-eval (eval-quality) ← the missing producer**

All producers emit evidence into one PASS / CONCERNS / FAIL gate. eval-quality is scoped to being *one* producer done well. Aggregating them is a direction, not a v0 build.

### Layered architecture

1. **Existing eval engine:** agent execution, trace ingestion, standard assertions, repeated runs, reports, and CI integration.
2. **eval-quality semantic evaluator pack:** only the custom evaluators that pass validation.
3. **eval-quality methodology and governance:** agents and rules that select, generate, calibrate, interpret, and gate the evals.

Layers two and three share the `eval-quality` name. Layer one remains an external dependency selected for capability and maintainability.

### Differentiator vs commodity matrix

| Capability | Position | Lead with it? |
| --- | --- | --- |
| Claim-to-evidence lineage | Unproven hypothesis | Validate as H1 |
| Semantic checkpoints over accepted paths or dependencies | Unproven hypothesis | Validate as H2 |
| Path quality (soundness/efficiency of path) | Largely commodity | Baseline input to H2 and H3 |
| Process and outcome separation | Commodity in isolation | Validate incremental value as H3 |
| First material error attribution | Unproven hypothesis with commercial competition | Validate as H4 |
| Tool-call correctness | Commodity | Support, don't pitch |
| Efficiency metrics | Commodity | Support, don't pitch |
| Holistic LLM-as-judge | Commodity | Support, don't pitch |
| Exact-match trajectory assertion | Anti-pattern (brittle) | **No - explicitly avoid as the lead** |
| Judging chain-of-thought / reasoning prose | Anti-pattern (CoT often unfaithful) | **No - explicitly avoid** |

### Rejected / anti-pattern rationale

- **Exact-match trajectory:** brittle, punishes valid alternate paths, and breaks on prompt changes. Semantic checkpoints are the candidate alternative and still require proof.
- **Judging reasoning prose:** research shows chain-of-thought is frequently unfaithful (stated reasoning ≠ actual computation), so judging the narrative rewards storytelling over behavior. Judge grounding/outcome/path instead of prose.

### Candidate evaluator pack

- Claim-to-evidence lineage.
- Semantic checkpoints over accepted paths or a dependency graph.
- Process and outcome separation.
- First material error attribution.
- A minimal trace-normalization shim when the selected engine requires one.

### Constraints (hard)

- Product name: `eval-quality` for the methodology and any validated evaluator pack.
- Engine code is reused through public extension points.
- Prototype code may use the language best supported by the selected engine. TypeScript and JavaScript consumers remain supported where the engine protocol permits them.
- Published original code remains Apache-2.0 with permissively licensed dependencies.
- No production scope is approved until the validation decision is recorded.

### Competitive landscape (research-grounded, 2025-2026)

- **Python-first (dominant):** DeepEval (de facto CI eval standard; ships agent primitives: task completion, tool correctness, step efficiency, plan adherence), Ragas (RAG faithfulness, output-level), OpenAI Evals (benchmark-style), Arize Phoenix (OTel tracing + evals). Research/benchmark: Inspect (UK AISI), TruLens, AgentBench/τ-bench/SWE-Bench. Commercial: Galileo, Patronus.
- **SaaS platforms:** LangSmith (LangChain/LangGraph-native), Langfuse (open-core), Braintrust (framework-agnostic, eval-first).
- **TypeScript-native (exists, but coupled):** several TS-native options ship evals (Mastra, Braintrust, LangSmith/AgentEvals, Langfuse), but each is bound to a hosted platform or a specific agent runtime. Mastra evals (`@mastra/core/evals`) is the strongest developer-facing case (gates + scorers, CI-integrated, provider-agnostic router), yet still **coupled to Mastra's agent runtime**. Vercel AI SDK has no real eval harness; LangGraph.js trails Python and is LangSmith-coupled.
- **Missed direct competitor:** `agentevals-dev/agentevals` is a local-first, framework-neutral Python engine with OpenTelemetry traces, golden eval sets, CI gates, reports, and language-neutral custom evaluators. It covers most of the original engine scope.
- **Takeaway:** reuse an existing engine. Test the semantic hypotheses against `agentevals-dev/agentevals` and Promptfoo before claiming a product wedge.

### Research validation (prior art supporting the differentiators)

- **TRACE** (arXiv 2510.02837) - reference-free trajectory eval via an accumulating per-step "evidence bank." Design prior for evidence-grounded per-step faithfulness, with two honest caveats: (a) TRACE grounds the agent's *thought* field (its hallucination axis), whereas eval-quality deliberately grounds *answer Claims* against observed Evidence and does **not** judge reasoning prose; (b) TRACE's own evaluation uses injected synthetic faults, exposed thought fields, and an LLM judge - it is a research prior, not production validation. eval-quality adapts the evidence-bank *idea*, not TRACE's thought-grounding target.
- **CORE** (arXiv 2509.20998) - models tasks as DFAs over tool calls; each prompt induces a *set* of valid reference paths. Strongest prior art for reference-*set* (order-tolerant) trajectory scoring. Precise: CORE scores by *normalized edit-distance partial credit* over the DFA-defined path set (its Kendall's-tau component stays order-sensitive), so it is path-set-aware, **not** literally path-invariant - the product term "path-invariant" is marketing shorthand for this.
- **Process Reward Models** (AgentPRM/InversePRM, arXiv 2502.10325) - step-level rewards vs reference policies (training-oriented).
- **CoT unfaithfulness**: Anthropic "Reasoning models don't always say what they think" (arXiv 2505.05410; decisive hints mentioned ~25-39% of the time); Oxford "Chain-of-Thought Is Not Explainability" (2025); BonaFide meta-eval (arXiv 2605.25052; existing CoT-faithfulness metrics near-chance). Together these justify NOT judging reasoning prose.

### Dogfooding targets

- `couture-cast` project.
- A BMAD skill's own evals.

### Current repo state (2026-07-22)

The repository is a TypeScript scaffold with a version constant and placeholder test. It has no evaluator behavior. The next approved artifact is the hypothesis experiment described in `HYPOTHESIS_VALIDATION_PLAN.md`.
