# Roadmap and backlog

All work is planned, not complete. Estimates assume one maintainer spending 6–10 hours per week. Milestones have completion gates rather than calendar promises.

## M0 — Validate the problem (estimated 1–2 weeks)

- Review five maintainer workflows, including existing documentation runners and manual CI.
- Capture two reproducible installed-package failures with redistribution permission or original minimal reproductions.
- Obtain three willing pilot maintainers. Do not claim these conversations have happened.
- Record a proceed/change/stop decision in an issue. If existing tools solve the need with a short configuration, prefer contributing upstream.

A minimal throwaway fixture demonstration is allowed during validation; full product implementation follows the decision.

## M1 — First installed-wheel check (estimated 2–3 weeks after M0)

- Confirm manifest, markers, exit codes, and result schema.
- Implement strict parsing and source locations.
- Create an isolated environment and install one local wheel.
- Run one selected example outside the checkout; demonstrate good-wheel pass and missing-resource failure.
- Publish local development instructions that actually work.

Gate: vertical slice passes on Windows and Linux using local fixtures.

## M2 — Reliable v0.1 (estimated 2–4 additional weeks)

- Implement bounded output, process-tree cancellation, and cleanup.
- Implement console/JSON reporting and exact stdout assertions.
- Add pinned public-package mode, environment allowlisting, and input validation.
- Complete all SPEC.md acceptance checks, packaging checks, and CI matrix.
- Select an available registry name and review the release procedure.

Gate: acceptance evidence exists for both OSes; publish only after reviewing the artifact and installation instructions. Process handling may extend this estimate.

## M3 — Pilot adoption (estimated 4–6 additional weeks)

- Integrate with three consenting pilot projects.
- Capture actual defects found, setup friction, and retention after a month.
- Target five independent adopters as a product goal, not a release requirement.
- Write migration recipes from observed workflows, triage issues, and invite repeat contributors into review.

Gate: at least three pilots choose to retain the check. Otherwise revisit scope and differentiation.

## Initial issue sequence

1. Validate demand and compare existing workflows — M0, no dependencies.
2. Reproduce good-wheel and missing-resource behavior — M0, independent demonstration.
3. Agree v0.1 manifest and result contracts — M1, after M0 decision.
4. Implement strict manifest and Markdown selection — M1, after issue 3.
5. Implement isolated wheel execution vertical slice — M1, after issues 2–4.
6. Bound execution, clean up processes, and filter environment — M2, after issue 5.
7. Add reports, registry mode, and cross-platform release checks — M2, after issues 5–6.
8. Run pilot integrations and decide release scope — M3, after M2 acceptance.

Do not assign all tasks to newcomers. Maintainers own core contracts and difficult process isolation work. Label a bounded issue `good first issue` only after the development environment and reproduction instructions exist.

## Deferred

macOS support, package extras, multi-block sessions, shell snippets, pytest adapters, caching, hermetic wheelhouses, and automatic documentation updates require demonstrated demand. Avoid expanding the scope to manufacture contribution opportunities.
