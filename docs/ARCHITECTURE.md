# Architecture

Status: proposed. No modules are implemented.

## Pipeline

Configuration validation → Markdown selection → environment creation → installation → execution → assertions → reporting → cleanup.

Suggested package layout: `src/firstrun/` containing `cli.py`, `config.py`, `markdown.py`, `environment.py`, `runner.py`, and `results.py`. Fixture wheels and process helpers belong under `tests/fixtures/`.

Use standard-library `argparse`, `tomllib`, `venv`, and `subprocess`; evaluate a maintained Markdown parser for source locations. Use pytest for regression tests. Pin development dependencies in a reproducible lockfile when implementation begins. Do not invent an executable package skeleton before the manifest/runner contract is agreed.

## Decisions

1. **Installed distribution first.** Never substitute editable installation. This preserves the failure boundary the tool exists to test.
2. **Explicit example IDs.** A missing selection is an error, avoiding false-green checks after documentation changes.
3. **One interpreter per run.** Delegate interpreter installation and OS matrices to existing CI systems.
4. **Direct Python execution.** Avoid shell quoting differences; shell support is outside v0.1.
5. **Fresh workspace per example.** Share installation cost without implicit file/state dependencies.
6. **Structured results first.** Console rendering consumes the same result model as JSON.
7. **No sandbox claim.** Package installation and examples execute arbitrary code with runner privileges.
8. **No initial cache.** Favor correctness; measure runtime before introducing cache invalidation risks.

## Implementation risks and spikes

- Process-tree cancellation differs by OS. Compare Windows Job Objects and suitable maintained libraries; use process groups on POSIX where appropriate. Test descendants, not merely the direct child. Choose the implementation before promising release dates.
- Environment allowlisting must retain necessary Windows runtime variables while excluding secrets and developer path leakage. Define the exact allowlist and test it with sentinel variables.
- pip defaults and user config can alter installation. Use isolated configuration, public-index defaults, and an explicit local-fixture test path. Confirm behavior against current pip documentation at implementation time.
- Markdown parsing must preserve useful line numbers and reject ambiguous selections. Start with top-level fenced blocks only.
- A shared venv can be mutated by example code. Trusted examples are required; stronger per-example installation isolation can be explored later.
- Version recording is diagnostic evidence, not hermetic reproducibility. Lockfiles/wheelhouses are deferred product features.

## Engineering checks

At implementation time add lint/type checks plus a Windows/Linux regression matrix. Pin third-party CI actions to reviewed commit SHAs, use minimum permissions, and separate release credentials from PR execution. No workflow is installed during the planning phase.
