# FirstRun

**Catch the installation and quickstart failures your development environment hides.**

FirstRun is a planned Python CLI for checking README examples against an installed package, in a fresh environment outside the source checkout, on Windows and Linux.

**Status: planning. There is no runnable CLI or published package yet.** The repository name is reserved here; a package registry name has not been selected or reserved.

## The problem

A quickstart can work in your checkout while the published wheel is missing a data file, module, or runtime dependency. FirstRun will test the installed-package experience and report the failing README location and execution stage.

## Proposed workflow

1. Mark a Python README block with a stable ID.
2. Select the block in `firstrun.toml`.
3. Test a locally built wheel or a pinned public package in a clean virtual environment.
4. Receive console and machine-readable results.

Illustrative interface, not available yet:

```text
firstrun check --config firstrun.toml --artifact dist/example.whl
firstrun check --config firstrun.toml --package example==1.2.3
```

## Project plan

- [MVP specification and acceptance criteria](docs/SPEC.md)
- [Architecture and design decisions](docs/ARCHITECTURE.md)
- [Milestones and implementation backlog](docs/ROADMAP.md)
- [Problem validation and pilot guide](docs/VALIDATION.md)
- [Contributing](CONTRIBUTING.md)
- [Security and execution boundaries](SECURITY.md)

The initial audience is maintainers of pure-Python libraries with examples that need no credentials or external services. The first release will support Python 3.11+ on Windows and Linux. It will not execute shell snippets or provide a security sandbox.

## Existing tools

[pytest-examples](https://github.com/pydantic/pytest-examples), [Runme](https://docs.runme.dev/), and [rundoc](https://github.com/eclecticiq/rundoc) already address executable documentation. FirstRun's proposed focus is testing installed distributions outside a checkout with explicit example selection and cross-platform failure diagnostics. We will validate whether this deserves a separate tool before committing to a full runner.

## Contribute

During planning, the most valuable contributions are concrete packaging failures, comparisons with existing workflows, and feedback on the specification. Start with the [contribution guide](CONTRIBUTING.md) and repository issues. No adopter or contributor counts are claimed.

## License

MIT. See [LICENSE](LICENSE).
