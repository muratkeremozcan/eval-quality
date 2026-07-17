---
stepsCompleted: [1, 2, 3, 4, 5, 6]
inputDocuments: []
workflowType: 'research'
lastStep: 6
research_type: 'market'
research_topic: 'eval-quality - an open-source agent-evaluation library focused on reasoning trajectories'
research_goals: 'Validate the supplied market thesis; complete the OSS-versus-commercial tooling landscape; identify reusable open-source capabilities and improvement opportunities; estimate TAM and priority segments; produce a competitor matrix, positioning statement, and differentiated wedge; assess vendor-neutral methodology and certification as a moat.'
user_name: 'Murat.ozcan'
date: '2026-07-17'
web_research_enabled: true
source_verification: true
---

# Research Report: market

**Date:** 2026-07-17
**Author:** Murat.ozcan
**Research Type:** market

---

## Research Overview

This report evaluates the market for **eval-quality**, a proposed open-source agent-evaluation library centered on observable execution trajectories rather than final outputs alone. Research covered customer behavior, pain points, buying processes, fifteen OSS and commercial competitors, TypeScript maturity, adjacent market sizing, methodology and certification whitespace, and the role of eval-quality as one evidence producer within a larger Evidence Platform. Primary product documentation, repositories, licenses, standards, research papers, current surveys, and official organizational sources were prioritized.

The research validates the core problem but narrows the differentiation. Tool-call correctness, efficiency, holistic trajectory judging, grounding metrics, and even basic process/outcome separation are already available across several products. The defensible wedge is a portable combination that remains under-served: **claim-to-evidence lineage across observable steps, path-invariant semantic checkpoints and constraints, independent process-versus-outcome classification, first-material-error attribution, and calibrated deterministic-first evaluation**. Chain-of-thought must not be treated as faithful access to a model's internal reasoning.

The recommended strategy is developer-led OSS adoption among TypeScript agent teams, followed by a managed Evidence Platform for shared evidence, governance, and audit workflows. A vendor-neutral methodology can become a durable moat, but certification should follow demonstrated employer demand and independent governance. See **Research Synthesis: Executive Summary and Strategy** near the end of this document for the integrated TAM, positioning, roadmap, and recommendations.

---

<!-- Content will be appended sequentially through research workflow steps -->

# Market Research: eval-quality

## Research Initialization

### Research Understanding Confirmed

**Topic**: eval-quality - an open-source agent-evaluation library focused on reasoning trajectories  
**Goals**: Validate the supplied market thesis; complete the OSS-versus-commercial tooling landscape; identify reusable open-source capabilities and improvement opportunities; estimate TAM and priority segments; produce a competitor matrix, positioning statement, and differentiated wedge; assess vendor-neutral methodology and certification as a moat.  
**Research Type**: Market Research  
**Date**: 2026-07-17

### Research Scope

**Market Analysis Focus Areas:**

- Validate commoditized evaluation capabilities and the proposed open frontier
- Assess chain-of-thought faithfulness risks and implications for product design
- Compare promptfoo, DeepEval/Confident AI, Ragas, OpenAI Evals, Inspect AI, Braintrust, LangSmith, Arize Phoenix, Langfuse, Galileo, Mastra, Vercel AI SDK evals, Humanloop, Patronus, and W&B Weave
- Classify OSS versus commercial models, implementation languages, evaluation coverage, and positioning
- Identify open-source components and patterns eval-quality can reuse or improve
- Estimate TAM using transparent assumptions and define priority customer/user segments
- Analyze methodology, training, and certification whitespace
- Develop the competitor matrix, positioning statement, differentiated wedge, and Evidence Platform role

**Research Methodology:**

- Current web data with direct source verification
- Primary sources prioritized; independent corroboration for critical market claims
- Explicit separation of verified fact, inference, and strategic recommendation
- Confidence levels and caveats for uncertain market-size estimates
- Global scope, with geographic distinctions where evidence supports them

### Next Steps

**Research Workflow:**

1. ✅ Initialization and scope setting (current step)
2. Customer and user segment analysis
3. Competitive landscape and open-source borrowing analysis
4. Strategic synthesis, TAM, positioning, and recommendations

**Research Status**: Scope confirmed, ready for detailed research

Scope confirmed by user on 2026-07-17.

## Customer Behavior and Segments

### Customer Behavior Patterns

The market is moving from informal inspection toward repeatable evaluation, but adoption still trails observability. In LangChain's November–December 2025 survey of 1,340 practitioners, 57.3% reported production agents, 89% had some observability, 52.4% ran offline evaluations, and 37.3% ran online evaluations. Human review remained common at 59.8%, alongside LLM-as-judge at 53.3%. This creates a clear adoption sequence: teams first instrument traces, then add offline regression tests after quality failures, and later add online evaluation when agents face real users. The survey is useful directional evidence but overrepresents technology firms (63%) and organizations under 100 employees (49%). [LangChain, “State of Agent Engineering,” 2026](https://www.langchain.com/state-of-agent-engineering)

_Behavior Drivers:_ Quality is the primary trigger: 32% named it the top production barrier. Teams need to catch regressions, compare models, debug nondeterministic behavior, and justify greater agent autonomy. Security becomes more prominent in enterprises with 2,000+ employees, where 24.9% cited it as a leading concern.  
_Interaction Preferences:_ Engineering-led users favor code-first, local/offline tests, reusable datasets, CI gates, trace drill-down, and a mix of deterministic graders, LLM judges, and calibrated human review.  
_Decision Habits:_ Developers commonly try OSS or low-friction tools before centralized procurement. Menlo Ventures describes AI developer-tool adoption as bottom-up: individuals prove value in daily work, creating demand that later converts into enterprise contracts. [Menlo Ventures, “State of Generative AI in the Enterprise,” 2025](https://menlovc.com/perspective/2025-the-state-of-generative-ai-in-the-enterprise/)

### Demographic Segmentation

For this B2B developer market, role, stack, organization size, deployment maturity, and risk profile are more predictive than age or household income.

| Dimension | Segment | Relevant behavior |
|---|---|---|
| Role | AI/ML and application engineers | Adopt code-first libraries to debug failures and gate releases |
| Role | Evaluation/platform teams | Standardize datasets, graders, trace schemas, and release policies across teams |
| Role | Engineering/product leaders | Sponsor tooling when customer failures, review cost, or release risk becomes material |
| Role | Governance, model-risk, and compliance teams | Require auditability, reproducibility, policy checks, privacy controls, and human oversight |
| Organization | Startups and small teams | Optimize for speed, low setup cost, local execution, and actionable debugging |
| Organization | Mid-market and enterprises | Prioritize integration, security, RBAC/SSO, data residency, support, and organization-wide reporting |
| Maturity | Prototype teams | Rely on manual review and ad hoc scripts; weak immediate willingness to pay |
| Maturity | Production teams | Have the strongest need for regression suites, online feedback, and failure attribution |

The TypeScript segment is strategically attractive because TypeScript is widely used by professional developers, yet agent-evaluation tooling remains disproportionately Python-centric. Stack Overflow's 2025 survey reports high AI-tool use but persistent distrust of output accuracy, supporting test-like evaluation workflows rather than blind automation. [Stack Overflow Developer Survey, 2025](https://survey.stackoverflow.co/2025/ai) [Stack Overflow Technology Survey, 2025](https://survey.stackoverflow.co/2025/technology)

_Geographic Distribution:_ The OSS engine should be global and self-hostable. Regulated European deployments add demand for AI literacy, logging, governance, and evidence, while data-residency and sovereignty concerns also affect global enterprises.  
_Education and Skill Profile:_ Initial users are technically advanced engineers and researchers; broader adoption requires examples, methodology, and training that translate ambiguous business expectations into testable criteria.

### Psychographic Profiles

**Pragmatic builders** value fast feedback, test ergonomics, transparent implementation, and freedom from provider lock-in. They will reject abstract “AI quality” claims unless the library catches failures their current tests miss.

**Reliability owners** value repeatability, comparable results, coverage, and root-cause evidence. Their frustration is not lack of traces but the inability to determine whether an observed path was valid, grounded, safe, and robust across alternate successful paths.

**Governance-oriented buyers** value defensibility: who approved the rubric, which evidence supports each step, what changed between versions, and whether results can be reproduced. They are wary of sending sensitive trajectories to another SaaS provider.

**Researchers and methodologists** value raw data, custom metrics, statistical rigor, benchmark portability, and transparent assumptions. They are natural contributors but may resist product constraints that obscure experimental detail.

_Values and Beliefs:_ Openness, auditability, model neutrality, path flexibility, and evidence over opaque judge scores.  
_Attitudes:_ Skeptical of exact “golden path” matching and uncalibrated LLM-as-judge claims; receptive to a framework that separates process quality from outcome quality.  
_Source:_ [Anthropic, “Demystifying evals for AI agents,” 2026](https://www.anthropic.com/engineering/demystifying-evals-for-ai-agents) and [Yehudai et al., “A Survey on the Evaluation of Large Language Model-Based Agents,” 2025](https://arxiv.org/abs/2503.16416)

### Customer Segment Profiles

#### Segment 1 - TypeScript AI product teams: beachhead

- **Profile:** Small-to-mid-sized teams building Node.js, Next.js, or TypeScript agents, often already emitting traces.
- **Trigger:** A prompt, model, or tool change causes an “almost right” regression that output-only tests cannot explain.
- **Job to be done:** Add Vitest/Jest-like trajectory regression tests without introducing a separate Python evaluation stack.
- **Selection criteria:** npm-native API, strong types, local execution, CI thresholds, adapters for OpenTelemetry and major agent SDKs, provider-neutral exports.
- **Commercial behavior:** Adopt OSS first; pay later for collaboration, hosted evidence, governance, or support.
- **Confidence:** Medium-high. TypeScript prevalence is verified; willingness to pay for trajectory-specific evaluation still needs interviews.

#### Segment 2 - Central evaluation and AI platform teams: expansion buyer

- **Profile:** Teams supporting multiple agent applications, models, frameworks, and business units.
- **Trigger:** Duplicated graders, inconsistent rubrics, growing review costs, or production traces that cannot be converted into systematic regression evidence.
- **Job to be done:** Standardize process and outcome evaluation while preserving framework and vendor neutrality.
- **Selection criteria:** stable schemas, plugin architecture, versioned datasets/rubrics, batch execution, cross-run analysis, human calibration, cost controls, and enterprise integration.
- **Commercial behavior:** Will sponsor a managed Evidence Platform after the OSS engine proves technical fit and internal adoption.
- **Confidence:** High on need; medium on budget ownership.

#### Segment 3 - Regulated and high-consequence enterprises

- **Profile:** Financial services, healthcare, public-sector, and customer-facing automation teams.
- **Trigger:** Model-risk review, audit requirements, customer harm, security review, or increased agent autonomy.
- **Job to be done:** Demonstrate that both intermediate actions and final outcomes follow policy and are supported by evidence.
- **Selection criteria:** self-hosting, retention/redaction controls, deterministic policy checks, reproducible reports, version history, human approvals, and data residency.
- **Commercial behavior:** Longer procurement; higher support and assurance requirements; methodology credibility matters as much as code.
- **Confidence:** Medium-high. The risk drivers are clear, but use-case-specific requirements vary.

#### Segment 4 - Researchers, consultancies, and training partners

- **Profile:** Benchmark authors, safety researchers, AI consultancies, internal enablement teams, and professional educators.
- **Trigger:** Need to compare agent harnesses, teach evaluation design, or turn client policies into reusable rubrics.
- **Job to be done:** Access transparent trajectory data, reference methodologies, domain templates, and portable evidence.
- **Selection criteria:** permissive licensing, reproducibility, extensibility, report portability, and vendor-neutral curriculum.
- **Commercial behavior:** Researchers contribute credibility and extensions; consultancies and educators can distribute methodology and certification.
- **Confidence:** High for research fit, medium-low for certification demand pending buyer interviews.

### Behavior Drivers and Influences

**Rational drivers:** fewer escaped regressions, faster debugging, model portability, lower human-review burden, audit evidence, and freedom from proprietary trace formats. The core pain is measurable: observability is already near-table-stakes while evaluation remains materially less adopted. [LangChain, 2026](https://www.langchain.com/state-of-agent-engineering)

**Emotional drivers:** fear of a customer-facing agent failure, distrust of opaque automated judges, and frustration when a final score cannot explain why an agent failed.

**Social influences:** framework communities, GitHub activity, trusted practitioners, benchmark authors, and peer adoption shape the technical shortlist. GitHub reported rapid growth in generative-AI projects and contributions, reinforcing public repositories as a major experimentation and learning channel. [GitHub Octoverse, 2024](https://github.blog/news-insights/octoverse/octoverse-2024/)

**Economic influences:** OSS removes the initial procurement barrier; managed services become attractive when collaboration, scale, security assurance, and operational accountability outweigh self-hosting costs. Enterprise buyers are likely to prefer a hybrid: open engine and portable evidence, with paid platform capabilities layered above.

### Customer Interaction Patterns

1. **Discovery:** GitHub, npm, framework documentation, practitioner content, benchmark papers, and failure-driven searches.
2. **Technical validation:** Run locally on a small golden dataset or production incident set; compare whether the tool reveals failure modes missed by output-only evaluation.
3. **Team adoption:** Add CI gates, trace adapters, reusable rubrics, and shared failure taxonomies.
4. **Organizational standardization:** Platform or governance teams require security review, stable schemas, dashboards, access controls, and reproducible reports.
5. **Retention:** Every escaped incident becomes a regression case; accumulated datasets, calibrated rubrics, and comparable evidence create durable workflow value without requiring data lock-in.

_Purchase Decision Process:_ Developer champion → team proof of value → platform/security review → managed-platform or support decision.  
_Loyalty Drivers:_ evaluator validity, actionable failure attribution, backward-compatible evidence formats, trusted methodology, responsive maintainership, and avoidance of vendor lock-in.  
_Primary Sources:_ [Menlo Ventures, 2025](https://menlovc.com/perspective/2025-the-state-of-generative-ai-in-the-enterprise/), [LangChain, 2026](https://www.langchain.com/state-of-agent-engineering), [Anthropic, 2026](https://www.anthropic.com/engineering/demystifying-evals-for-ai-agents)

### Cross-Segment Synthesis and Research Gaps

The strongest entry motion is **developer-led OSS adoption among TypeScript teams that already have traces but lack trajectory regression tests**. The natural expansion buyer is the central evaluation/platform team; the natural long-term moat is a trusted, vendor-neutral methodology and certification ecosystem that makes evidence comparable across tools.

Critical assumptions still requiring primary interviews:

- Whether TypeScript teams perceive trajectory evaluation as urgent enough to add another test dependency
- Which trace schemas and frameworks should receive first-class adapters first
- Whether “per-step evidence grounding” catches sufficiently frequent and costly failures
- Who owns budget: application engineering, AI platform, quality, model risk, or governance
- Willingness to pay for certification and whether employers value it in hiring or assurance

**Overall confidence:** High that agent quality and evaluation maturity are market problems; medium that TypeScript-first trajectory evaluation is the best beachhead; medium-low that certification demand exists as a standalone market without first establishing methodology adoption.

## Customer Pain Points and Needs

### Customer Challenges and Frustrations

The central frustration is not lack of traces; it is lack of trustworthy interpretation. Existing tools can show tool calls, messages, latency, and token usage, but users still struggle to answer: **Was this successful run sound, was this failed run reasonable, where did the first material error occur, and what evidence supports that diagnosis?**

Five recurring problems define the opportunity:

1. **Outcome scores collapse unlike failures.** A correct result can arise from a brittle or unsafe process, while a failed result can follow a mostly sound path with one recoverable error. Microsoft Research's AgentLens study found substantial heterogeneity among successful SWE-agent runs: 10.7% of passing trajectories in its eligible subset were classified as “Lucky,” and model rankings shifted by as many as five positions when process quality replaced pass rate. [Microsoft Research, AgentLens, 2026](https://www.microsoft.com/en-us/research/publication/agentlens-revealing-the-lucky-pass-problem-in-swe-agent-evaluation/)
2. **Exact reference paths are brittle.** Anthropic reports that checking a prescribed sequence of steps is often too rigid because agents find valid approaches designers did not anticipate. LangSmith already offers strict, unordered, subset, and superset matching, demonstrating both the demand and the inadequacy of one exact path. [Anthropic, 2026](https://www.anthropic.com/engineering/demystifying-evals-for-ai-agents) [LangSmith trajectory evaluations](https://docs.langchain.com/langsmith/trajectory-evals)
3. **Holistic LLM judges are difficult to trust.** Judge outputs can reflect position, verbosity, self-preference, rubric overload, and task-specific weaknesses. Current research finds no single aggregate judge score captures all failure modes, while fine-grained evaluation can outperform holistic scoring for structural failures. [“Time to Reflect,” 2026](https://arxiv.org/html/2605.19196v1) [“A Survey on Agent-as-a-Judge,” 2026](https://arxiv.org/html/2601.05111v1)
4. **Tracing does not equal evaluation.** The 89% observability versus 52.4% offline-evaluation gap suggests teams can collect trajectories more easily than they can turn them into defensible tests. [LangChain, 2026](https://www.langchain.com/state-of-agent-engineering)
5. **Evaluation itself is costly and nondeterministic.** Repeated agent trials and model-based graders multiply token cost and latency; human review remains essential for nuanced or high-stakes cases.

_Primary Frustrations:_ opaque aggregate scores, unclear first-error attribution, brittle references, duplicated grader code, and difficulty converting incidents into regression cases.  
_Usage Barriers:_ trace normalization, missing intermediate evidence, privacy-sensitive content, nondeterministic runs, judge calibration, and unclear rubric ownership.  
_Frequency Assessment:_ Quality problems are common at the category level; the frequency and economic severity of specifically ungrounded intermediate claims still need production telemetry.  
_Confidence:_ High for the general diagnostic problem; medium for the prevalence of each proposed trajectory-error class.

### Unmet Customer Needs

The frontier is real but narrower than the initial thesis suggests. Commercial and OSS products already expose span graders, trace-level judges, order-insensitive matching, and separate process/outcome primitives. The unmet need is **validated semantics across those primitives**, not basic access to trajectories.

#### 1. Evidence-grounded per-step evaluation

Users need each externally verifiable claim or action linked to the observation, retrieved source, tool result, state transition, or policy that supports it. TRACE operationalizes a research version through an evidence bank and asks whether each thought is grounded in prior evidence. [TRACE, arXiv:2510.02837](https://arxiv.org/abs/2510.02837)

This should not be marketed as grading a model's true internal reasoning. Anthropic found Claude 3.7 Sonnet mentioned experimentally supplied hints only 25% of the time and DeepSeek R1 39%, despite evidence that the hints affected their answers. Stated chain-of-thought is therefore an unreliable proxy for latent reasoning. [Anthropic, “Reasoning models don't always say what they think,” 2025](https://www.anthropic.com/research/reasoning-models-dont-say-think)

**Product implication:** evaluate observable evidence use, actions, claims, and state transitions - not unverifiable reasoning prose.

#### 2. Path-invariant checkpoint and constraint scoring

Users need to define required checkpoints and invariants without dictating every intermediate action:

- required and forbidden actions
- partial-order constraints
- argument predicates and side-effect assertions
- state checkpoints before or after critical operations
- multiple accepted references
- semantic equivalence and partial credit

AgentLens merges multiple passing paths into Prefix Tree Acceptor references, while TRACE is reference-free and tests evidence use within the observed path. Both show promising directions, but neither establishes a universal solution. [AgentLens paper](https://arxiv.org/abs/2605.12925) [TRACE repository](https://github.com/wonjoong-kim/TRACE)

#### 3. Decoupled process and outcome scoring with error attribution

Users need four quadrants rather than one pass/fail:

| Outcome | Process | Interpretation |
|---|---|---|
| Correct | Sound | Reliable success |
| Correct | Unsound | Lucky, brittle, unsafe, or non-reproducible success |
| Incorrect | Sound | Capability, environment, or terminal-step limitation |
| Incorrect | Unsound | Process failure requiring root-cause diagnosis |

The distinction itself is no longer unique: Anthropic, Google Vertex AI, OpenAI trace grading, Braintrust, Patronus, Langfuse, and LangSmith expose related primitives. The whitespace is a **portable, independently validated taxonomy**, first-error localization, confidence, and comparable evidence across vendors.

_Critical Unmet Needs:_ observable evidence lineage, semantic checkpoints, calibrated evaluators, first-error detection, error propagation, uncertainty/abstention, and portable process/outcome reports.  
_Priority:_ Evidence grounding and path invariance are highest; a broad universal taxonomy should follow domain-specific validation rather than precede it.  
_Confidence:_ Medium-high that these capabilities are under-served; low that one cross-domain scoring function will work without domain adapters.

### Research Evidence and Its Limits

#### TRACE

TRACE evaluates efficiency, hallucination, and adaptivity using an accumulated evidence bank. It provides direct support for reference-free, step-level grounding and reports strong results on its meta-evaluation datasets. However:

- Most injected faults are synthetic.
- “Reference-free” still depends on LLM judgment and potentially fallible tool observations.
- It identifies unnecessary evidence within an observed trajectory, not a globally optimal path.
- Valid prior model knowledge absent from the evidence bank may be labeled hallucination.
- It depends on exposed thought fields, which may be unavailable or unfaithful.
- Adaptivity testing is narrow, focusing on immediate response to unavailable tools.

**Assessment:** highly relevant design prior and potential source to borrow from, but not production validation that the problem is solved. [TRACE paper](https://arxiv.org/abs/2510.02837) [TRACE code](https://github.com/wonjoong-kim/TRACE)

#### AgentLens

AgentLens evaluated 2,614 OpenHands trajectories across eight model backends and 60 SWE-bench Verified tasks; 47 tasks had enough passing runs to construct references, producing a 1,815-trajectory subset. It classified 10.7% of passing trajectories as Lucky.

Important caveats:

- Lucky/Solid/Ideal labels derive from AgentLens thresholds rather than independent human ground truth.
- 12.3% of the Lucky category was “Divergent-but-Valid,” exposing reference-space false positives.
- Process references varied materially with trajectory selection and merge order.
- The study is limited to software-engineering tasks and one agent framework.
- The announced SDK repository was unavailable during this research snapshot.

**Assessment:** strong evidence that pass rates hide meaningful process variation, but the 10.7% figure is not a universal lucky-pass prevalence estimate. [Microsoft Research](https://www.microsoft.com/en-us/research/publication/agentlens-revealing-the-lucky-pass-problem-in-swe-agent-evaluation/) [arXiv:2605.12925](https://arxiv.org/abs/2605.12925)

### Barriers to Adoption

| Barrier | Severity | Why it matters | Required response |
|---|---:|---|---|
| Integration and schema fragmentation | Critical | Agent traces differ across frameworks, nested agents, tool errors, and runtimes | OTLP plus portable JSON, adapters, normalization, trace-to-test conversion |
| Evaluator validity | Critical | A sophisticated but uncalibrated judge creates false assurance | Versioned judges, calibration sets, repeated trials, abstention, agreement metrics |
| Unclear incremental value | Critical | Teams already have traces, output tests, and generic judges | Demonstrate unique caught failures and shorter diagnosis time |
| Privacy and data residency | High | Prompts, retrievals, tool arguments, and outputs can contain secrets or PII | Local-first, metadata-only defaults, redaction, allowlists, retention controls |
| Cost and latency | High | Repetitions and model judges multiply execution cost | Deterministic-first cascades, caching, sampling, budgets, async execution |
| Benchmark and rubric brittleness | High | Broken or ambiguous cases produce misleading regressions | Provenance, rubric versions, challenge workflow, sensitivity analysis |
| Human calibration burden | High | Domain experts remain necessary and quality definitions drift | Blind review, disagreement tracking, adjudication, active sampling |
| TypeScript skepticism | Medium | TypeScript support now exists in Promptfoo and AgentEvals | Win on depth and ergonomics, not merely language |
| New methodology learning curve | Medium | Process scoring is harder to explain than pass/fail | Opinionated defaults, visual evidence, worked examples, certification later |
| Certification credibility | High | A self-issued credential without employer demand has little value | Independent advisory body, public syllabus, practical assessment, employer pilots |

OpenTelemetry explicitly warns that GenAI span content can include sensitive instructions, messages, retrieval queries, tool arguments, and outputs. Privacy-safe defaults are therefore part of product-market fit, not an enterprise add-on. [OpenTelemetry GenAI span conventions](https://github.com/open-telemetry/semantic-conventions-genai/blob/main/docs/gen-ai/gen-ai-spans.md)

### Service and Support Pain Points

OSS users will expect more than API documentation:

- examples covering real traces, nested tools, retries, and partial failures
- transparent explanations for every score
- migration guarantees for evidence schemas
- framework adapters maintained as upstream SDKs evolve
- reproducible issue reports without leaking sensitive trajectory content
- guidance for judge calibration and rubric design

The likely support failure mode is conceptual rather than operational: users may receive a score but remain unsure whether the evaluator is valid for their domain. eval-quality therefore needs methodology documentation, evaluator cards, confidence reporting, and calibration workflows from the beginning.

_Support Gap:_ existing vendor docs often explain how to run an evaluator but not how to establish that it measures the intended property.  
_Response-Time Need:_ framework breakages require fast maintenance; methodology disputes require documented evidence and governance rather than ad hoc support answers.

### Customer Satisfaction and Trust Gaps

**Expectation gap:** Buyers expect automated evaluation to provide release confidence; current methods often provide only proxy scores. A 2026 enterprise survey found only 5% fully trusted automated evaluation, while half reported a customer-facing failure after an agent had passed internal evaluations. The same survey found two-thirds of organizations already allow, or are actively engineering toward, shipping agent changes to production on automated evaluation alone, with no human in the loop - autonomy is outrunning trust, not lagging behind it. The sample was 157 enterprises and should be treated as directional, not population-level truth. [VentureBeat Pulse Research, 2026](https://venturebeat.com/ai/the-agent-evaluation-gap-enterprise-ai-organizations-have-a-reality-alignment-problem-not-a-coverage-problem-and-most-are-shipping-to-production-anyway)

**Quality gap:** Aggregate pass rates conceal inconsistency, severity, and process quality. A single run says little about repeatability.

**Value gap:** Another dashboard or generic judge wrapper adds little value because tracing and holistic judging are already crowded categories.

**Credibility gap:** “Reasoning evaluation” can imply access to a model's actual cognition. That claim is technically indefensible from ordinary traces. eval-quality must narrowly claim evaluation of **observable trajectory evidence and behavior**.

### Emotional and Organizational Impact

- **Engineers:** frustration and lost time when debugging nondeterministic failures from long traces
- **Leaders:** fear of customer harm, brand damage, and unreliable automated release gates
- **Governance teams:** anxiety about proving policy adherence and reconstructing decisions after incidents
- **Developers adopting OSS:** concern about lock-in, telemetry leakage, and abandoned integrations
- **Methodology buyers:** skepticism toward credentials created by a vendor without independent governance

These pressures can accelerate adoption after an incident, but fear-led messaging alone is counterproductive. The product should demonstrate faster diagnosis and defensible evidence, not promise certainty.

### Pain Point Prioritization

#### High priority: product wedge

1. Evidence-grounded checks over observable claims, actions, and tool results
2. Path-invariant constraints and semantic checkpoints
3. Independent process-versus-outcome scores with first-error localization
4. Trace normalization and TypeScript test-runner ergonomics
5. Evaluator calibration, confidence, and abstention
6. Privacy-safe local execution and portable evidence

#### Medium priority: adoption and expansion

1. Cross-run clustering and error-taxonomy analytics
2. Human-review and adjudication workflows
3. Cost-aware evaluator cascades
4. Managed collaboration, governance, and reporting

#### Later and validation-dependent

1. Universal cross-domain process score
2. Broad certification program
3. Full hosted observability platform

### Refined Opportunity Statement

The defensible gap is not “nobody evaluates trajectories.” Existing platforms already provide trace inspection, tool-call matching, span scorers, and holistic judges. The opportunity is:

> **A vendor-neutral, TypeScript-first evaluation kernel that turns observable agent traces into calibrated evidence: grounding each verifiable step, accepting multiple valid paths through checkpoint and constraint scoring, and separating process quality from outcome quality with explicit error attribution.**

This framing incorporates Anthropic's warning: eval-quality should not grade chain-of-thought as though it were faithful internal reasoning. It should grade externally observable evidence, actions, checkpoints, and outcomes.

**Overall confidence:** High that current evaluation creates diagnostic and trust pain; medium-high that the refined wedge is technically differentiated; medium that customers will prioritize it over simpler output tests without compelling incident-based demonstrations.

## Customer Decision Processes and Journey

### Two Linked Decisions

eval-quality must win two different decisions:

1. **Developer adoption:** “Is this library useful enough to add to my codebase and CI?”
2. **Organizational purchase:** “Is the managed Evidence Platform valuable, safe, and supportable enough to standardize across teams?”

The OSS user, technical champion, economic buyer, and risk approver are often different people. Developer adoption can occur without procurement; enterprise conversion requires evidence that the hosted platform adds collaboration, governance, and operational value without undermining the portability promised by OSS.

### Customer Decision-Making Process

| Stage | Primary participants | Decision | Required evidence | Common failure |
|---|---|---|---|---|
| 1. Problem recognition | Application engineer, AI lead | Is an evaluation problem worth solving? | Escaped regression, manual-review cost, unexplained trajectory, audit need | Pain remains anecdotal or low priority |
| 2. OSS discovery | Developer user | Is eval-quality credible enough to try? | Clear thesis, npm package, license, examples, releases, project health | Vague positioning or generic “LLM quality” claims |
| 3. Local proof of value | Developer user | Does it catch a real missed failure? | One representative trace, baseline, actionable process/outcome diagnosis | Setup cost exceeds insight; judge score is unconvincing |
| 4. Team pilot | Champion, peers, platform engineer | Should it become a team dependency? | CI integration, repeatability, calibration, privacy behavior, comparison against alternatives | Flaky results, weak adapters, no maintainer confidence |
| 5. Production standardization | Platform owner, engineering leadership | Should multiple teams use one methodology? | Versioned schemas/rubrics, incident-to-test loop, ownership, cost controls | Each team retains incompatible scripts and metrics |
| 6. Managed-platform trigger | Champion, platform owner, sponsor | Is centralization better than operating OSS alone? | Shared evidence store, datasets, review, lineage, reporting, support, lower internal maintenance | Hosted product is only a dashboard over free features |
| 7. Technical and risk review | Engineering, security, privacy, model risk | Is it valid, secure, portable, and operable? | Architecture, data flow, DPA, trust evidence, evaluator validation, export test | Sensitive traces, opaque judges, proprietary instrumentation |
| 8. Commercial decision | Sponsor, procurement, legal | Are economics and terms acceptable? | TCO, ROI case, SLA, pricing, audit/data rights, exit terms, references | Hidden pricing, uncertain budget owner, unresolved risk exception |
| 9. Renewal and expansion | Platform owner, governance, sponsor | Is the platform producing operational evidence? | Adoption, regression escapes, diagnosis time, audit readiness, support, portability | Evidence is collected but does not alter release decisions |

This bottom-up-to-enterprise pattern is visible across AI developer tooling. Menlo Ventures reports developers and technical teams discovering tools individually, proving daily value, and generating demand that later converts into enterprise contracts. Its 2025 analysis used a survey of 495 U.S. enterprise AI decision-makers and a bottom-up market model. [Menlo Ventures, 2025](https://menlovc.com/perspective/2025-the-state-of-generative-ai-in-the-enterprise/)

_Decision Timelines:_ No defensible public benchmark was found for OSS evaluation-library adoption or evaluation-platform procurement duration. Timelines depend on dependency review, data sensitivity, budget cycle, and contract requirements; the report therefore avoids invented ranges.  
_Decision Complexity:_ Low for local OSS experimentation, medium for team adoption, and high for managed enterprise standardization.

### Decision Factors and Criteria

#### Developer adoption criteria

| Criterion | Relative importance | Proof expected |
|---|---:|---|
| Detects a failure current tests miss | Critical | Side-by-side result on the team's own trace |
| TypeScript-native ergonomics | Critical for beachhead | Typed API, npm installation, Vitest/Jest pattern, no Python sidecar |
| Actionable diagnosis | Critical | Evidence links, first-error location, process/outcome classification |
| Local/privacy-safe execution | High | No mandatory upload; documented network behavior |
| Reproducibility | High | Versioned models, rubrics, inputs, seeds/configuration, repeated-run support |
| Integration | High | OpenTelemetry and major TypeScript agent framework adapters |
| Performance and cost | Medium-high | Deterministic-first execution, caching, budget visibility |
| OSS credibility | Medium-high | Permissive license, release cadence, security policy, transparent roadmap |

#### Enterprise platform criteria

| Criterion | Relative importance | Proof expected |
|---|---:|---|
| Evaluator validity | Critical | Human calibration, chance-corrected agreement, stability and bias tests |
| Data protection and deployment | Critical | Data flow, retention/deletion, residency, encryption, no-training terms |
| Portability and neutrality | Critical | OTLP ingestion, raw export, code-defined evaluators, documented exit test |
| Lifecycle coverage | High | Production trace → review → dataset → experiment → CI gate |
| Enterprise controls | High | SSO, SCIM, RBAC, audit logs, service accounts, tenant isolation |
| Operational fit | High | SLA, support, APIs, scale tests, administration, cost controls |
| TCO and pricing predictability | High | Seats, traces/spans/scores, model costs, retention, egress, operations |
| Vendor and project durability | Medium-high | Financial/project health, maintenance commitments, roadmap, references |

Enterprise AI procurement is becoming more rigorous, with hosting, benchmark scrutiny, security, and cost gaining influence. a16z's 2025 research combined a survey of 100 CIOs across 15 industries with buyer interviews; it is useful directional evidence but carries an investor perspective. [a16z, 2025](https://a16z.com/ai-enterprise-2025/)

#### Recommended proof-of-value scorecard

The initial evaluation should use the customer's own traces rather than a polished vendor benchmark:

- **30% failure-detection value:** catches known incidents and identifies the earliest material cause
- **20% evaluator validity:** agreement with independent domain reviewers; stability under repeated runs and perturbations
- **15% TypeScript/CI fit:** local execution, custom evaluators, test-runner gates
- **15% privacy and deployment:** no unintended upload, redaction, self-host or regional requirements
- **10% portability:** neutral trace/evidence export and code-defined rubric portability
- **10% cost and operational fit:** runtime, model calls, storage, support, and ownership

Weights should change by segment; regulated buyers should shift weight toward privacy, lineage, and governance.

### Customer Journey Map

#### Awareness

The prospect experiences or anticipates a failure that output-only tests cannot diagnose. Discovery occurs through GitHub, npm, framework integrations, practitioner examples, research papers, conference talks, and incident-focused search.

**Required message:** “Know whether an agent succeeded for the right reasons - without grading private chain-of-thought or prescribing one golden path.”

#### Consideration

The developer compares eval-quality with existing scripts, Promptfoo, AgentEvals/LangSmith, DeepEval, Phoenix, Langfuse, Braintrust, and provider-native tooling. The project enters the shortlist only if it states clearly what it does that generic trajectory judges and tool-call matchers do not.

**Required artifacts:** competitor comparison, architecture, exact license, supported schemas, sample failure taxonomy, evaluator card, privacy model, and runnable examples.

#### Local trial

The user installs the package, imports or records one trajectory, defines checkpoints/evidence expectations, and examines process and outcome results. The decisive moment is not a successful run; it is an explanation that the user agrees with and could not obtain as quickly before.

**Activation event:** first accepted diagnosis on the user's own trace.

#### Team adoption

The champion adds a regression suite to CI and socializes the method. The team tests reliability across repeated runs, reviews disagreements, and decides who owns rubrics and thresholds.

**Expansion signal:** multiple contributors, repositories, adapters, or shared evaluator packages within one organization.

#### Organizational standardization

Platform teams seek shared schemas, datasets, evaluator registries, human review, and production feedback. Security and governance ask how trajectory content is handled and whether every result is reproducible.

**Managed-platform trigger:** the coordination and evidence-management cost exceeds the cost of operating the local engine.

#### Procurement

The champion needs a forwardable business case. Security, privacy, legal, governance, finance, and procurement may each hold veto power. The purchase stalls if pricing, trust documentation, architecture, or exit rights are unclear.

**Required commercial bridge:** public pricing logic, trust center, data-flow documentation, TCO calculator, pilot report template, and export/exit commitment.

#### Post-purchase and renewal

The platform must become part of release and incident workflows, not a passive dashboard. Renewal depends on lower diagnosis time, increased evaluated coverage, fewer escaped critical regressions, defensible approvals, and predictable cost.

### Touchpoint and Information-Source Analysis

| Touchpoint | Audience | Decision role |
|---|---|---|
| GitHub repository and npm registry | Developers | Establish technical credibility and reduce trial friction |
| Documentation and examples | Developers, champions | Prove exact workflows and limitations |
| Research validation pages | Champions, researchers | Explain TRACE/AgentLens borrowing and methodological departures |
| Framework adapters | Developers, platform teams | Serve as both integration and distribution channels |
| Public benchmarks and calibration sets | Champions, governance | Demonstrate evaluator validity rather than only agent scores |
| Security policy, SBOM, OpenSSF signals | Platform/security | Assess OSS dependency risk |
| Trust center, DPA, architecture | Security, privacy, procurement | Remove managed-platform vetoes |
| Comparison and migration guides | Champions, platform teams | Bound switching cost and clarify differentiation |
| Certification syllabus and sample assessment | Employers, practitioners | Test methodology credibility - only after adoption evidence |
| Customer references | Sponsor, procurement | Validate fit at comparable scale and risk |

Developers rely heavily on documentation for learning and reject tools for security/privacy concerns, pricing, or better alternatives. The Stack Overflow 2025 survey provides broad but self-selected evidence across more than 49,000 responses. [Stack Overflow Developer Survey, 2025](https://survey.stackoverflow.co/2025/)

### Information Gathering and Trust Hierarchy

Prospects should verify claims in this order:

1. Repository source, `LICENSE`, releases, package metadata, and security policy
2. Runnable proof on their own trajectories
3. API, self-hosting, security, privacy, and pricing documentation
4. OpenTelemetry interoperability and export tests
5. Independent papers and reproducible evaluator-validity studies
6. Trust-center evidence, SOC reports, DPAs, and architecture supplied under NDA
7. GitHub issues and release history
8. References from customers with comparable scale and regulation
9. Vendor comparisons and surveys as discovery inputs only

Open-source selection should include project sustainability and supply-chain review. The Linux Foundation's 2025 global research reports demand for long-term support, rapid security patching, developer training, and upstream participation; the exact importance varies by region and organization. [Linux Foundation, State of Global Open Source 2025](https://www.linuxfoundation.org/research/world-of-open-source-global-2025)

### Decision Influencers and Buying Committee

| Role | Core question | Likely veto |
|---|---|---|
| Developer user | Does this make evaluation and debugging materially better? | Poor DX, flaky scores, mandatory cloud |
| Technical champion | Can this become a defensible team standard? | Weak proof, no owner, low project health |
| Platform owner | Can we operate and integrate it across teams? | Proprietary schemas, weak APIs, uncontrolled cost |
| Security/privacy | What data and code enter our trust boundary? | Sensitive-data leakage, weak supply chain, unclear retention |
| Governance/model risk | Can we reconstruct and defend approval decisions? | Unvalidated judges, missing lineage, undocumented changes |
| Procurement/legal | Are supplier terms, rights, and economics acceptable? | Hidden price, weak SLA, poor data/audit/exit terms |
| Executive sponsor | Does standardization improve delivery and reduce risk enough? | No quantified baseline, unclear ownership or ROI |
| Researchers/practitioners | Is the methodology technically credible? | Overclaiming internal reasoning or universal validity |

NIST's Generative AI Profile recommends due diligence for acquired systems and components, including privacy, security, intellectual property, and ongoing monitoring. This supports treating evaluation-platform purchase as both an engineering and governance decision. [NIST AI 600-1](https://www.nist.gov/publications/artificial-intelligence-risk-management-framework-generative-artificial-intelligence)

### Purchase Drivers and Delays

**Immediate drivers:**

- customer-facing incident that passed internal evaluation
- model or framework migration requiring regression confidence
- multiple agent teams duplicating evaluator infrastructure
- increased autonomy or high-consequence deployment
- audit, model-risk, or policy-evidence deadline
- human-review cost becoming a release bottleneck

**Delay factors:**

- current scripts appear “good enough”
- process-quality value has not been quantified
- disagreement over rubric ownership
- uncertainty about trace privacy
- overlap with an observability or MLOps platform
- new-category budget without an obvious owner
- certification demand assumed rather than demonstrated

**Price sensitivity:** Individual developers expect the core engine to be free and open. Team and enterprise buyers compare hosted cost with internal operations, judge-model spend, incident cost, and existing platform overlap - not only license price.

### Decision Optimization Recommendations

#### Reduce developer friction

- Deliver a useful local result from one trace and one command.
- Publish first-class TypeScript types and Vitest/Jest examples.
- Offer adapters without requiring eval-quality-owned tracing.
- Make every score explainable and link it to observable evidence.
- Include examples where eval-quality disagrees with output-only and holistic-judge results.

#### Build trust

- Publish evaluator cards: intended use, calibration data, limitations, model/version, and known biases.
- Separate deterministic evidence checks from model judgments.
- Support confidence, abstention, reviewer disagreement, and rubric versioning.
- Label TRACE and AgentLens concepts as research-derived, not settled standards.
- Avoid “we evaluate the model's true reasoning” language.

#### Enable enterprise conversion

- Keep evaluators and evidence portable between OSS and managed offerings.
- Make the paid value collaboration, scale, governance, and operations - not withholding core methodology.
- Provide a champion kit: one-page business case, architecture, privacy answer, comparison matrix, and pilot scorecard.
- Publish modelable pricing and an exit/export guarantee.
- Treat the Evidence Platform as a system of record for decisions and provenance, while eval-quality remains one evidence producer.

#### Build durable loyalty

- Backward-compatible evidence schemas
- stable, vendor-neutral methodology
- maintained ecosystem adapters
- incident-to-regression workflows
- transparent governance of taxonomy and certification
- independent practitioner participation in methodology evolution

### Journey Evidence Quality

**High confidence:** technical selection criteria, security/privacy concerns, need for evaluator calibration, OSS project-health review, and separation of developer versus enterprise buyer.  
**Medium confidence:** the exact handoff from OSS team use to managed Evidence Platform and which function owns budget.  
**Low confidence:** universal procurement timelines and willingness to pay for certification; both require interviews and observed funnel data.

## Competitive Landscape

### Category Structure

The market is not one category. It contains four overlapping layers:

1. **Local/CI evaluation frameworks:** Promptfoo, DeepEval, Ragas, Inspect AI, OpenAI Evals
2. **Hosted evaluation and observability platforms:** Braintrust, LangSmith, Langfuse, Arize AX/Phoenix, Galileo, Patronus, W&B Weave, Confident AI
3. **Agent frameworks with embedded evaluation:** Mastra and Vercel Eve
4. **Provider-native evaluation:** OpenAI and other model-provider tooling

Most mature teams combine an offline framework with a production observability/evaluation platform. eval-quality should therefore behave as a composable evidence producer, not attempt to replace every trace store, benchmark runner, dataset manager, and human-review UI.

### Key Market Players

| Product | OSS / commercial reality | Language and TypeScript status | What and how it evaluates | Primary positioning | Strategic implication for eval-quality |
|---|---|---|---|---|---|
| **Promptfoo** | MIT Community edition; proprietary hosted/on-prem enterprise offering. Acquisition by OpenAI confirmed, announced 2026-03-09, folded into OpenAI Frontier for agentic security testing. OpenAI committed to keeping it open source under its current license and continuing to support existing customers - but that is a license commitment, not a neutrality guarantee. | Genuinely TS-first Node/CLI library; YAML remains the dominant UX. | Deterministic output and trajectory assertions; tool use, arguments, sequence, step count, cost/latency; RAG metrics; LLM rubrics; red teaming. | Broad AI security, red-team, and CI evaluation suite. | Major distribution and TS competitor, now owned by a model provider. Its prior "vendor-neutral" positioning is arguably weakened by that ownership - a conflict-of-interest optic (analysis, not a claim of bad faith) for an evaluation tool, and a sharper opening for eval-quality's genuine independence than mere feature differentiation. [Repository](https://github.com/promptfoo/promptfoo) [OpenAI acquisition announcement](https://openai.com/index/openai-to-acquire-promptfoo/) |
| **DeepEval / Confident AI** | DeepEval Apache-2.0 OSS; Confident AI proprietary managed/on-prem platform. | Python-first. TypeScript implementation is expanding rapidly, but published local-evaluator parity and package licensing remain unsettled as of the research date. | Task completion, step efficiency, plan quality/adherence, tool/argument correctness, faithfulness, G-Eval, loop detection, tool permissions, component and trace scoring. | “Pytest for LLMs” plus a commercial quality platform. | Strongest direct metric-taxonomy competitor; its TS expansion can erase language-only differentiation. [DeepEval](https://github.com/confident-ai/deepeval) [Agent metrics](https://deepeval.com/docs/metrics-step-efficiency) |
| **Ragas** | Apache-2.0 OSS; no standardized hosted product found. | Python; no official TypeScript evaluator SDK. | RAG faithfulness and context metrics; tool-call sequence/arguments; agent-goal accuracy; rubric judges. | Focused RAG evaluation library. | Useful metric source, but weaker at full trajectory semantics. [Repository](https://github.com/vibrantlabsai/ragas) [Agent metrics](https://docs.ragas.io/en/stable/concepts/metrics/available_metrics/agents/) |
| **OpenAI Evals / trace grading** | Legacy `openai/evals` code is MIT, with dataset-specific licenses; modern hosted Evals and trace grading are proprietary. Hosted Evals is being wound down and users are directed to Promptfoo. | OSS framework is Python. Hosted APIs have typed Node support, but custom code graders execute Python. | Output graders, model judges, Python graders, multi-graders, complete OpenAI Agent traces, tools, handoffs, guardrails, and outcomes. | Provider-native evaluation for OpenAI applications. | Migration creates demand for portable eval artifacts. Do not rely on provider-native persistence. [OSS repository](https://github.com/openai/evals) [Migration to Promptfoo](https://developers.openai.com/cookbook/examples/evaluation/moving-from-openai-evals-to-promptfoo) |
| **Inspect AI** | MIT OSS from the UK AI Security Institute; no commercial hosted tier found. | Python; no TypeScript authoring parity. | Reproducible benchmark/task execution, agent bridges, full transcripts, custom scorers, tools, sandboxes, limits, intervention, checkpoint/resume, rescorable logs. | Serious agent/model evaluation harness for safety and research. | Interoperate with its logs; borrow event/log/scorer separation. Do not compete with its sandbox/runtime breadth. [Documentation](https://inspect.aisi.org.uk/) [Repository](https://github.com/UKGovernmentBEIS/inspect_ai) |
| **Braintrust** | Proprietary platform; TypeScript/Python SDKs Apache-2.0; Autoevals MIT. Self-hosting retains a Braintrust-operated control plane. | Strong first-class TypeScript and Python evaluation. | Trace/span scorers, typed agent assertions, tool order/arguments/budgets, datasets, experiments, CI, online scoring, human review, production-to-regression loop; RAG grounding metrics. | Managed evaluation-first AI development platform. | Strongest complete-loop and TS threat. eval-quality should export to Braintrust rather than reproduce its platform. [Evaluation docs](https://www.braintrust.dev/docs/evaluate/run-evaluations) [Autoevals](https://github.com/braintrustdata/autoevals) |
| **LangSmith / AgentEvals** | LangSmith platform proprietary; paid enterprise self-hosting. SDK and standalone AgentEvals are MIT. | Real Python and TypeScript support for trajectory evaluators and code evaluators. | Strict, unordered, subset, and superset trajectory matching; tool-argument overrides; graph matching; LLM trajectory judge; output, step, multi-turn, online, human, and pairwise evaluation. | LangChain/LangGraph-native observability and evaluation. | Existing matching modes are table stakes. Improve with partial-order semantics, evidence graphs, and first-error attribution. [AgentEvals](https://github.com/langchain-ai/agentevals) [Trajectory docs](https://docs.langchain.com/langsmith/trajectory-evals) |
| **Arize Phoenix / AX** | Phoenix platform and Python evals use ELv2 - source-available, not OSI open source. TypeScript eval package is Apache-2.0. Arize AX is commercial. | Python heritage; real TypeScript evaluator package, still labeled alpha. | OTel/OpenInference tracing, faithfulness, correctness, document relevance, tool selection/invocation/response handling, path/convergence recipes, experiments and CI. | Open-observability and evaluation infrastructure with enterprise AX upgrade. | Prime integration target and source for Apache-licensed TS evaluator patterns. Avoid ELv2 code in a hosted service. [License](https://arize.com/docs/phoenix/self-hosting/license) [TS evals](https://arize.com/docs/phoenix/sdk-api-reference/typescript/packages/phoenix-evals/overview) |
| **Langfuse** | MIT core with proprietary enterprise directories and controls; acquired by ClickHouse in January 2026 and continues operating. | Most of the platform is TypeScript; real TS code evaluators and experiment runners. | Traces, observations, sessions, experiments, LLM judges, code evaluators, human annotations, production rules, RAG templates, score analytics and agent graphs. | Open-core AI engineering and observability platform. | Strong self-host and TS platform; weakly opinionated on trajectory semantics. eval-quality should emit scores and evidence to Langfuse. [License](https://github.com/langfuse/langfuse/blob/main/LICENSE) [ClickHouse acquisition](https://clickhouse.com/blog/clickhouse-acquires-langfuse-open-source-llm-observability) |
| **Galileo** | Proprietary platform; public Python/TypeScript clients are Apache-2.0. Hosted, customer-VPC, and on-prem options. | Python ahead of TypeScript; not TS-first. | Agent Flow, Action Advancement, Action Completion, Agent Efficiency, tool selection/arguments, reasoning coherence, grounding, experiments, monitoring and controls. | Enterprise evaluation intelligence, observability, and guardrails. | Already has the closest commercial metric vocabulary. Differentiate through open, deterministic, inspectable semantics. Galileo remains independent; Cisco is an AGNTCY partner, not an acquirer. [Agentic metric docs](https://github.com/rungalileo/docs-official/tree/main/concepts/metrics/agentic) [AGNTCY](https://galileo.ai/blog/agntcy-open-collective-multi-agent-standardization) |
| **Mastra** | Apache-2.0 core and `@mastra/evals`, with restrictive enterprise-edition directories; repository is mixed-license. | Genuinely TS-first. | Typed scorers, deterministic quick checks, tool presence/order/count/errors, trajectory accuracy, grounding, datasets, experiments, gates, production sampling, historical trace scoring, and tool mocks. | Full TypeScript agent/workflow framework with embedded evaluation. | Closest OSS developer-experience competitor. eval-quality must be framework-neutral and deeper in evidence/path semantics. [Repository](https://github.com/mastra-ai/mastra) [Evals](https://mastra.ai/docs/evals/overview) |
| **Vercel AI SDK / Eve** | AI SDK and Eve Apache-2.0; Vercel platform commercial. | TS-first. | AI SDK itself supplies rich telemetry, not native semantic evals. Separate beta Eve framework provides multi-turn eval cases, tool/event/subagent assertions, judges, gates, JUnit and local/remote targets. | Dominant TypeScript AI application SDK; Eve is an emerging agent runtime. | Highest-value initial trace adapter. Do not describe AI SDK telemetry as a full eval framework. [AI SDK telemetry](https://ai-sdk.dev/docs/ai-sdk-core/telemetry) [Eve](https://github.com/vercel/eve) |
| **Humanloop** | Proprietary platform shut down September 8, 2025 after its founders and most staff joined Anthropic. Public SDK repositories have no declared reusable license. | Historically real TypeScript and Python evaluation. | Formerly datasets, code/model/human evaluators, prompt lifecycle, CI and collaboration. | No longer an active competitor. | Migration and shutdown reinforce portability as a requirement. Do not copy no-license SDK code. [Official sunset](https://humanloop.com/) [Migration guide](https://humanloop.com/docs/v5/guides/migrating-from-humanloop.md) |
| **Patronus AI** | Proprietary platform; selected API clients/MCP server Apache-2.0; several models/datasets are noncommercial or additionally restricted. | Full framework Python-first; Node client provides typed remote API access, not parity. | Percival full-trace diagnosis, 20+ reasoning/planning/execution errors, grounding and hallucination evaluators, human annotation, guardrails, simulations and custom taxonomies. | Enterprise evaluator, AI debugger, and simulated “Digital World Model” platform. | Closest competitor to the process-error thesis. Win on deterministic local evidence and portable semantics. [Percival](https://www.patronus.ai/percival) [Error taxonomy](https://docs.patronus.ai/docs/percival/error-taxonomy) |
| **W&B Weave** | Apache-2.0 SDK/evaluation repository; managed and self-managed W&B deployments are commercial. W&B was acquired by CoreWeave in 2025. | Real TypeScript and Python evaluation, but TS lacks several Python scorer and aggregation features. | Agent tracing, sessions/turns/tools, datasets, repeated trials, custom scorers, annotations, online scoring and production-to-dataset workflows. | Agent observability and continuous improvement integrated with the W&B ML stack. | Valuable OSS evaluation architecture and export target; less opinionated on deterministic process validity. [Repository](https://github.com/wandb/weave) [Evaluations](https://docs.wandb.ai/weave/guides/core-types/evaluations) |

### TypeScript Landscape: Revised Finding

The premise that TypeScript evaluation is rare is now only partly true.

**First-class TypeScript evaluation exists in:**

- Promptfoo
- Mastra
- Braintrust
- LangSmith/AgentEvals
- Langfuse
- W&B Weave, with documented parity gaps
- Vercel Eve, currently beta

**TypeScript is real but immature or secondary in:**

- Arize Phoenix TypeScript evals, still alpha
- DeepEval, with rapidly expanding but not yet dependable published parity
- Galileo

**Python remains dominant in:**

- Ragas
- Inspect AI
- OpenAI's OSS framework and custom hosted code graders
- Patronus's high-level framework

Therefore, “TypeScript-first” is a useful adoption attribute but not a durable standalone moat.

### Capability Matrix

Legend: **●** strong/productized, **◐** available through composition or custom code, **○** weak/not found.

| Product | Tool/argument checks | Trajectory/path | Semantic grounding | Process/outcome separation | TS eval runtime | Local/OSS core |
|---|:---:|:---:|:---:|:---:|:---:|:---:|
| Promptfoo | ● | ● | ● | ◐ | ● | ● |
| DeepEval | ● | ● | ● | ● | ◐ | ● |
| Ragas | ● | ◐ | ● | ◐ | ○ | ● |
| OpenAI Evals | ● | ● hosted | ◐ | ◐ | ◐ | ● legacy |
| Inspect AI | ◐ | ● transcript | ◐ | ◐ | ○ | ● |
| Braintrust | ● | ● | ● | ●/composed | ● | ◐ SDKs |
| LangSmith/AgentEvals | ● | ● | ●/custom | ◐ | ● | ◐ library |
| Phoenix | ● | ●/recipes | ● | ◐ | ◐ | ◐ ELv2 |
| Langfuse | ◐ | ◐ | ● | ◐ | ● | ● core |
| Galileo | ● | ● | ● | ● | ◐ | ○ |
| Mastra | ● | ● | ● | ◐ | ● | ● core |
| Vercel AI SDK/Eve | ● Eve | ● Eve | ◐ | ◐ | ● | ● |
| Humanloop | - | - | - | - | - | - |
| Patronus | ◐ | ● | ● | ● | ◐ API | ○ |
| W&B Weave | ◐ | ◐ | ●/Python | ◐ | ● | ● SDK |

### Validation of the “Commoditized” Thesis

The supplied commoditized capability set is validated, with an important extension:

- **Tool selection, invocation, and argument correctness:** broadly available across Promptfoo, DeepEval, AgentEvals, Braintrust, Phoenix, Galileo, Mastra, Patronus, and others.
- **Efficiency/convergence:** available through step counts, budgets, loop detection, semantic efficiency judges, or path-convergence recipes.
- **Holistic trajectory coherence:** common through LLM trajectory judges and full-trace evaluators.
- **Basic process-versus-outcome scoring:** also becoming commoditized as separate component scores, especially in Galileo, DeepEval, Patronus, Braintrust, and platform-level custom evaluators.

eval-quality should implement these for completeness and interoperability, but should not lead with them.

### Validation of the “Open Frontier” Thesis

The frontier exists, but “only in research” is too strong.

#### Per-step faithfulness and evidence grounding

Grounding metrics are common, but most evaluate a response/span against one context bundle. Few treat **claim-to-evidence lineage across a complete trajectory** as the primary artifact. Patronus Percival and TRACE come closest conceptually; commercial platforms provide primitives rather than a portable evidence standard.

#### Path-invariant/checkpoint reference scoring

AgentEvals already supports unordered, subset, and superset matching; Promptfoo, Braintrust, Mastra, and Eve support configurable trajectory constraints. AgentLens merges multiple passing paths. The remaining gap is richer:

- semantic checkpoints
- partial-order constraints
- interchangeable evidence sources and tools
- state invariants
- multiple valid branches
- explicit treatment of unseen but valid strategies

#### Decoupled process and outcome with taxonomy

Several products can score process and outcome separately, and Patronus offers an explicit error taxonomy. The whitespace is a **vendor-neutral, independently validated, portable classification** with first-error localization, evidence, confidence, and calibration - not the mere existence of two scores.

### Market Share and Concentration

Reliable vendor market-share data is not available. GitHub stars, package downloads, and vendor customer claims measure different phenomena and should not be converted into share.

The best current directional evidence is VentureBeat's June 2026 survey of 157 enterprises with 100+ employees:

- OpenAI native evals/traces: 17%
- No dedicated evaluation tool: 17%
- Anthropic native evals: 13%
- DeepEval: 12%
- Braintrust: 8%
- LangSmith, Weave, Promptfoo, Langfuse, and Arize: low-single-digit to low-double-digit fragments
- In-house tooling: 11%
- 64% intended to adopt, add, or replace a platform within twelve months

This is a small, single-wave, publication-sponsored survey and should not be treated as definitive global share. It does support the conclusion that the category is fragmented and lacks an independent standard. [VentureBeat Pulse Research, 2026](https://venturebeat.com/ai/the-agent-evaluation-gap-enterprise-ai-organizations-have-a-reality-alignment-problem-not-a-coverage-problem-and-most-are-shipping-to-production-anyway)

### Competitive Positioning Map

| Position | Representative players | Buyer motivation | eval-quality response |
|---|---|---|---|
| Provider-native convenience | OpenAI and model providers | Minimal integration | Remain provider-neutral and exportable |
| Framework-native convenience | LangSmith, Mastra, Eve | Best fit with chosen agent runtime | Supply adapters without requiring framework migration |
| Broad local CI and security | Promptfoo, DeepEval | Fast regression testing and red teaming | Interoperate; differentiate on evidence semantics |
| RAG metric library | Ragas, Phoenix, Autoevals | Grounding and retrieval quality | Move from context-level score to claim-level evidence graph |
| Hosted evaluation platform | Braintrust, Galileo, Patronus, Confident AI | Collaboration, analytics, production loop | Feed the Evidence Platform; do not race every dashboard |
| Open/self-host observability | Langfuse, Phoenix, Weave | Data control and trace visibility | Consume and enrich their traces |
| Research execution harness | Inspect AI, OpenAI Evals | Reproducible benchmarks and sandboxes | Export scorers/artifacts rather than rebuild harnesses |
| Portable process evidence | **eval-quality target** | Defensible trajectory validity across stacks | Own evidence graph, path invariance, 2×2 diagnosis, and methodology |

### What eval-quality Can Borrow

Licensing must be checked at file/package level before reuse; this is an engineering assessment, not legal advice.

| Source | License | Reusable pattern | Improvement for eval-quality |
|---|---|---|---|
| Promptfoo | MIT | Canonical trajectory normalization, composable grading result, CI reporters | Typed code-first DSL; retain raw source events and evidence IDs |
| DeepEval | Apache-2.0 | Process/outcome metric decomposition, trace-required metric types | Deterministic-first semantics; avoid opaque task extraction |
| Ragas | Apache-2.0 | Strict plus precision/recall/F1 tool scoring; claim decomposition | Support multiple valid paths and preserve claim→evidence links |
| Inspect AI | MIT | Event/log/scorer separation, recorder abstraction, external-agent bridges | Port the architecture, not the Python runtime |
| Braintrust SDK / Autoevals | Apache-2.0 / MIT | Typed agent assertions and grounding scorers | Trace-wide evidence lineage and portable results |
| AgentEvals | MIT | Strict/unordered/subset/superset matching and graph extraction | Partial orders, checkpoints, alternatives, state constraints |
| Phoenix TS evals | Apache-2.0 | Evaluator contracts, binding/mapping, tool and grounding metrics | Stabilize API; avoid ELv2 platform/Python code |
| Langfuse core | MIT | Score model, evaluator analytics, experiment patterns | Portable calibration and CI-readable judge agreement |
| Mastra core/evals | Apache-2.0 | Quick checks, gates, dataset-versioned tool mocks | Framework-neutral schema and fail-closed replay |
| Vercel AI SDK | Apache-2.0 | OTel model/tool telemetry contract | High-quality first adapter and normalized event model |
| Vercel Eve | Apache-2.0 | Gate/soft severity, reporters, local/remote test ergonomics | Remove Eve runtime assumptions |
| W&B Weave | Apache-2.0 | Repeated trials, bounded concurrency, result aggregation | Add process evidence and deterministic trajectory semantics |
| Patronus TRAIL runner | MIT | Reasoning/execution/planning error framing | Keep gated/noncommercial datasets external |

Do not reuse:

- Humanloop's unlicensed public SDK code
- Langfuse enterprise-directory code
- Phoenix ELv2 platform/Python evaluator code in a hosted competitive service without legal review
- Patronus noncommercial models or restricted datasets as default commercial dependencies
- OpenAI registry datasets without checking each dataset's license

### eval-quality SWOT

#### Strengths

- Narrow, explainable thesis centered on observable path evidence
- TypeScript-native and local-first architecture
- Vendor-neutral role across agent frameworks and trace platforms
- Ability to make deterministic evidence primary and judges secondary
- Natural alignment with CI, audits, and a larger Evidence Platform

#### Weaknesses

- New project without adoption, benchmark evidence, or evaluator calibration
- Trace normalization across providers is difficult and fast-moving
- Evidence lineage depends on sufficient source/tool metadata
- A credible taxonomy requires domain validation and human labels
- TypeScript alone is no longer differentiated

#### Opportunities

- Fragmented market with no independent standard
- Observability-to-evaluation adoption gap
- OpenAI Evals and Humanloop migrations reinforce portability
- Rising enterprise need for auditable AI evidence
- Existing platforms can distribute eval-quality through adapters
- Vendor-neutral methodology and certification remain structurally open

#### Threats

- Mastra, DeepEval, Braintrust, and Promptfoo can ship overlapping TS features quickly
- Patronus and Galileo already market sophisticated agent-process evaluation
- Provider and framework owners can bundle “good enough” evaluation
- OpenTelemetry GenAI conventions may absorb part of the evidence schema
- Uncalibrated trajectory judges could damage trust in the category
- Certification launched before employer demand would create a low-value vendor badge

### Market Differentiation

eval-quality should not claim:

- the first trajectory evaluator
- the only TypeScript evaluation framework
- access to a model's true reasoning
- that process/outcome separation is absent elsewhere
- that a single score can establish reasoning correctness

It can credibly claim:

1. **Observable evidence, not hidden chain-of-thought**  
   Evaluate claims, actions, tool results, citations, state transitions, and declared rationale.

2. **Path-invariant constraints**  
   Required/forbidden actions, partial ordering, semantic checkpoints, argument/state predicates, alternative branches, and multiple evidence sources.

3. **First-class process × outcome diagnosis**  
   Sound success, lucky/unsafe success, sound failure, and process failure.

4. **First-material-error attribution**  
   Identify the earliest unsupported claim, invalid transition, bad tool decision, or contradiction that affected the result.

5. **Claim-to-evidence graph**  
   Preserve the specific lineage behind each verdict rather than returning only “faithfulness: 0.82.”

6. **Deterministic-first escalation**  
   Structural, permission, schema, budget, provenance, and policy checks precede optional semantic judges.

7. **Portable evidence artifact**  
   Stable JSON plus JUnit/SARIF/human reports containing findings, source spans, uncertainty, versions, cost, and reproduction metadata.

### Competitive Threat Prioritization

1. **Mastra:** closest OSS TypeScript developer experience
2. **DeepEval:** strongest metric vocabulary and rapidly expanding TS implementation
3. **Braintrust:** mature TypeScript agent assertions and complete hosted workflow
4. **Promptfoo:** broad adoption funnel, red-team strength, and OpenAI backing
5. **Patronus:** closest process-diagnosis and error-taxonomy thesis
6. **Galileo:** strongest ready-made commercial agent metric suite
7. **LangSmith/AgentEvals:** established trajectory matching and LangGraph distribution

### Competitive Recommendation

Do not build another observability database or generic judge catalog first. Build a small independent evaluation kernel that can sit above existing traces and below any evidence platform:

```text
Agent/runtime traces
        ↓
eval-quality normalization and evidence graph
        ↓
deterministic constraints + calibrated semantic checks
        ↓
portable process/outcome evidence artifact
        ↓
CI, Langfuse, Phoenix, Braintrust, LangSmith, Weave, or Evidence Platform
```

**Competitive confidence:** High on licenses, product positioning, and broad capability coverage where verified in primary sources; medium on rapidly changing TypeScript parity and pricing; low on market-share precision beyond the cited directional survey.

---

# Research Synthesis: Executive Summary and Strategy

## Executive Summary

Agent engineering has moved into production faster than evaluation practice has matured. LangChain's late-2025 survey of 1,340 practitioners found 57.3% had agents in production, 89% had observability, but only 52.4% ran offline evaluations and 37.3% ran online evaluations. Quality was the leading production barrier. This validates a real evaluation gap, though the survey's technology-sector and small-company skew limits generalization. [LangChain, 2026](https://www.langchain.com/state-of-agent-engineering)

The original eval-quality thesis is directionally right:

- Output-only evaluation cannot distinguish a sound success from a lucky or unsafe success.
- Exact golden-path matching is brittle because several valid paths may reach the same goal.
- Observable actions and claims should be assessed against evidence.
- Process quality and outcome quality should remain independent.

However, the category is more advanced than the initial framing implied. Promptfoo, DeepEval, Braintrust, LangSmith/AgentEvals, Phoenix, Galileo, Mastra, Patronus, Langfuse, and Weave already expose combinations of trajectory checks, tool evaluation, grounding, LLM judges, and process/outcome primitives. TypeScript evaluation is also no longer rare.

The recommended strategic position is therefore:

> **For teams shipping tool-using AI agents, eval-quality is the local-first evidence engine that determines whether an agent succeeded for grounded, reproducible reasons - and identifies where its observable path first became unreliable.**

eval-quality should evaluate **observable execution evidence**, not claim access to a model's true internal reasoning. Anthropic's research shows stated chain-of-thought can omit causal hints or rationalize answers after the fact. [Anthropic, 2025](https://www.anthropic.com/research/reasoning-models-dont-say-think)

### Headline Market Model

- **2026 commercial TAM:** approximately **$0.5B**, with a modeled range of **$0.3–1.0B**
- **Initial SAM:** approximately **$180M**
- **Illustrative five-year SOM:** approximately **$10M ARR**, with a scenario range of **$3–25M**
- **Current SOM:** effectively zero; eval-quality is an early project without validated adoption

These figures are scenario estimates, not published measurements of a recognized “agent trajectory evidence” category. Market definitions overlap heavily across LLM evaluation, observability, governance, LLMOps, and assurance.

### Strategic Sequence

1. OSS TypeScript evidence kernel
2. Portable evidence schema and ecosystem adapters
3. Managed Evidence Platform
4. Open, validated methodology
5. Independently governed practitioner certification
6. Separate system-assurance scheme only if market and standards maturity justify it

## Table of Contents

1. [Research Introduction and Methodology](#1-research-introduction-and-methodology)
2. [Market Analysis and TAM](#2-market-analysis-and-tam)
3. [Target Segments and Customer Insights](#3-target-segments-and-customer-insights)
4. [Competitive Positioning](#4-competitive-positioning)
5. [Product and Business Strategy](#5-product-and-business-strategy)
6. [Go-to-Market and Growth](#6-go-to-market-and-growth)
7. [Methodology and Certification](#7-methodology-and-certification)
8. [Risk Assessment](#8-risk-assessment)
9. [Implementation Roadmap](#9-implementation-roadmap)
10. [Success Metrics](#10-success-metrics)
11. [Future Outlook](#11-future-outlook)
12. [Research Quality and Limitations](#12-research-quality-and-limitations)
13. [Final Recommendations](#13-final-recommendations)

## 1. Research Introduction and Methodology

### Why This Market Matters

Agent failures are not equivalent. Four cases require different remedies:

| Outcome | Process | Meaning |
|---|---|---|
| Correct | Sound | Reliable success |
| Correct | Unsound | Lucky, brittle, unsafe, or non-reproducible success |
| Incorrect | Sound | Capability, evidence, environment, or terminal-step limitation |
| Incorrect | Unsound | Process failure requiring root-cause correction |

AgentLens provides domain-specific evidence for this distinction. In its eligible SWE-agent subset, 10.7% of passing trajectories were classified as “Lucky,” and model rankings changed when process quality replaced pass rate. The study has important caveats: labels came from AgentLens thresholds, some “Lucky” paths were divergent-but-valid, and the evidence is limited to one software-engineering setting. [Microsoft Research, 2026](https://www.microsoft.com/en-us/research/publication/agentlens-revealing-the-lucky-pass-problem-in-swe-agent-evaluation/)

TRACE demonstrates reference-free trajectory analysis using an evidence bank for efficiency, hallucination, and adaptivity. It is a valuable design prior, but synthetic fault injection, dependence on LLM judges, exposed thought fields, and narrow recovery tests prevent treating it as a solved production method. [TRACE, arXiv:2510.02837](https://arxiv.org/abs/2510.02837)

### Method

- **Scope:** global agent-evaluation tooling, with an initial North American and European commercial focus
- **Time:** current through July 17, 2026
- **Sources:** official docs, repositories, license files, standards bodies, research papers, product announcements, surveys, and analyst estimates
- **Framework:** market segmentation, buyer journey, capability matrix, SWOT, TAM/SAM/SOM scenarios, competitive positioning, risk analysis, and phased GTM
- **Evidence treatment:** facts, modeled assumptions, and recommendations are separated; uncertain claims receive confidence caveats

## 2. Market Analysis and TAM

### External Market Bounds

Published estimates use conflicting definitions:

| Adjacent category | 2026 estimate | Limitation |
|---|---:|---|
| Agentic monitoring/analytics/observability tools | $1.1B | Includes broad monitoring, not only evaluation |
| Agent observability and governance | $1.68B | Mixes observability, governance, software, and services |
| LLM observability platforms | $2.69B | Includes prompt, latency, token, cost, and runtime monitoring |

Sources: [Research and Markets, 2026](https://www.researchandmarkets.com/reports/6244028/agentic-ai-monitoring-analytics-observability), [Mordor Intelligence, 2026](https://www.mordorintelligence.com/industry-reports/agent-observability-and-governance-market), [Research and Markets LLM Observability, 2026](https://www.researchandmarkets.com/reports/6215671/large-language-model-llm-observability)

These values must not be summed. They are bounding references with overlapping scope and generally paywalled methodologies.

### Top-Down Scenario

Formula:

```text
commercial TAM =
  evaluation-market anchor × trajectory/evidence relevance
  + assurance-market anchor × methodology relevance
  − overlap
```

| Input | Conservative | Base | Upside |
|---|---:|---:|---:|
| Evaluation anchor | $1.4B | $1.9B | $2.4B |
| Relevant trajectory/evidence share | 20% | 30% | 40% |
| Agent-assurance anchor | $0.6B | $0.6B | $0.6B |
| Relevant methodology share | 10% | 20% | 35% |
| Overlap haircut | 15% | 15% | 15% |
| **Modeled TAM** | **~$0.29B** | **~$0.59B** | **~$0.98B** |

Rounded headline: **$0.5B**, with **$0.3–1.0B** bounds.

### Bottom-Up Cross-Check

The base case models large and mid-market organizations likely to operate agents and pay for centralized evidence:

| Segment | Illustrative formula | Revenue pool |
|---|---|---:|
| Large enterprises | 76K accounts × 20% agent-active × 40% paid fit × $75K ACV | ~$460M |
| Mid-market | 886K × 15% relevant verticals × 6% agent-active × 20% paid fit × $25K | ~$40M |
| Methodology/assurance | 25K eligible organizations × 10% adoption × $20K | ~$50M |
| Overlap haircut | 15% | (~$80M) |
| **Bottom-up base** | | **~$470M** |

The account counts combine differently defined U.S. and EU size bands and are suitable only for order-of-magnitude modeling. [U.S. Census Bureau](https://www.census.gov/library/stories/2026/05/small-business-week.html) [Eurostat](https://ec.europa.eu/eurostat/en/web/products-eurostat-news/w/ddn-20251209-2)

### SAM and SOM

Initial SAM assumes English-first North American and European selling, TypeScript-led or API-compatible teams, and evidence-sensitive software, financial-services, healthcare, and professional-services use cases.

| Scenario | TAM | Effective serviceability | SAM |
|---|---:|---:|---:|
| Conservative | $0.3B | 20% | ~$60M |
| Base | $0.5–0.6B | ~33% | **~$180M** |
| Upside | $1.0B | 45% | ~$440M |

Illustrative five-year ARR:

| Scenario | Customer mix | ARR |
|---|---|---:|
| Conservative | 25 enterprise, 50 mid-market, limited methodology | ~$3–4M |
| Base | 70 enterprise, 100 mid-market, methodology revenue | **~$10M** |
| Upside | 150 enterprise, 200 mid-market, scaled methodology | ~$25M |

### Market Drivers

- Production-agent growth and quality failures
- Observability adoption outpacing evaluation
- Multi-model and multi-framework environments
- Security, privacy, and data-residency requirements
- Procurement demand for portable evidence
- NIST emphasis on objective, repeatable, documented TEVV
- ISO/IEC 42001 performance evaluation, monitoring, and continual improvement
- Regulatory requirements for logs, documentation, oversight, robustness, and accuracy in applicable high-risk contexts

[NIST AI RMF Core](https://airc.nist.gov/airmf-resources/airmf/5-sec-core/) [ISO/IEC 42001](https://www.iso.org/standard/42001) [EU AI Act overview](https://digital-strategy.ec.europa.eu/en/policies/regulatory-framework-ai)

## 3. Target Segments and Customer Insights

### Beachhead

**TypeScript AI product teams already collecting traces but lacking repeatable trajectory regression tests.**

Why:

- TypeScript became GitHub's most-used language by monthly contributors in August 2025.
- Agent application development is often closer to web/product engineering than model research.
- The segment values npm-native setup, types, CI, local execution, and framework adapters.

[GitHub Octoverse, 2025](https://github.blog/news-insights/octoverse/octoverse-a-new-developer-joins-github-every-second-as-ai-leads-typescript-to-1/)

### Expansion Buyer

**Central AI evaluation and platform teams** supporting multiple applications, models, and frameworks.

They pay for:

- shared evidence inventory
- versioned datasets and evaluator registries
- approvals and waivers
- cross-run analysis
- online feedback loops
- access controls, retention, and audit evidence
- support and operational accountability

### High-Value Enterprise Segment

**Regulated and high-consequence teams** in financial services, healthcare, public-sector, and customer-facing automation.

They need:

- local/private processing
- data minimization
- reproducible evidence
- deterministic policy and permission checks
- human review and segregation of duties
- stable exports and long-term retention

### Methodology Segment

Researchers, consultancies, training providers, QA leaders, internal enablement teams, and assurance professionals can distribute and validate the methodology. Certification demand remains unproven and should not be included as a guaranteed revenue stream.

## 4. Competitive Positioning

### Category Position

eval-quality should create a narrow category:

> **Agent Evidence Assurance**

It should not initially position as:

- another trace database
- a generic LLM evaluation suite
- a benchmark leaderboard
- a complete governance platform
- a certifier

### Differentiated Wedge

1. **Evidence-grounded observable-step checks**  
   Map externally verifiable claims and actions to tool results, retrieved passages, observations, and policies.

2. **Path invariance**  
   Use semantic checkpoints, partial orders, required/forbidden actions, state predicates, alternative tools, and multiple valid branches.

3. **Process × outcome classification**  
   Preserve the four quadrants instead of collapsing them into one score.

4. **First-material-error attribution**  
   Identify the earliest unsupported claim, invalid transition, bad tool decision, or contradiction that affected the result.

5. **Deterministic-first escalation**  
   Run structural, permission, schema, budget, provenance, and policy checks before semantic judges.

6. **Evaluator validity**  
   Version judges and rubrics; report repeated-run stability, disagreement, human calibration, and abstention.

7. **Portable evidence**  
   Emit stable JSON plus JUnit, SARIF, and human-readable findings.

### Evidence Platform Role

```text
Agent and runtime traces
        ↓
eval-quality evidence production
        ↓
portable signed evidence artifact
        ↓
CI / existing observability tools / managed Evidence Platform
        ↓
release, review, procurement, governance, and audit decisions
```

The OSS engine wins through adoption and interoperability. The Evidence Platform monetizes coordination, history, policy, governance, and support.

## 5. Product and Business Strategy

### OSS Engine

The open engine should own:

- canonical provider-neutral event model
- TypeScript assertion DSL
- OTel/OpenInference and framework adapters
- process and outcome results
- evidence graph
- repeated-trial statistics
- local reports and CI exit codes
- signed or hash-verifiable evidence manifests
- custom evaluator interface
- optional model-judge adapters

Core methodology should remain open and auditable. Proprietary scoring would weaken assurance credibility.

### Managed Evidence Platform

Paid value should come from convenience and organizational coordination:

- multi-repository inventory
- longitudinal comparisons and drift
- centralized datasets and evaluator versions
- approvals, waivers, review history, and segregation of duties
- policy packs and control crosswalks
- retention, search, and evidence rooms
- private runners and minimized-data upload
- SSO, SCIM, RBAC, audit logs, residency, and support

### Pricing Assumptions

Planning bands - not observed eval-quality prices:

| Offering | Assumed annual range |
|---|---:|
| Developer/team managed service | $5K–20K |
| AI-native startup or mid-market | $20K–50K |
| Enterprise Evidence Platform | $60K–150K |
| Private cloud/on-prem | $150K–400K |
| Practical practitioner assessment | $650–1,200 per candidate |
| Organizational methodology assessment | $10K–50K |

Avoid seat-only pricing for the platform. Governed applications, evidence volume, retention, and private deployment align more closely with value.

## 6. Go-to-Market and Growth

### Entry Motion

Use developer-led OSS adoption followed by sales-assisted enterprise conversion:

1. Developer discovers and installs eval-quality.
2. The library catches a failure current tests missed.
3. The team adds CI gates and shared evaluators.
4. Multiple repositories create coordination and evidence-retention needs.
5. Platform/security/governance teams sponsor a managed pilot.
6. Enterprise purchase adds collaboration, assurance, and controls.

Menlo Ventures documents this bottom-up AI-tool pattern: technical users prove value before demand becomes an enterprise contract. [Menlo Ventures, 2025](https://menlovc.com/perspective/2025-the-state-of-generative-ai-in-the-enterprise/)

### Initial Acquisition Channels

- GitHub and npm
- Vercel AI SDK, Mastra, LangChain, OpenAI Agents, and OTel adapters
- framework-specific failure examples
- practitioner research and reproducible benchmark studies
- OWASP Agentic Security and OpenTelemetry communities
- consultancies and AI-assurance design partners
- GitHub Actions and later GitLab/Jenkins integrations

### Activation Event

The activation event is not installation. It is:

> **The first diagnosis on the user's own trace that they accept and could not obtain as quickly from existing output tests or dashboards.**

### Enterprise Bridge

Provide the champion:

- one-page business case
- architecture and data-flow diagrams
- privacy and security answers
- competitor comparison
- pilot scorecard
- pricing logic and TCO model
- trust center
- evidence export/exit guarantee

## 7. Methodology and Certification

### Validated Whitespace

ISTQB officially announced more than one million certificates in May 2025. CT-AI v2, released in 2026, covers broad AI/ML-system testing, GenAI/LLM black-box testing, red teaming, data testing, model testing, statistical testing, drift, and metamorphic testing. It does **not** provide a dedicated agent-evaluation methodology for trajectories, tool use, memory/state, orchestration, recovery, repeated-run agent reliability, or evaluator calibration. [ISTQB milestone](https://istqb.org/a-global-milestone-in-software-quality-1000000-istqb-certificates/) [CT-AI v2](https://istqb.org/certifications/certified-tester-ai-testing-ct-ai/)

Current offerings from Maven, Evidently, Arize/DeepLearning.AI, Braintrust, Galileo, LangChain, W&B, OpenAI, and Anthropic provide valuable training, but most award completion credentials or product education. They do not combine:

- vendor-neutral body of knowledge
- independent certification governance
- substantial hidden practical assessment
- employer-recognized role definition
- recurring competence maintenance

### The Missing Role Standard

A credible agent-evaluation practitioner should demonstrate:

- evaluation-objective and risk definition
- agent-system decomposition
- trajectory instrumentation
- representative scenario design
- deterministic, human, model, and simulation evaluators
- judge calibration and statistical validity
- repeated-run reliability analysis
- tool/argument/result/side-effect testing
- planning, recovery, memory, and multi-agent analysis
- security and least-agency testing
- online monitoring and incident feedback
- reproducible evidence packages and decision reports

### Phased Certification Strategy

1. **Open methodology:** public glossary, competency map, evidence model, and RFC process
2. **Assessment-based certificate:** training plus graded practical capstone; do not call it professional certification
3. **Independent beta certification:** separate training from assessment; hidden practical environment and proctored knowledge component
4. **Provider ecosystem:** approved training providers, independent exam delivery, registry, appeals, and recertification
5. **Accreditation:** seek ISO/IEC 17024 alignment for persons only after demand and psychometric validity
6. **System assurance:** separate later scheme under appropriate product/process certification governance

### Moat

The moat should not be a secret rubric. It should be:

- open method with difficult-to-reproduce validation
- curated failure-trajectory corpus
- calibrated evaluator and human-disagreement data
- accepted scenario packs
- longitudinal link between gates and incidents
- employer and auditor recognition
- trusted practitioner network
- compatibility testing and marks

## 8. Risk Assessment

| Risk | Probability / impact | Mitigation |
|---|---|---|
| Incumbents bundle trajectory evaluation | High / High | Interoperate with their traces; own portable assurance semantics |
| TypeScript differentiation disappears | High / Medium | Treat TS as entry wedge; maintain language-neutral evidence schema and add Python ingestion |
| Evaluation does not predict production safety | High / Critical | Scope results to intended use and tested scenarios; retain uncertainty and post-deployment feedback |
| LLM-judge drift and bias | High / High | Deterministic-first checks, pinned versions, calibration sets, disagreement, abstention |
| Sensitive trajectory leakage | High / Critical | Local processing, metadata-only defaults, redaction, private runners, explicit retention |
| OSS adoption does not convert | Medium / High | Monetize collaboration and governance; instrument organizational expansion signals |
| Certification lacks credibility | High / High | Independent governance, practical assessment, employer validation, accurate naming |
| Liability from PASS decisions | Medium / Critical | Say “evidence-supported within declared scope,” not safe/certified; maintain waivers and limitations |
| Standards change | High / Medium | Version internal schema independently; preserve original inputs; publish migrations |
| OSS supply-chain compromise | Medium / Critical | Signed releases, SBOM, provenance, pinned actions, reproducible builds, disclosure policy |
| Methodology overclaims reasoning | Medium / High | Evaluate observable evidence and behavior; never claim faithful internal cognition |

### Regulatory Caveat

Do not sell primarily through deadline fear. Regulatory timelines, classifications, and obligations change, and not every agent is high-risk. Sell reduced review effort, reproducible release decisions, and procurement evidence.

## 9. Implementation Roadmap

### Phase 0 - Prove the Wedge: 0–3 Months

Build:

- TypeScript core and `Expect.*` DSL
- canonical trace schema
- AI SDK/OpenTelemetry and one additional framework adapter
- tool authorization, evidence grounding, and cost/latency packs
- process/outcome result
- JSON and JUnit output
- GitHub Action

Validate with 8–12 design partners.

Exit criteria:

- median time to first accepted result under 15 minutes
- 10 organizations repeatedly run eval-quality in CI
- five real incidents become regression fixtures
- three evidence artifacts are used outside engineering
- 30% four-week repository retention

### Phase 1 - Establish Evidence Format: 3–9 Months

Add:

- OpenInference and broader OTel ingestion
- Python-compatible manifest/runner protocol
- semantic checkpoints and partial-order constraints
- first-material-error attribution
- judge calibration and disagreement
- baseline/candidate comparison
- signed evidence bundles
- public methodology v1
- NIST, OWASP, and selected regulatory crosswalks

Exit criteria:

- 50 organizations with repeated monthly runs
- 15 release-gating organizations
- five paid managed pilots
- three procurement or audit outcomes
- at least two independent trace sources

### Phase 2 - Managed Evidence Platform: 9–18 Months

Add:

- portfolio inventory
- shared evaluator/dataset registry
- approvals, waivers, and review
- private runners and minimized-data architecture
- SSO/RBAC/audit/retention/residency
- GRC and work-management integrations

Exit criteria:

- evidence used in ten formal release/risk decisions
- greater than 120% net retention among initial managed customers
- 25% of active OSS organizations attach to managed service
- two independent advisory/audit partners accept the output format

### Phase 3 - Assurance Ecosystem: 18–36 Months

- independent advisory and scheme governance
- practical assessment pilot
- public validity studies
- approved evaluator/training partners
- registry, appeals, surveillance, and recertification

## 10. Success Metrics

### Developer Value

- time to first accepted evidence
- activation rate
- four-week repeated-run rate
- gated PRs per active repository
- production failures converted into regression cases

### Evidence Quality

- claim-to-source traceability
- seeded-failure detection
- false-pass and false-fail rates
- first-error localization accuracy
- rerun reproducibility
- judge/human agreement
- percentage of runs with complete provenance

### Enterprise Conversion

- active OSS organizations connecting to managed service
- pilot-to-paid conversion
- review hours saved
- evidence bundles consumed outside engineering
- expansion from one to multiple agent applications

### Methodology Moat

- external tools consuming/emitting the evidence format
- independent auditors accepting artifacts
- contributed scenario packs
- practitioner-assessment validity
- production incidents represented in the corpus

GitHub stars and downloads are distribution indicators, not primary success metrics.

## 11. Future Outlook

### Near Term: 1–2 Years

- Tool/trajectory assertions and generic LLM judging become standard features.
- OpenTelemetry and framework trace schemas converge but remain incomplete.
- TypeScript parity improves across Python-first vendors.
- Consolidation and provider bundling continue.
- Buyers increasingly demand evaluator provenance and calibration.

### Medium Term: 3–5 Years

- Evidence formats begin to connect engineering evaluation with governance and audit.
- Domain-specific scenario packs and simulation become more valuable.
- Independent evaluation methods gain importance as model providers own more tooling.
- Practical agent-evaluation roles and credentials may emerge through ISTQB, IEEE, Linux Foundation, CSAI, or new schemes.

### Long Term

The durable opportunity is not one evaluator. It is a trust network around comparable evidence:

- open schemas
- validated methods
- calibrated scenario corpora
- accepted practitioner competence
- interoperable evidence artifacts
- institutional recognition

## 12. Research Quality and Limitations

### High-Confidence Findings

- Agent quality and evaluation maturity are real problems.
- Observability is substantially more adopted than systematic evaluation.
- Exact-path grading is brittle.
- Stated chain-of-thought cannot be assumed faithful.
- Tool checks, holistic judges, and basic trajectory metrics are crowded.
- TypeScript is a viable but non-unique beachhead.
- Certification credibility requires independence and practical validation.

### Medium-Confidence Findings

- Claim-to-evidence lineage and semantic path invariance form a differentiated product wedge.
- Platform teams are the strongest expansion buyer.
- A managed Evidence Platform can monetize OSS adoption.
- Methodology can become a moat if institutionally accepted.

### Low-Confidence Findings

- Exact TAM/SAM/SOM values
- willingness to pay specifically for trajectory evidence
- certification demand and pricing
- budget ownership across engineering, platform, security, and governance
- five-year market-share outcomes

### Required Primary Validation

- 25–40 customer and employer interviews
- design-partner telemetry
- comparative proof against Promptfoo, Mastra, DeepEval, Braintrust, and AgentEvals
- human-labeled evaluator-calibration set
- willingness-to-pay and budget-owner interviews
- employer tests of proposed practitioner credential

## 13. Final Recommendations

### Build

1. A small TypeScript-native local evaluation kernel
2. A language-neutral evidence schema
3. AI SDK/OpenTelemetry adapters
4. Claim-to-evidence graph
5. Semantic checkpoints and partial-order constraints
6. Process × outcome classification
7. First-material-error attribution
8. Deterministic-first evaluator cascade
9. Portable JSON/JUnit/SARIF evidence
10. Calibration and disagreement reporting

### Integrate

- Langfuse
- Phoenix
- Braintrust
- LangSmith
- W&B Weave
- Inspect AI
- Mastra
- Vercel AI SDK

### Borrow Carefully

Use permissively licensed patterns from Promptfoo, AgentEvals, Braintrust Autoevals/SDK, Phoenix TypeScript evals, Mastra core/evals, Inspect AI, Ragas, Weave, and Vercel. Preserve notices and perform package/file-level license review.

### Avoid

- full observability-platform scope at launch
- a generic metric catalog
- exact golden-path positioning
- chain-of-thought grading claims
- proprietary evidence transport
- certification before employer validation
- regulation-only sales messaging

### Final Strategic Assessment

eval-quality has a credible opportunity, but not because trajectory evaluation is empty or TypeScript tooling is absent. Its opportunity is to make **observable agent-process evidence portable, path-invariant, calibrated, and useful in release decisions**.

The OSS engine should earn trust and distribution. The Evidence Platform should monetize organizational coordination. The methodology should be open enough to audit. Certification should become a moat only after independent governance and demonstrated labor-market value.

---

## Market Research Conclusion

The market is fragmented, growing, and consolidating. Incumbents own traces, dashboards, generic judges, and increasingly sophisticated trajectory checks. No incumbent yet owns a broadly accepted, vendor-neutral standard for proving that an agent's observable execution was grounded, permissible, reproducible, and sound independently of its final answer.

That is the eval-quality opening.

**Market Research Completion Date:** 2026-07-17  
**Research Period:** Current market snapshot through 2026-07-17  
**Source Verification:** Primary sources prioritized; assumptions and limitations identified  
**Overall Confidence:** Medium-high on the problem and refined wedge; low-to-medium on market sizing and certification economics
