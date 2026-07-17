---
title: "Product Brief: eval-quality"
status: final
created: 2026-07-17
updated: 2026-07-17
---

# Product Brief: eval-quality

## Executive Summary

`eval-quality` is an open-source, provider-agnostic TypeScript library for evaluating AI agents. It drives an agent against a task, captures the full transcript, and scores the **trajectory**: the sequence of steps, tool calls, and claims the agent made to reach its answer. Its output is not a benchmark percentage; it is **evidence** for a ship/don't-ship decision (PASS / CONCERNS / FAIL).

The bet behind eval-quality is a claim about where value lives in AI-assisted delivery: **planning and validation are the expensive, high-value ends; code generation in the middle is cheap and getting cheaper.** Teams have mature evidence sources at the validation end for deterministic software: unit, contract, E2E, performance, observability. Agent behavior is the part of validation that stays hardest to pin down, because agents are non-deterministic and a right answer reached by luck is a different failure from a wrong answer reached by good reasoning on bad data. Output-only scoring cannot tell those apart.

eval-quality fills that gap as **one evidence producer** that plugs into the same gate as every other check. It leads with two things the rest of the market treats as afterthoughts: **evidence-grounded per-step faithfulness** (does each claim in the answer trace back to something the agent actually observed?) and **path-invariant reference-trajectory scoring** (did the agent take a sound path, without punishing it for taking a different-but-valid one?). This first release is deliberately scoped to the OSS engine; the broader "Evidence Platform" and the doctrine layer that wraps it are vision, not v0 build.

## The Problem

Agents are shipping into production faster than teams can build confidence in them. When a team asks "is this agent good enough to ship?", the honest answer today is usually a vibe check plus a benchmark score, and neither survives contact with reality:

- **Benchmark numbers hide the failure that matters.** "The agent scored 87%" says nothing about *why* the 13% failed, or whether the 87% was reached by sound reasoning or by luck. A gate needs to know the difference.
- **Output-only grading is blind to the trajectory.** An agent can fabricate a confident answer that happens to be right, or reach a wrong conclusion through correct reasoning on stale data. Grading only the final string treats these identically.
- **Exact-match trajectory checks are brittle.** Asserting the agent called exactly these tools in exactly this order punishes valid alternative paths and breaks on every prompt tweak, so teams abandon it.
- **Judging the reasoning prose is a trap.** Chain-of-thought is frequently *unfaithful* - the stated reasoning does not reflect the computation that produced the answer - so scoring the narrative rewards good storytelling, not good behavior.
- **The tooling is Python-first and often vendor-coupled.** TypeScript teams building agents get second-class eval tooling, and much of it is entangled with a specific model provider or a hosted platform.

The cost of the status quo: teams either ship agents on faith, or they over-invest in bespoke, unmaintainable eval harnesses. Either way, agent behavior stays outside the evidence-based release process that governs the rest of their software.

## The Solution

eval-quality is a harness plus a grading pipeline that turns an agent run into structured evidence.

- **Runner**: drives an agent against a task and captures the complete transcript (steps, tool calls, parameters, observations, final answer). The driver and the judge sit behind **adapters**, so eval-quality is provider-agnostic by construction (Anthropic first). Pre-flight checks classify connection/auth failures instead of surfacing a raw SDK crash.
- **`Expect.*` assertion DSL**: a typed, readable way to assert on tool calls, sequence, parameters, and budgets - the deterministic checks a gate can trust without a model in the loop. Sequence checks give partial credit for interleaved calls, not just exact order.
- **Deterministic-first, judge-second grader pipeline**: cheap, reproducible checks run first; an LLM judge is invoked only where genuine judgment is required, keeping cost and flake down. A crashed run skips judging entirely rather than scoring nothing as if it were something.
- **pass@k for non-determinism**: runs a task k times and aggregates with a pluggable pass rule (all attempts / any attempt / minimum rate), so a gate reasons about a distribution of behavior rather than a single lucky (or unlucky) draw.
- **Cost budget**: evaluation is a first-class cost center; a hard circuit breaker stops spend before the cap is exceeded, not just after, and every run reports actual spend.
- **Evidence reporting**: every run emits a human-readable summary and a machine-readable result designed to feed a PASS / CONCERNS / FAIL gate in CI; a broken judge is reported distinctly from a legitimately low score.
- **CLI**: thin `run` + `scaffold` wrapper over the same library, for non-interactive CI and Test Architect invocation without a TypeScript import.

**The differentiator - trajectory graders:**

- **Per-step faithfulness / grounding** - each claim in the answer is traced back to evidence the agent actually observed. Ungrounded claims are the signal.
- **Path quality** - was the path sound, efficient, and non-redundant?
- **Process-vs-outcome judging** - distinguishes "right answer, wrong reasons" from "wrong answer, sound process." The two-score split itself is becoming table-stakes across tools; eval-quality's edge is wiring it to the evidence-grounding above, not the split alone. (First-material-error attribution, the research's deeper whitespace here, is deliberately deferred past v0.)
- **Path-invariant reference-trajectory scoring** - compares against a reference trajectory by the *quality and soundness* of the path taken, not by exact step-for-step match, so valid alternate routes pass. "Invariant" is shorthand: scoring is order-tolerant over a *set* of accepted paths, not literally indifferent to order.

## What Makes This Different

eval-quality makes a deliberate, opinionated bet about *what to lead with* - validated by prior research into agent evaluation:

- **Lead with evidence-grounded per-step faithfulness.** Grounding claims in observed evidence is a real, hard, underserved problem. This is the headline.
- **Lead with path-invariant reference scoring.** Score the soundness of the path, not its literal shape. This is what makes trajectory eval usable instead of brittle.
- **Explicitly do NOT lead with brittle exact-match** trajectory assertions, and **do NOT judge reasoning prose** - because chain-of-thought is often unfaithful, judging it rewards narrative over behavior.
- **Commoditized, so present but not the pitch:** tool-call correctness, efficiency metrics, and holistic LLM-as-judge. eval-quality supports them; it does not pretend they are the moat.

**The honest landscape.** Mature eval tooling is overwhelmingly Python-first (DeepEval, Ragas, Arize Phoenix) or hosted SaaS (LangSmith, Langfuse, Braintrust). TypeScript-native options do exist (Mastra, Braintrust, LangSmith/AgentEvals, Langfuse), but each is bound to a hosted platform or a specific agent runtime; none is a standalone, framework-agnostic, CI-first OSS library. So no single component here is novel in isolation - trajectory tracing, tool-call correctness, and holistic judging are table stakes, and recent research establishes the hard parts as *tractable design priors, not production-proven*: TRACE (evidence-per-step, though it grounds the agent's *thought* field via injected synthetic faults), CORE (path/reference matching via a DFA-defined *set* of valid paths scored by edit-distance partial credit), and multiple 2025 papers showing chain-of-thought is unfaithful and its faithfulness metrics near-chance. These are starting points to adapt, not evidence the problem is solved - that gap is precisely what v0 has to close.

**What we borrow, not reinvent** - the commodity layer is adapted from proven, permissively-licensed patterns rather than built from scratch, so original design effort stays reserved for the two differentiators above: transcript normalization (Promptfoo, MIT), event/log/scorer separation (Inspect AI, MIT), sequence-matching modes (AgentEvals, MIT), typed assertion ergonomics (Braintrust SDK Apache-2.0 / Autoevals MIT), quick-check and tool-mock patterns (Mastra `@mastra/core`, Apache-2.0), grounding-evaluator shape (Phoenix's TypeScript evals package `@arizeai/phoenix-evals`, Apache-2.0 - not its Elastic-2.0 platform code), and score-object modeling (Langfuse core, MIT). Where a project is open-core (Mastra, Langfuse), the borrow is scoped to the permissive core package only, never each project's `ee/` enterprise tree. Patterns only, never code.

**The honest moat** is therefore the *combination*, not any one metric: (1) **TypeScript-native and framework-agnostic** - the real gap, since the serious TS options are platform- or runtime-bound; (2) the **trajectory-as-evidence framing** wired to a ship/don't-ship gate, leading with evidence-grounded faithfulness and path-invariant scoring rather than a metrics menu; and (3) the doctrine layer that will eventually wrap it (out of scope here). For v0, execution, language niche, and framing are the edge - stated plainly. The differentiators are well-supported by current research; the commodity metrics are not claimed as novel.

## Who This Serves

- **Primary: TypeScript teams shipping AI agents** who already run evidence-based release gates for their deterministic code and want agent behavior held to the same standard - not a dashboard vanity metric.
- **Primary: platform / QA / test-architect engineers** building the release gate itself, who need a machine-readable evidence artifact they can wire into CI alongside unit/contract/E2E/perf checks.
- **Secondary: agent framework authors and OSS contributors** who need a provider-agnostic eval layer that is not locked to one model vendor or hosted platform.
- **Secondary (early): the author's own projects** - `couture-cast` and a BMAD skill's own evals are the initial dogfooding targets.

Success for these users looks like: replacing "I think the agent is fine" with a defensible PASS / CONCERNS / FAIL backed by a trajectory they can inspect.

## Success Criteria

- **Dogfood proof:** eval-quality gates real agent behavior in the `couture-cast` project and evaluates a BMAD skill's own evals, producing PASS / CONCERNS / FAIL evidence that changed at least one real ship decision.
- **Faithfulness works as advertised:** on a curated set, per-step faithfulness grading reliably flags ungrounded claims that output-only grading misses.
- **Path-invariance holds:** reference-trajectory scoring passes valid alternate paths and fails genuinely bad ones - measurably less brittle than exact-match.
- **Provider-agnostic in practice:** swapping the driver/judge adapter requires no changes to task or grader code.
- **Adoptable:** a TypeScript developer can install the package, write and run a trajectory eval, and read the evidence report in under 30 minutes from the README.
- **Cost-honest:** every run reports its cost against a budget; deterministic-first keeps judge spend a minority of total.
- **Licensing:** Apache-2.0; dependencies permissively licensed, reviewable before release.

## Scope

**In (v0 - the OSS engine, this repo):**
- Provider-agnostic runner with adapter interface; Anthropic adapter first.
- `Expect.*` deterministic assertion DSL (tool calls, sequence, params, budgets).
- Deterministic-first / judge-second grader pipeline.
- Trajectory graders: per-step faithfulness/grounding, path quality, process-vs-outcome, path-invariant reference-trajectory scoring.
- pass@k aggregation; cost budgeting with a hard circuit breaker; human + machine-readable evidence reporting.
- CLI (`run` + `scaffold`) as a thin wrapper over the library, for CI and Test Architect invocation.
- TypeScript, Node >= 20, Apache-2.0, unscoped npm name `eval-quality`.

**Out (explicitly not v0):**
- The BMAD Test Architect "AE" workflow and certification - the doctrine/moat layer that *consumes* this engine.
- The full "Evidence Platform" (unifying unit/contract/E2E/perf/observability/agent-eval under one gate) - this is framing/vision, not a v0 deliverable.
- Additional provider adapters beyond Anthropic (interface exists; more adapters come later).
- Hosted service, dashboards, or UI.

## Vision

If it succeeds, eval-quality becomes the default way TypeScript teams produce **agent-behavior evidence** for a release gate - the missing check next to unit, contract, E2E, perf, and observability. It anchors an **Evidence Platform** where every quality signal, deterministic and non-deterministic alike, feeds one ship/don't-ship decision. On top of the engine sits a **Test Architect doctrine and certification** (the "AE" workflow) that turns "we run eval-quality" into "we evaluate agents *correctly*" - the durable moat. The engine is open and provider-agnostic on purpose: the value is the framing and the doctrine, not lock-in.

---

## Appendix: Downstream Depth
*(Vision and architecture context beyond the 1-2 page brief.)*

### The Evidence Platform (vision context, not v0)

The larger frame: AI-assisted delivery has an expensive-planning end and an expensive-validation end, with cheap code-gen in the middle. The Evidence Platform is the validation-end story - a single ship/don't-ship gate fed by many evidence producers:

- unit tests
- contract tests
- E2E tests
- performance tests
- observability / production signals
- **agent-eval (eval-quality) ← the missing producer this product supplies**

All producers emit evidence into one PASS / CONCERNS / FAIL gate. eval-quality is scoped to being *one* producer done exceptionally well. The platform itself is not a v0 build.

### Two-artifact architecture

1. **eval-quality (this repo) - the OSS engine.** Provider-agnostic. Apache-2.0. The mechanism. Scoped as THIS product / this PRD.
2. **BMAD Test Architect "AE" workflow + certification - the doctrine/moat.** The consuming layer that encodes *how to evaluate agents correctly* and certifies teams/agents. Out of scope for this PRD; noted so the engine's interfaces stay clean enough to be wrapped later.

Design implication for v0: keep the engine's outputs (evidence artifacts) and grader interfaces stable and composable, because a doctrine layer will orchestrate them.

### Differentiator vs commodity matrix

| Capability | Position | Lead with it? |
| --- | --- | --- |
| Per-step faithfulness / grounding (claims traced to evidence) | Differentiator | **Yes - headline** |
| Path-invariant reference-trajectory scoring (order-tolerant, path-set-aware) | Differentiator | **Yes - headline** |
| Path quality (soundness/efficiency of path) | Differentiator | Yes (supporting) |
| Process-vs-outcome judging | Commodity in isolation | Support - the two-score split is table-stakes; value is only in wiring it to grounding |
| First-material-error attribution / localization | Differentiator (research whitespace) | **Deferred past v0** - not built yet; candidate fast-follow FR |
| Tool-call correctness | Commodity | Support, don't pitch |
| Efficiency metrics | Commodity | Support, don't pitch |
| Holistic LLM-as-judge | Commodity | Support, don't pitch |
| Exact-match trajectory assertion | Anti-pattern (brittle) | **No - explicitly avoid as the lead** |
| Judging chain-of-thought / reasoning prose | Anti-pattern (CoT often unfaithful) | **No - explicitly avoid** |

### Rejected / anti-pattern rationale

- **Exact-match trajectory:** brittle, punishes valid alternate paths, breaks on prompt tweaks; teams abandon it. Path-invariant scoring is the answer.
- **Judging reasoning prose:** research shows chain-of-thought is frequently unfaithful (stated reasoning ≠ actual computation), so judging the narrative rewards storytelling over behavior. Judge grounding/outcome/path instead of prose.

### v0 engine components (for PRD elaboration)

- Runner: provider-agnostic driver + judge behind adapters; **Anthropic adapter first**; transcript capture.
- `Expect.*` DSL: tool calls, sequence, params, performance/cost budgets.
- Grader pipeline: deterministic-first, LLM-judge-second.
- Trajectory graders: faithfulness/grounding, path quality, process-vs-outcome, path-invariant reference scoring.
- pass@k aggregation for non-determinism.
- Cost budget accounting.
- Evidence reporting: human summary + machine-readable result for gates/CI.

### Constraints (hard)

- Language: TypeScript. Node >= 20.
- npm name: `eval-quality` (unscoped, neutral).
- License: Apache-2.0.
- **License:** Apache-2.0; dependencies permissively licensed.
- v0 scope tight and shippable; Evidence Platform = vision, not build.

### Competitive landscape (research-grounded, 2025-2026)

- **Python-first (dominant):** DeepEval (de facto CI eval standard; ships agent primitives: task completion, tool correctness, step efficiency, plan adherence), Ragas (RAG faithfulness, output-level), OpenAI Evals (benchmark-style), Arize Phoenix (OTel tracing + evals). Research/benchmark: Inspect (UK AISI), TruLens, AgentBench/τ-bench/SWE-Bench. Commercial: Galileo, Patronus.
- **SaaS platforms:** LangSmith (LangChain/LangGraph-native), Langfuse (open-core), Braintrust (framework-agnostic, eval-first).
- **TypeScript-native (exists, but coupled):** several TS-native options ship evals (Mastra, Braintrust, LangSmith/AgentEvals, Langfuse), but each is bound to a hosted platform or a specific agent runtime. Mastra evals (`@mastra/core/evals`) is the strongest developer-facing case (gates + scorers, CI-integrated, provider-agnostic router), yet still **coupled to Mastra's agent runtime**. Vercel AI SDK has no real eval harness; LangGraph.js trails Python and is LangSmith-coupled.
- **Takeaway:** trajectory eval is the 2025-26 theme, but mature options are Python or SaaS. eval-quality's wedge = TS-native + framework-agnostic + evidence-grounded per-step faithfulness + path-invariant reference matching, packaged as CI ship-gates. The metrics menu is commodity; the combination, language niche, and evidence-first (not prose-judging) framing are the differentiators.

### Research validation (prior art supporting the differentiators)

- **TRACE** (arXiv 2510.02837) - reference-free trajectory eval via an accumulating per-step "evidence bank." Design prior for evidence-grounded per-step faithfulness, with two honest caveats: (a) TRACE grounds the agent's *thought* field (its hallucination axis), whereas eval-quality deliberately grounds *answer Claims* against observed Evidence and does **not** judge reasoning prose; (b) TRACE's own evaluation uses injected synthetic faults, exposed thought fields, and an LLM judge - it is a research prior, not production validation. eval-quality adapts the evidence-bank *idea*, not TRACE's thought-grounding target.
- **CORE** (arXiv 2509.20998) - models tasks as DFAs over tool calls; each prompt induces a *set* of valid reference paths. Strongest prior art for reference-*set* (order-tolerant) trajectory scoring. Precise: CORE scores by *normalized edit-distance partial credit* over the DFA-defined path set (its Kendall's-tau component stays order-sensitive), so it is path-set-aware, **not** literally path-invariant - the product term "path-invariant" is marketing shorthand for this.
- **Process Reward Models** (AgentPRM/InversePRM, arXiv 2502.10325) - step-level rewards vs reference policies (training-oriented).
- **CoT unfaithfulness**: Anthropic "Reasoning models don't always say what they think" (arXiv 2505.05410; decisive hints mentioned ~25-39% of the time); Oxford "Chain-of-Thought Is Not Explainability" (2025); BonaFide meta-eval (arXiv 2605.25052; existing CoT-faithfulness metrics near-chance). Together these justify NOT judging reasoning prose.

### Dogfooding targets

- `couture-cast` project.
- A BMAD skill's own evals.

### Current repo state (2026-07-17)

Greenfield scaffold already in place: `package.json` (name `eval-quality`, Apache-2.0, Node >=20, ESM, Biome + Vitest + TS, husky/lint-staged), `src/index.ts` + one placeholder test, README/CONTRIBUTING/SECURITY/CoC, CI dir. No engine code yet; v0 is a build-from-scaffold effort.
