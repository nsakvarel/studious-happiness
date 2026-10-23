# studious-happiness

Node.js 22 + TypeScript service with an AI-assisted delivery pipeline:
Plan → Design → Develop → Review → Test → Secure → Deploy → Maintain.

| Stage | In this repo |
|---|---|
| 01 Plan | [`PLAN.md`](PLAN.md), feature issue template; Jira / ADO Boards linked via apps |
| 02 Design | [`SPEC.md`](SPEC.md); Confluence / SharePoint / Blob docs linked from it |
| 03 Develop | [`CLAUDE.md`](CLAUDE.md), `.github/copilot-instructions.md`, `.claude/skills`, `.claude/agents` |
| 04 Review | `claude-review.yml` (Claude PR review + `@claude`), CODEOWNERS, PR template |
| 05 Test | `ci.yml`: ESLint, tsc, Vitest with 80% coverage gate, Playwright |
| 06 Secure | `codeql.yml`, `dependency-review.yml`, `secret-scan.yml` (gitleaks) |
| 07 Deploy | `deploy.yml`: Azure App Service via OIDC, plus a smoke test |
| 08 Maintain | `dependabot.yml` |
| Principles | [`NON-NEGOTIABLES.md`](NON-NEGOTIABLES.md) |

```bash
npm ci
npm run dev            # http://localhost:3000
npm run lint && npm run typecheck && npm run test:coverage && npm run test:e2e
```

One-time account setup (secrets, Azure, branch protection): [`docs/SETUP.md`](docs/SETUP.md).
