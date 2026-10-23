---
name: feature-from-spec
description: Implement a feature end to end through the Plan → Spec → Develop → Test stages. Use when asked to build a new feature or pick up a PLAN.md item.
---

1. Read `NON-NEGOTIABLES.md`, `PLAN.md` and `SPEC.md`.
2. If the feature has no `PLAN.md` row, add one. If it has no `SPEC.md` section, draft one (endpoints/behavior, config, acceptance criteria) and stop to confirm it with the user before writing code.
3. Write failing tests first: a unit test per acceptance criterion in `tests/unit/`, and a Playwright test in `tests/e2e/` for anything user-visible.
4. Implement in `src/` until tests pass.
5. Run `npm run lint && npm run typecheck && npm run test:coverage && npm run test:e2e`. Fix everything; never lower the coverage threshold.
6. Update the `PLAN.md` status and the `SPEC.md` acceptance-criteria → tests mapping.
7. Commit on a feature branch and summarize for a PR using `.github/pull_request_template.md`.
