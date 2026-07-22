# eval-quality

**Evidence-driven quality decisions for AI agents.**

Existing eval engines already execute agents, capture trajectories, run standard checks, and report results. `eval-quality` is testing two product ideas: independent black-box evaluation for human-on-the-loop and dark-factory delivery, plus four semantic evaluators that may add meaningful evidence.

> Status: hypothesis validation. API not yet published. The standalone engine scope is paused. Execute [HYPOTHESIS_VALIDATION_PLAN.md](HYPOTHESIS_VALIDATION_PLAN.md) before production implementation.

The current direction uses an established open-source eval engine for execution, traces, reports, and CI. `eval-quality` owns the agents, rules, methodology, governance, and any semantic evaluator extensions that pass the validation gate.

The independent-evaluation hypothesis uses a sealed Eval Contract created by TEA before implementation. The evaluator receives that contract and black-box system access. The original spec, source code, repository, and builder context remain outside its workspace. The [hypothesis validation plan](HYPOTHESIS_VALIDATION_PLAN.md) defines this provisional research boundary.

## Why

Planning and validation are the expensive, high-value ends of AI-assisted delivery. Agent behavior remains difficult to diagnose because a correct answer can come from unsupported claims or an unsound path, while an incorrect answer can follow a sound process over bad evidence.

`eval-quality` treats agent evaluation as an **evidence source** for a ship / don't-ship decision. The hypothesis experiment determines whether its proposed evaluator layer adds useful evidence beyond mature baseline tools.

## Candidate evaluator capabilities

- **Claim-to-evidence lineage:** identify material claims that lack support in the agent's observed evidence.
- **Semantic checkpoints:** accept valid alternate paths while detecting skipped dependencies and unsound actions.
- **Process and outcome separation:** distinguish lucky success, sound failure, and ordinary success or failure.
- **First material error attribution:** identify the earliest action that caused the later failure.

These are hypotheses. Existing tools provide the execution engine. Only capabilities that pass the [validation plan](HYPOTHESIS_VALIDATION_PLAN.md) will enter the production scope.

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
