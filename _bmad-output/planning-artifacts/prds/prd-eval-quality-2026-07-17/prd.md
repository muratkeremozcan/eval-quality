---
title: eval-quality
status: final
created: 2026-07-17
updated: 2026-07-17
---

# PRD: eval-quality
*Working title - confirm.*

## 0. Document Purpose

This PRD is for the maintainer (Murat) and future OSS contributors of `eval-quality`, and for downstream BMAD workflows (architecture, epics/stories) that will build on it. It specifies **v0 of the OSS engine only** - the provider-agnostic TypeScript library that turns an agent run into ship/don't-ship evidence. It builds on the product brief (`../../briefs/brief-eval-quality-2026-07-17/brief.md`); vision-level material (the "Evidence Platform", the BMAD Test Architect "AE" doctrine/certification layer) lives there and is explicitly out of scope here. Vocabulary is Glossary-anchored; features are grouped with globally numbered FRs; assumptions are tagged inline and indexed in §9.

## 1. Vision

`eval-quality` is a provider-agnostic TypeScript library that drives an AI agent against a task, captures the full transcript, and scores the **Trajectory** as **Evidence** for a ship/don't-ship decision - not as a benchmark percentage. Its output is a Gate Verdict (PASS / CONCERNS / FAIL) backed by an inspectable, machine-readable Evidence Artifact.

The wager: in AI-assisted delivery, planning and validation are the expensive, high-value ends while code generation is cheap. Deterministic software already has evidence producers at the validation end (unit, contract, E2E, perf, observability); agent behavior is the missing one. eval-quality supplies it, and does so with an opinion: it leads with **evidence-grounded per-step faithfulness** (every Claim traced to observed Evidence) and **path-invariant reference-trajectory scoring** (sound paths pass even when they differ from the reference), while treating tool-call correctness, efficiency, and holistic LLM-judging as commodity support rather than the pitch.

For v0 the goal is a tight, shippable engine that a TypeScript developer can adopt from the README in under 30 minutes and use to gate real agent behavior - proven by dogfooding on the `couture-cast` project and a BMAD skill's own evals.

## 2. Target User

### 2.1 Jobs To Be Done

- As a **TypeScript engineer shipping an agent**, I need to answer "is this good enough to ship?" with defensible evidence, not a vibe check or a single benchmark number.
- As a **platform / QA / test-architect engineer**, I need a machine-readable agent-behavior evidence artifact I can wire into a CI gate next to my other checks.
- As an **agent/framework author**, I need an eval layer that is not locked to one model provider or a hosted platform.
- As **the maintainer**, I need an engine clean enough (stable grader/report interfaces) that a doctrine layer can later wrap it, and IP-clean enough to be unambiguously Apache-2.0.

### 2.2 Non-Users (v1)

- Teams wanting a hosted dashboard / SaaS eval product (no UI or service in v0).
- Non-TypeScript ecosystems needing a native SDK (Python-first tools already serve them).
- Teams wanting a leaderboard/benchmark score as the deliverable (eval-quality produces evidence, not rankings).

### 2.3 Key User Journeys

- **UJ-1. Dana gates an agent change in CI.** Dana, a TS engineer, opens a PR that tweaks her agent's prompt. CI runs an `eval-quality` suite: each task drives the agent through the Anthropic adapter, captures the Transcript, runs deterministic `Expect.*` assertions, then faithfulness and path-invariant graders, aggregated with pass@k. The run emits an Evidence Artifact and a Gate Verdict. The verdict is CONCERNS: two answers contained ungrounded Claims. Dana reads the human summary, sees exactly which Claims lacked Evidence, and fixes the prompt before merge.
- **UJ-2. Sam wires eval-quality into the release gate.** Sam, a test-architect, consumes the machine-readable Evidence Artifact from an eval-quality Run and maps PASS/CONCERNS/FAIL into the team's existing gate alongside unit/contract/E2E evidence - without eval-quality dictating the gate policy.
- **UJ-3. Murat dogfoods on couture-cast.** Murat writes reference trajectories for a handful of couture-cast agent tasks, runs eval-quality, and confirms path-invariant scoring passes a valid alternate path while failing a genuinely unsound one - changing a real ship decision.
- **UJ-4. Frankie swaps the provider.** Frankie, an agent-framework author, has an eval-quality suite running against the Anthropic Adapter and needs it provider-neutral. Frankie supplies a different Driver/Judge Adapter (or the test/fake Adapter) and re-runs the same Tasks, Assertions, and Graders unchanged - no task or grader code edits - confirming the eval layer is not locked to one model vendor. Exercises FR-2; validated by SM-5.

## 3. Glossary

- **Task**: A single unit of work given to an agent, plus the inputs/context needed to attempt it. The subject of evaluation.
- **Run**: One execution of the harness over a Task (or a suite of Tasks), producing a Transcript and graded results. May include multiple attempts (see pass@k).
- **Adapter**: A pluggable implementation of the driver and/or judge interface for a specific provider. Makes eval-quality provider-agnostic. **Anthropic** ships first.
- **Driver**: The Adapter role that executes the agent against a Task and yields a Transcript.
- **Judge**: The Adapter role that invokes an LLM to render a judgment for graders that require one.
- **Transcript**: The complete, structured capture of a Run: ordered Steps, Tool Calls (name, params), observations/results, and the final answer.
- **Step**: One discrete action in a Transcript (e.g., a Tool Call and its observation, or the final answer emission).
- **Tool Call**: An invocation the agent makes to an external capability, with a name and parameters.
- **Trajectory**: The path the agent took: the ordered sequence of Steps/Tool Calls leading to its answer. The primary object eval-quality scores.
- **Claim**: An assertion of fact in the agent's answer that can be checked against Evidence.
- **Evidence**: Something the agent actually observed during the Run (a Tool Call result, provided context) that a Claim can be traced to.
- **Assertion**: A deterministic check expressed via the `Expect.*` DSL over the Transcript (tool calls, sequence, params, budgets).
- **Grader**: A component that scores some aspect of a Run and emits a result. Deterministic graders run first; Judge-backed graders run second.
- **Faithfulness / Grounding**: A trajectory Grader: the degree to which each Claim in the answer traces to observed Evidence. Ungrounded Claims are the signal.
- **Path Quality**: A trajectory Grader: soundness, efficiency, and non-redundancy of the Trajectory.
- **Process-vs-Outcome**: Judging that distinguishes correctness of the answer from soundness of the Trajectory (e.g., "right answer, wrong reasons").
- **Reference Trajectory**: An author-provided exemplar Trajectory for a Task, used for comparison.
- **Path-Invariant Scoring**: Scoring a Trajectory against a Reference Trajectory by soundness/quality of the path rather than exact step-for-step match, so valid alternate paths pass. *"Invariant" is shorthand:* scoring is **order-tolerant over a set of accepted paths** (partial-order, predicate-matched checkpoints), not literally indifferent to order - see FR-7 and the CORE prior art in the Appendix.
- **pass@k**: Running a Task k times and aggregating results to reason about a distribution of behavior rather than one draw.
- **Cost Budget**: A per-Run (and/or per-Task) cost ceiling; the engine accounts spend against it and reports it.
- **Evidence Artifact**: The machine-readable result of a Run, designed to feed a gate/CI.
- **Evidence Report**: The human-readable summary of a Run.
- **Gate Verdict**: The ship/don't-ship signal derived from a Run: **PASS / CONCERNS / FAIL**.

## 4. Features

### 4.1 Provider-Agnostic Runner & Adapters

**Description:** The harness drives an agent against a Task and captures a complete, structured Transcript. Driver and Judge sit behind an Adapter interface so eval-quality is provider-agnostic by construction; the **Anthropic** Adapter ships first. Realizes UJ-1, UJ-3, UJ-4.

**Functional Requirements:**

#### FR-1: Drive a Task and capture a Transcript
A caller can run a Task through a configured Driver Adapter and receive a structured Transcript. Realizes UJ-1.

**Consequences (testable):**
- The Transcript contains ordered Steps, each Tool Call with its name and parameters, each observation/result, and the final answer, normalized to a provider-neutral core shape (Open Question 6, §8); one untyped `raw` passthrough field per Step carries provider-native detail that core Assertions/Graders never read.
- The Transcript is serializable to a stable, documented schema (JSON) and round-trips without loss.
- A Run with zero Tool Calls (direct answer) still produces a well-formed Transcript.

#### FR-2: Provider-agnostic Adapter interface
A caller can supply a Driver and/or Judge Adapter conforming to a public interface; swapping Adapters requires no change to Task, Assertion, or Grader code. Realizes UJ-4.

**Consequences (testable):**
- An Anthropic Adapter is provided and covered by tests (against a mock/recorded transport by default).
- A test/fake Adapter can be injected to run the full pipeline with no network calls.
- Adapter selection is explicit configuration; no provider SDK is imported at the library's top level / entry path unless that Adapter is used.
- Before the first Task runs, the Adapter performs a one-time connection/auth pre-flight check and fails fast with a classified error (e.g. certificate/TLS, network, auth, rate-limit, response-parse) rather than surfacing a raw SDK stack trace on the Nth Task of a suite. Directly protects SM-4 (time-to-first-eval); an opaque failure on first use is the most common way that metric gets missed.

**Out of Scope:**
- Additional provider Adapters beyond Anthropic (interface exists; more come later).

### 4.2 `Expect.*` Assertion DSL

**Description:** A typed, readable DSL for deterministic Assertions over a Transcript: tool calls, sequence, parameters, and performance/cost budgets. These are the checks a gate can trust without a model in the loop. Realizes UJ-1.

**Functional Requirements:**

#### FR-3: Assert on tool calls, sequence, and parameters
A caller can assert that specific Tool Calls occurred, in a required relative order, with matching parameters. Realizes UJ-1.

**Consequences (testable):**
- Assertions support presence ("tool X was called"), ordering (relative sequence), and parameter matching (exact and predicate-based).
- Sequence assertions score partial credit via longest-common-subsequence matching rather than requiring an exact contiguous match, so extra or interleaved Tool Calls the agent legitimately made don't fail an otherwise-correct sequence.
- A failed Assertion yields a structured, human-readable diff (expected vs. actual) in the result - not just a boolean.
- Assertions are pure functions of the Transcript (no side effects, deterministic).

#### FR-4: Assert on performance and cost budgets
A caller can assert that a Run stayed within a step-count, latency, and/or Cost Budget. Realizes UJ-1.

**Consequences (testable):**
- Exceeding a declared budget produces a failing Assertion with the measured value and the ceiling.
- Budget Assertions compose with the Cost Budget accounting in FR-8.

### 4.3 Grader Pipeline (Deterministic-First, Judge-Second)

**Description:** A pipeline that runs cheap, reproducible deterministic Graders first and invokes Judge-backed Graders only where genuine judgment is required, keeping cost and flake down. Realizes UJ-1.

**Functional Requirements:**

#### FR-5: Ordered deterministic-first, judge-second execution
A caller can compose a pipeline of Graders that executes deterministic Graders before Judge-backed Graders, with configurable short-circuiting on deterministic failure. Realizes UJ-1.

**Consequences (testable):**
- Given a deterministic failure with short-circuit enabled, no Judge (LLM) call is made.
- If the Driver produced no usable final answer (a Run-level crash, not merely a failed Assertion), Judge-backed Graders are skipped entirely and reported as skipped rather than scored; there is nothing for a Judge to evaluate, and it should not be charged for or scored as a low pass.
- Each Grader emits a structured result (id, score/verdict, rationale, evidence pointers) merged into the Run result.
- The pipeline accepts caller-supplied **commodity Graders** - a holistic LLM-as-judge Grader and an efficiency-metrics Grader ship in-box as opt-in, non-default Graders (tool-call correctness is already covered deterministically by FR-3). These are supported so the Non-Goals promise of "supported, not the pitch" is backed by a real interface, but none is on the default path.
- The pipeline is deterministic given the same Transcript and fixed Judge outputs (injectable for tests).

### 4.4 Trajectory Graders - the Differentiator

**Description:** The graders eval-quality leads with. They score the Trajectory as Evidence: per-step Faithfulness/Grounding, Path Quality, Process-vs-Outcome, and Path-Invariant reference scoring. eval-quality deliberately does **not** lead with brittle exact-match trajectory checks, and does **not** judge chain-of-thought / reasoning prose (it is frequently unfaithful). Realizes UJ-1, UJ-3.

**Functional Requirements:**

#### FR-6: Per-step Faithfulness / Grounding grader
A caller can grade whether each Claim in the agent's answer traces to observed Evidence in the Transcript. Realizes UJ-1.

**Consequences (testable):**
- The grader decomposes the answer into discrete Claims and, per Claim, emits grounded / ungrounded (or a graded score) with a pointer to the supporting Evidence when grounded.
- On a curated fixture where the answer contains a fabricated (ungrounded) Claim, the grader flags that Claim while an output-only equality check passes.
- The grader judges Claims against observed Evidence, not against the agent's stated reasoning narrative.

#### FR-7: Path-Invariant Reference-Trajectory grader
A caller can grade a Trajectory against an author-provided Reference Trajectory by path soundness/quality rather than exact step match. Also provides Path Quality and Process-vs-Outcome judgments. Realizes UJ-3.

**Scoring mechanism (v0) - so "soundness" is a computation, not a vibe:** the grade is produced in two explicit layers, not a single unspecified judge call. (1) A **deterministic checkpoint-satisfaction** layer over the Reference Trajectory's required Tool Calls: each checkpoint is predicate-matched (reusing the `Expect.tools.*` matcher vocabulary, Open Question 6, §8 item 3) and enforced as a *partial order* via the FR-3 LCS matcher, so extra, interleaved, or validly-reordered steps do not fail it. (2) A **Judge-scored soundness rubric** over the residual (unsound detours, redundancy, skipped preconditions), invoked only where the deterministic layer cannot decide. The reported path score is a graded [0,1] value combining the checkpoint-satisfaction ratio with the soundness rubric, and the two layers are reported **separately** so a consumer can tell a structural miss from a judgment call.

**Consequences (testable):**
- On a fixture, a valid alternate path that satisfies all required checkpoints in a valid partial order passes; a path that skips a required checkpoint or takes an unsound detour fails - where an exact-match check would fail both.
- The checkpoint-satisfaction layer is a pure, deterministic function of the Transcript + Reference Trajectory (same inputs → same ratio, every time); only the soundness rubric consults the Judge.
- Process-vs-Outcome is reported as two distinct signals (outcome correctness vs. path soundness), so "right answer / wrong reasons" is distinguishable from "wrong answer / sound process."
- Path Quality reports soundness/efficiency/redundancy signals over the Trajectory.

**Notes:**
- `[NOTE FOR PM]` Faithfulness and path-invariant graders depend on a Judge; their calibration (thresholds, prompt design) is a v0 quality risk. Fixtures must lock behavior. See Open Questions.

### 4.5 pass@k Aggregation

**Description:** Runs a Task k times and aggregates results so a Gate reasons about a distribution of behavior, not a single draw. Realizes UJ-1.

**Functional Requirements:**

#### FR-8a: Aggregate k attempts
*(FR-8a is intentionally sequenced before FR-8: pass@k was inserted after FR-8 already existed, and the suffix preserves stable FR IDs rather than renumbering. Read order is FR-8a then FR-8.)*

A caller can run a Task k times and receive an aggregated result (e.g., pass rate, per-attempt breakdown). Realizes UJ-1.

**Consequences (testable):**
- With k attempts, the result reports how many passed each Grader and an aggregate suitable for the Gate.
- The case-level pass criterion is pluggable (e.g. `all` attempts pass, `any` attempt passes, or a `minPassRate` threshold) rather than a single hard-coded aggregation rule, so "does this ever flake" and "is this reliable enough" can be asked separately.
- k=1 behaves identically to a single Run (no aggregation overhead changes the verdict).

### 4.6 Cost Budget Accounting

**Description:** Treats evaluation cost as first-class: each Run accounts spend and reports it against a budget, and can stop a Run before it overspends, not just report the overspend afterward. Realizes UJ-1.

**Functional Requirements:**

#### FR-8: Account and report Run cost against a budget
A caller can set a Cost Budget and have the Run report actual spend against it. Realizes UJ-1, UJ-2.

**Consequences (testable):**
- Every Run result includes a cost figure (token/where-available pricing) attributable to Driver and Judge calls.
- When a Cost Budget is set and exceeded, the result flags it (and can fail via FR-4).
- Independent of that reporting, the Cost Budget is enforced as a hard, charge-as-you-go circuit breaker: the next Driver/Judge call that would exceed the ceiling is refused *before* it is made, not detected after the spend already happened. A single runaway tool-loop therefore cannot blow a whole suite's budget, not just one Task's.
- The circuit breaker is shareable across Runs within one process/worker (e.g. a whole suite invocation), so the ceiling can be scoped per-Task or per-suite at the caller's discretion.

### 4.7 Evidence Reporting & Gate Verdict

**Description:** Every Run emits a human-readable Evidence Report and a machine-readable Evidence Artifact, and derives a Gate Verdict (PASS / CONCERNS / FAIL). Realizes UJ-1, UJ-2.

**Functional Requirements:**

#### FR-9: Emit human + machine-readable evidence
A Run produces both an Evidence Report (human summary) and an Evidence Artifact (machine-readable) covering Assertions, Grader results, pass@k aggregate, and cost. Realizes UJ-1, UJ-2.

**Consequences (testable):**
- The Evidence Artifact conforms to a documented, versioned schema and is consumable by a CI step without parsing human text.
- The Evidence Report cites, per finding, the Steps/Claims/Evidence involved (inspectable, not just a score).
- A Judge-backed Grader that returns an unparseable/malformed response is reported as a distinct `judge_error` status, never conflated with a low or zero score; a broken judge and a legitimately bad Trajectory must be visibly different failures.

#### FR-10: Derive a Gate Verdict
A Run derives a PASS / CONCERNS / FAIL Gate Verdict from configurable policy over Grader/Assertion results. Realizes UJ-2.

**Consequences (testable):**
- Verdict mapping is configurable by the consumer (eval-quality supplies a sensible default, does not hard-code gate policy).
- Default policy (Open Question 5, §8): FAIL on any deterministic Assertion failure or pass@k below `minPassRate`; CONCERNS if a Judge-backed Grader flagged an issue with no deterministic failure; PASS otherwise.
- The same results + same policy always yield the same Verdict (deterministic).

### 4.8 CLI

**Description:** A thin command-line wrapper over the library - not a second engine. Added primarily because CI and other headless runners need a non-interactive, shell-invokable entry point that returns an Evidence Artifact and a verdict-derived exit code without a caller first writing a TypeScript test file - the dominant v0 execution surface (§9). It secondarily unblocks the future BMAD Test Architect ("AE") doctrine layer (out of scope), which will orchestrate via shell invocation rather than a TS import. Library-first remains true: the CLI calls the same Run/Grader/Reporting functions the programmatic API calls. Realizes UJ-1, UJ-2.

**Functional Requirements:**

#### FR-11: Run an eval from the command line
A caller can invoke a CLI command to run a Task or suite and receive the Evidence Artifact plus a process exit code reflecting the Gate Verdict, without writing a test file first. Realizes UJ-1, UJ-2.

**Consequences (testable):**
- The command runs non-interactively by default (no prompts) - required for it to be callable headlessly from CI or from an orchestrated TEA workflow run.
- The exit code reflects the Gate Verdict (e.g. 0 on PASS, nonzero on CONCERNS/FAIL, configurable strictness); JSON and JUnit output are supported for CI consumption.
- Output conforms to the same versioned Evidence Artifact schema as FR-9; the CLI is a caller of the library, not an alternate implementation.

#### FR-12: Scaffold a new eval via CLI
A caller can run a scaffold/init command to generate a starter eval spec and Reference Trajectory stub for a Task. Realizes UJ-3.

**Consequences (testable):**
- Generates a minimal, typed eval file plus a Reference Trajectory stub in the documented authoring format (ties to Open Question 3).
- Designed as the shell-out target for the BMAD "AE" workflow's own scaffolding step (generate eval spec + reference-trajectory stub + rubric template per skill) rather than requiring TEA to hand-generate TypeScript boilerplate itself.

**Notes:**
- `[ASSUMPTION]` The CLI ships in v0 (this reverses the earlier "library-first, no CLI" assumption - see §9).

## 5. Non-Goals (Explicit)

- Not a hosted service, dashboard, or GUI.
- Not a benchmark/leaderboard producer; no "the agent got 87%" as the deliverable.
- Not the BMAD Test Architect "AE" workflow or certification (the consuming doctrine layer) - out of scope, informs interface stability only.
- Not the full "Evidence Platform" - that is vision, not a v0 build.
- Not leading on commodity graders (tool-call correctness, efficiency, holistic LLM-judge) - supported as opt-in, non-default Graders in the FR-5 pipeline, not the pitch.
- Not first-material-error attribution / first-error localization in v0. The market research names this as genuine whitespace, so it is a deliberate deferral (candidate fast-follow FR), *not* an oversight. v0's process-vs-outcome signal (FR-7) reports the two scores but does not localize the first failing Step.
- Not judging chain-of-thought / reasoning prose as a scored signal.
- Not shipping provider Adapters beyond Anthropic in v0.
- Not a rich or interactive CLI - run + scaffold only, non-interactive by default, no TUI, no config wizard.

## 6. MVP Scope

### 6.1 In Scope
- Provider-agnostic Runner + Adapter interface; Anthropic Adapter; Transcript capture; connection/auth pre-flight with classified errors (FR-1, FR-2).
- `Expect.*` deterministic Assertion DSL incl. partial-credit sequence matching and budgets (FR-3, FR-4).
- Deterministic-first / Judge-second Grader pipeline, incl. skip-not-score on Driver crash (FR-5).
- Trajectory graders: Faithfulness/Grounding + Path-Invariant reference (with Path Quality, Process-vs-Outcome) (FR-6, FR-7).
- pass@k aggregation with pluggable pass criterion (FR-8a); Cost Budget accounting incl. hard charge-before-spend circuit breaker (FR-8).
- Human + machine-readable evidence + Gate Verdict, incl. distinct judge-error status (FR-9, FR-10).
- CLI: run + scaffold, wrapping the same library API (FR-11, FR-12).
- TypeScript, Node >= 20, ESM, Apache-2.0, unscoped npm `eval-quality`.

### 6.2 Out of Scope for MVP
- Additional provider Adapters (OpenAI, Google, etc.) - interface ready; deferred to post-v0.
- BMAD "AE" doctrine layer (out of scope) - keep engine interfaces stable and wrappable so it can consume the engine later.
- Evidence Platform aggregation across other test types - vision.
- Hosted UI / reporting service.

## 7. Success Metrics

**Primary**
- **SM-1**: Dogfood adoption - eval-quality gates real agent behavior in `couture-cast` and evaluates a BMAD skill's own evals, producing a Gate Verdict that changes ≥1 real ship decision. Validates FR-1, FR-6, FR-7, FR-9, FR-10.
- **SM-2**: Faithfulness efficacy - on a curated fixture set, the Faithfulness grader flags ≥90% of injected ungrounded Claims that output-only checks miss. Validates FR-6.
- **SM-3**: Path-invariance efficacy - reference-trajectory grading passes valid alternate paths and fails unsound ones on the fixture set with 0 false-fails on the "valid alternate" cases. Validates FR-7.

**Secondary**
- **SM-4**: Time-to-first-eval - a new TS dev writes and runs a trajectory eval from the README in < 30 min, via either the library or the CLI. Validates FR-1..FR-12 (adoptability).
- **SM-5**: Provider-agnosticism - swapping to a fake Adapter runs the full pipeline with zero network calls in tests. Validates FR-2.
- **SM-6**: Cost honesty - deterministic-first keeps Judge spend a minority of total cost on the dogfood suite. Validates FR-5, FR-8.

**Counter-metrics (do not optimize)**
- **SM-C1**: Do not maximize number of graders/metrics - counterbalances SM-2/SM-3; more signals that aren't evidence-grounded dilute the differentiator.
- **SM-C2**: Do not chase pass rate on the gate - counterbalances SM-1; a gate tuned to always PASS is worthless. Track that CONCERNS/FAIL fire when warranted.

## 8. Open Questions - Resolved

All six were open at draft time; all six are now decided defaults (v0 ships these, consumer-overridable where noted). None are runtime-tunable config surfaces in v0 unless stated.

1. **Judge calibration.** The Judge is pinned to an exact model version, never "latest," and must never be the same or a weaker tier than the Driver under test (no self-judging). Judge prompts and pass/fail thresholds are versioned fixtures in-repo; a Judge/prompt change only ships if it doesn't regress the calibration fixture set already required for SM-2/SM-3. Thresholds are an empirically-set default, not a v0 config surface.
2. **Claim decomposition.** Hybrid, biased to rule-based: deterministic sentence/clause segmentation produces the claim list (unit-testable - same answer string, same claims, every time); the Judge is invoked only to decide grounded/ungrounded per already-segmented Claim, never to do the segmentation itself. Full judge-based decomposition is non-deterministic and rejected for v0.
3. **Reference Trajectory authoring format.** A plain typed TypeScript object, not a DSL or YAML: an ordered checkpoint list (required Tool Calls with predicate-matchable params, reusing the `Expect.tools.*` matcher vocabulary) plus the expected final-answer shape - not a full recorded transcript. Bar: authoring one must take less effort than the equivalent output-only Assertion.
4. **Evidence Artifact schema versioning.** A single `schemaVersion` integer on the Evidence Artifact, independent of package SemVer. Additive fields bump nothing consumer-visible; breaking shape changes bump `schemaVersion`, and the library refuses to silently emit an old-shape artifact under a new version. No deprecation-window policy yet - one producer, no second real consumer to protect until the BMAD "AE" doctrine layer exists.
5. **Default Gate Verdict policy.** FAIL on any deterministic Assertion failure or pass@k below the caller's `minPassRate`; CONCERNS if a Judge-backed Grader (Faithfulness, Path-Invariant) flagged an issue with no deterministic failure; PASS otherwise. Deterministic failures are hard; Judge-backed findings are a flag, not a block. Ships as the literal, documented default function (FR-10) - no hidden weighting - and remains consumer-overridable.
6. **Transcript schema.** Normalize to the provider-neutral core every Assertion/Grader reads (ordered Steps, Tool Call name/params, result, final answer, cost), plus one untyped `raw` passthrough field per Step for provider-native detail. Core Assertions/Graders never read `raw` - preserves neutrality without discarding data a future Adapter might need.

## 9. Assumptions Index

- §1/§2 - [ASSUMPTION] Primary users are TS agent teams + platform/test-architect engineers; secondary are framework authors and the maintainer's own projects.
- §2.3 - [ASSUMPTION] CI is the dominant execution surface for v0 (vs. interactive/local-only).
- §4.1 FR-2 - [ASSUMPTION] Provider SDKs are loaded lazily per-Adapter so the core stays dependency-light and provider-neutral.
- §4.4 FR-6 - [ASSUMPTION] Faithfulness is scored against observed Evidence, and reasoning-prose judging is intentionally excluded.
- §4.4 FR-7 - [ASSUMPTION] Reference Trajectories are author-provided per Task (not auto-derived) in v0.
- §6 - [ASSUMPTION] "Shippable v0" = the FR set above with fixtures, not a broader feature surface.
- §4.8 FR-11/FR-12 - [ASSUMPTION] A thin CLI ships in v0 as a wrapper over the library (reverses the prior "library-first, no CLI" assumption - driven first by CI/headless runners needing a shell-invokable, non-interactive entry point rather than a TypeScript import, and secondarily by the future TEA layer having the same need).

---

## Developer-Product Requirements

*This is a library; the following cross-cutting requirements apply.*

### API Contracts / Public Surface
- Public surface: `Expect.*` DSL, Adapter interface, Grader/pipeline API, Run/Task entry point, Evidence Artifact/Report types, Gate Verdict, and the CLI (run + scaffold commands, their flags, and their exit-code contract). These are the stable contract.
- The Evidence Artifact is a versioned schema (see Open Question 4); breaking changes to it, the Adapter interface, or the CLI's command/flag/exit-code contract follow the versioning policy below.
- Types are exported from the package entry (`dist/index.d.ts`); the engine is usable programmatically, and the CLI (FR-11, FR-12) is a thin wrapper over that same programmatic surface, not a second implementation.

### Versioning & Deprecation Policy
- SemVer. Pre-1.0 (0.x): minor may break, documented in CHANGELOG; the Adapter interface, Evidence Artifact schema, and CLI command/flag/exit-code contract are called out on every change.
- Evidence Artifact carries a schema version field so CI consumers can detect incompatibility.

### Performance Budgets
- Deterministic-first: Judge (LLM) calls are a minority of total cost/time on the dogfood suite (SM-6).
- pass@k must not multiply Judge cost unnecessarily; deterministic short-circuit applies per attempt.

### Language / Runtime Targets & Dependency Policy
- TypeScript, Node >= 20, ESM (`"type": "module"`).
- Apache-2.0; unscoped npm name `eval-quality`.
- **Dependencies:** permissively licensed only (MIT / Apache-2.0); no copyleft or proprietary dependencies.
- Core dependency-light; provider SDKs pulled in only by their Adapter, not the core entry path.

### Constraints & Guardrails
- **Cost:** every Run is cost-accounted and budgetable (FR-4, FR-8); deterministic-first minimizes spend.
- **Licensing:** Apache-2.0; all dependencies permissively licensed and reviewable before release.
- **Safety:** Judge prompts and grader logic must not leak Task secrets into logs/artifacts beyond what the Evidence Report needs. `[NOTE FOR PM]` confirm redaction expectations.

---

## Appendix: Downstream Depth (architecture / rationale)
*(Depth that supports architecture and epics, beyond the PRD body.)*

### Rejected alternatives (rationale)

- **Exact-match trajectory assertion as the lead signal** - rejected: brittle, punishes valid alternate paths, breaks on prompt tweaks; teams abandon it. Kept only as an optional deterministic `Expect.*` capability, never the headline. Path-invariant reference scoring (FR-7) is the answer.
- **Judging chain-of-thought / reasoning prose** - rejected as a scored signal: research indicates CoT is frequently unfaithful (stated reasoning ≠ actual computation), so scoring the narrative rewards storytelling over behavior. Judge grounding/outcome/path instead (FR-6, FR-7).
- **Holistic single LLM-judge score as the product** - rejected as the pitch: commodity, low-signal for gates. Supported inside the pipeline, not led with.
- **Benchmark/percentage output** - rejected: the deliverable is evidence + Gate Verdict, not a ranking.

### Prior art (grounds FR-6 / FR-7 and the no-CoT-judging decision)

- **FR-6 (per-step faithfulness / evidence bank):** TRACE (arXiv 2510.02837) - reference-free trajectory eval accumulating evidence per step. *Adaptation, not adoption:* TRACE grounds the agent's *thought* field (its hallucination axis); eval-quality grounds *answer Claims* against observed Evidence and deliberately does not judge reasoning prose. TRACE is a research prior (injected synthetic faults, exposed thought fields, LLM judge), not production validation - v0 borrows the evidence-bank idea, not the target.
- **FR-7 (path-invariant reference scoring):** CORE (arXiv 2509.20998) - tasks as DFAs over tool calls; a prompt induces a *set* of valid reference paths (skipped preconditions / reordered calls that final-state checks miss). *Precise:* CORE scores by normalized edit-distance partial credit over that DFA-defined path set and stays order-sensitive (Kendall's-tau component), i.e. reference-*set*-aware, not literally path-invariant; and its authors flag the DFA abstraction as a scaling bottleneck. Also AgentPRM/InversePRM (arXiv 2502.10325) for step-level reward vs reference policy. *Prior-art note:* the market research grounded this wedge on AgentLens (arXiv 2605.12925, Prefix-Tree-Acceptor references) with the same "design-prior-not-production-proven" caveat; CORE is the sharper mechanism prior - both are priors to adapt, not solved methods.
- **No CoT-prose judging:** Anthropic 2505.05410, Oxford "CoT Is Not Explainability" (2025), BonaFide 2605.25052 (CoT-faithfulness metrics near-chance).
- **Landscape:** commodity primitives (tool-call correctness, step efficiency, holistic judge) are DeepEval/Mastra-standard; the strongest TS-native incumbent (Mastra evals) is coupled to its own runtime and the other TS options are platform-bound; eval-quality's framework-agnostic + evidence-first + path-invariant combination is the wedge.

### Mechanism / transport decisions (for architecture)

- **Adapter boundary** splits Driver (executes agent → Transcript) from Judge (LLM judgment for graders). Provider SDKs are lazy-loaded per Adapter so the core entry path imports no provider SDK. Anthropic first.
- **Transcript** is the single source of truth: a serializable, versioned, provider-neutral schema. All Assertions and Graders are pure functions of the Transcript (deterministic, testable with injected/fake Judge outputs).
- **Grader pipeline** ordering: deterministic graders first with configurable short-circuit; Judge-backed graders second. Enables cost control (SM-6) and network-free testing (SM-5). Judge-backed graders are additionally skipped (not scored) when the Transcript shows a Driver-level crash with no usable final answer.
- **Evidence Artifact** carries a schema version; Gate Verdict policy is consumer-configurable with an opinion-light default.
- **Faithfulness pipeline**: answer → Claim decomposition → per-Claim grounding check against observed Evidence with Evidence pointers. Claim-decomposition strategy (rule/judge/hybrid) is Open Question 2.
- **Path-invariant scoring** (FR-7): two explicit layers, not one judge call. (1) Deterministic checkpoint-satisfaction over the Reference Trajectory's required Tool Calls, predicate-matched via the `Expect.tools.*` vocabulary and enforced as a partial order via FR-3 LCS (extra/interleaved/validly-reordered steps do not fail it); (2) a Judge-scored soundness rubric over the residual, invoked only where the deterministic layer cannot decide. Reported path score is a graded [0,1] combining the checkpoint ratio and the rubric, with the two layers surfaced separately (structural miss vs judgment call). Emit Process-vs-Outcome as two distinct signals. Compare candidate to Reference on soundness/quality, not literal step match.
- **Cost circuit breaker (FR-8)**: a stateful, shareable accounting object (spent/remaining/cap) that refuses the next Driver/Judge call *before* it would exceed the cap - charge-as-you-go, not measure-after. Distinct from and complementary to the soft `Expect.performance.maxCostUsd`-style assertion, which only detects overspend post-hoc. Scope the object per-process/worker so it can cover a whole suite invocation, not just one Task.
- **Connection/auth pre-flight (FR-2)**: one cached check per Adapter per process, classifying failure into a small fixed set (tls/connection/auth/rate_limit/parse) with targeted remediation text per class - turns an opaque SDK exception into an actionable message on first use. If any TLS relaxation is ever needed for a corporate-proxy or self-hosted-gateway use case, scope it to a per-client transport option passed to that Adapter's SDK client, never a process-global TLS setting; a global relaxation silently weakens unrelated HTTPS traffic (Judge calls, telemetry, etc.), a per-client one doesn't.
- **Partial-credit sequence matching (FR-3)**: longest-common-subsequence over the expected vs. observed Tool Call sequence, so extra or interleaved calls don't fail an otherwise-valid ordering the way an exact/contiguous match would.
- **Judge error as a distinct status (FR-9)**: an unparseable/malformed Judge response is a `judge_error` enum value in the Grader result, never coerced into a numeric low score - keeps "the judge broke" visibly distinct from "the agent did badly" in the Evidence Artifact.
- **Judge tier discipline (Open Question 1)**: the Judge Adapter must default to a model tier at or above the Driver's, never below; self-judging (or under-judging) is a calibration risk, not just a cost optimization.

### Interface-stability rationale (why it matters for v0)

The out-of-scope BMAD "AE" doctrine/certification layer will *consume* this engine. To keep the engine wrappable later without churn, v0 must stabilize: Adapter interface, Grader/pipeline API, Evidence Artifact schema, and Gate Verdict shape. This is why versioning/schema-version fields are v0 requirements even though there is one consumer today.

### Testing / fixtures (dogfood + efficacy)

- Faithfulness efficacy (SM-2) and path-invariance efficacy (SM-3) require curated fixtures: transcripts with injected ungrounded Claims, and pairs of (valid-alternate-path, unsound-path) against a reference. These fixtures lock grader behavior against model drift (Open Question 1).
- Dogfood targets: `couture-cast` agent tasks + a BMAD skill's own evals (SM-1).

### Additional ideas considered

Beyond the mechanisms specified above, v0 deliberately favors portable, deterministic, versioned approaches over deployment-coupled configuration and brittle, unmaintainable alternatives - standard engineering hygiene, called out so the design choices are explicit rather than incidental.

### Deferred (post-v0)

- Additional provider Adapters (OpenAI, Google, local/OSS models).
- Evidence Platform aggregation across unit/contract/E2E/perf/observability.
- BMAD "AE" workflow + certification (the moat/doctrine layer).
- Anything beyond CLI run + scaffold (interactive mode, TUI, config wizard, additional subcommands).
