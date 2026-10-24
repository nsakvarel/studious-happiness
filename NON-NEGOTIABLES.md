# Non-negotiables

Guiding principles every change must satisfy — human or AI. Reviewers (including the Claude review workflow) block PRs that violate them.

1. **Main is always deployable.** Every merge to `main` deploys to production. Nothing merges with red CI.
2. **No secrets in the repo.** Credentials live in GitHub Secrets / Azure Key Vault. Deploy auth is OIDC only — no publish profiles or client secrets.
3. **Tests ship with the code.** New behavior needs a unit test; user-visible behavior needs a Playwright test. Coverage stays ≥ 80%.
4. **Security gates are not bypassed.** CodeQL, dependency review and secret scanning findings are fixed or explicitly dismissed with a reason, never disabled.
5. **Spec before code.** Non-trivial work starts with a `PLAN.md` entry and a `SPEC.md` section; the PR links both.
6. **Validate at the boundary.** All external input (HTTP, env, third-party APIs) is validated where it enters the system.
7. **Small, reviewable PRs.** One logical change per PR; aim for < 400 changed lines.
8. **Humans own merges.** AI may author and review; a human approves every merge to `main`.
