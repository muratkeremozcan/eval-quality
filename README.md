# eval-quality

**Agent evaluation for the reasoning path, not just the output.**

Most eval tools grade the final answer. `eval-quality` grades how the agent *got there* - the trajectory: which tools it called, in what order, whether each claim in its answer traces back to real evidence, and whether it reached a sound conclusion for sound reasons. A right answer reached by luck and a wrong answer reached by good reasoning on bad data are different failures, and output-only scoring can't tell them apart.

> Status: early scaffold. API not yet published.

## Why

Planning is expensive. Validation is expensive. Code generation in the middle is cheap. The hard, high-value ends of AI-assisted delivery are planning and validation - and agent behavior, whether it reasoned soundly rather than just answered correctly, is the part of validation that TypeScript-native tooling still underserves.

`eval-quality` treats agent-eval as an **evidence source**, not a benchmark number. Its output is meant to feed a ship / don't-ship decision (PASS / CONCERNS / FAIL), not to report "the agent got 87%."

## What it will do

- **Runner** - drive an agent (provider-agnostic) against a task and capture the full transcript.
- **Assertions** - a typed `Expect.*` DSL for tool calls, sequence, params, and performance budgets.
- **Graders** - deterministic-first, LLM-judge-second, with pass@k aggregation for non-determinism.
- **Trajectory eval** (the differentiator) - faithfulness / grounding, path quality, process-vs-outcome judging, and reference-trajectory scoring.
- **Evidence artifacts** - a human summary plus a machine-readable result for gates and CI.

## Development

```bash
npm install
npm run validate    # typecheck + lint + test
npm run build       # emit to dist/
npm run lint:fix    # auto-fix with Biome
```

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md) and our [Code of Conduct](CODE_OF_CONDUCT.md).

## Security

See [SECURITY.md](SECURITY.md). Please do not open a public issue for vulnerabilities.

## License

Apache-2.0 &copy; Murat Ozcan. See [LICENSE](LICENSE).
