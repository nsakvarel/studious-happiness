# CLAUDE.md

This file provides guidance for AI assistants (Claude Code and similar) working in this repository.

## Repository Overview

**Repository:** nsakvarel/studious-happiness
**Stack:** Node.js 22, TypeScript (ESM), Vitest, Playwright, deployed to Azure App Service via GitHub Actions.

Before any non-trivial change read, in order:
1. `NON-NEGOTIABLES.md` — rules every change must satisfy
2. `PLAN.md` — what we are building and why
3. `SPEC.md` — required behavior and the acceptance-criteria → tests mapping

For a new feature, use the `feature-from-spec` skill. Before opening a PR, run the `reviewer` agent.

---

## Git Workflow

### Branch Naming

AI-generated branches follow the convention:

```
claude/<short-description>-<SESSION_ID>
```

Example: `claude/add-claude-documentation-CAwJT`

Human-authored branches should follow whatever convention the team establishes (e.g., `feature/`, `fix/`, `chore/`).

### Commits

- Write clear, imperative commit messages (e.g., `Add authentication module`, `Fix null pointer in user service`)
- Keep commits focused — one logical change per commit
- Do not amend published commits; create new ones instead

### Pushing

Always push with tracking:

```bash
git push -u origin <branch-name>
```

On network failure, retry with exponential backoff: 2s → 4s → 8s → 16s (max 4 retries).

### Pull Requests

- Target `main`
- PRs should include a summary of changes and a test plan
- Do not force-push to shared or main branches

---

## Development Setup

Prerequisites: Node.js 22 (`.nvmrc`).

```bash
npm ci
npm run dev      # http://localhost:3000, reloads on change
npm run build && npm start
```

One-time account setup (secrets, Azure, branch protection): `docs/SETUP.md`.

---

## Project Structure

```
studious-happiness/
├── CLAUDE.md, NON-NEGOTIABLES.md, PLAN.md, SPEC.md   # repository context
├── src/app.ts          # request handler (unit-tested)
├── src/server.ts       # HTTP entry point
├── tests/unit/         # Vitest
├── tests/e2e/          # Playwright
├── .claude/skills/     # Claude skills (feature-from-spec)
├── .claude/agents/     # Claude subagents (reviewer, test-writer)
├── .github/workflows/  # ci, codeql, dependency-review, secret-scan, claude-review, deploy
└── docs/SETUP.md       # manual setup steps
```

---

## Testing

- Unit tests + coverage gate (80%): `npm run test:coverage`
- Single unit test: `npx vitest run tests/unit/app.test.ts -t "reports healthy"`
- E2E: `npm run test:e2e` (first time: `npx playwright install chromium`)
- Every SPEC.md acceptance criterion maps to at least one test
- Tests must pass before merging to `main`

### Guidelines

- Write tests alongside new features
- Avoid testing implementation details; prefer testing behavior
- Keep tests deterministic and side-effect free

---

## Code Style and Linting

- Lint: `npm run lint` (ESLint + typescript-eslint)
- Typecheck: `npm run typecheck` (strict mode)
- Relative imports use the `.ts` extension (rewritten to `.js` on build)
- Do not suppress lint warnings without a documented reason

---

## Key Conventions for AI Assistants

### General

- Read files before editing them
- Prefer editing existing files over creating new ones
- Do not add unnecessary abstractions, docstrings, or refactors beyond the scope of the task
- Only add error handling for real, reachable failure scenarios
- Do not commit unless explicitly asked

### Security

- Never introduce SQL injection, XSS, command injection, or other OWASP Top 10 vulnerabilities
- Validate input only at system boundaries (user input, external APIs)
- Do not commit secrets, credentials, or `.env` files

### Scoping

- Make only the changes that are directly requested
- If a task is ambiguous, ask before proceeding
- Confirm before taking destructive or irreversible actions (deleting files, force-pushing, dropping data)

### Tool Use

- Use dedicated tools (Read, Edit, Write, Grep, Glob) instead of raw shell equivalents
- Parallelize independent tool calls when possible
- Track multi-step work with the TodoWrite tool

---

## Updating This File

Whenever significant changes are made to the project — new framework adopted, new testing strategy, new CI pipeline, new conventions — update the relevant sections in this file. Keeping CLAUDE.md current ensures that AI assistants always have accurate context.
