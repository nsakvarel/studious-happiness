# CLAUDE.md

This file provides guidance for AI assistants (Claude Code and similar) working in this repository.

## Repository Overview

**Repository:** nsakvarel/studious-happiness
**Status:** Newly initialized — no source code committed yet.

This repository was created with the intention of hosting a project. As development begins, this file should be updated to reflect the actual codebase structure, conventions, and workflows.

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

- Target the default branch (update this once the default branch is established)
- PRs should include a summary of changes and a test plan
- Do not force-push to shared or main branches

---

## Development Setup

> This section should be updated once the project stack is decided.

### Prerequisites

Document required tools here (e.g., Node.js version, Python version, Docker, etc.)

### Installation

```bash
# Example — replace with actual setup commands
git clone <repo-url>
cd studious-happiness
# install dependencies
```

### Running Locally

```bash
# Add commands to start the development server or run the application
```

---

## Project Structure

> Update this section as the project takes shape.

```
studious-happiness/
├── CLAUDE.md          # This file — AI assistant guidance
├── README.md          # (to be created) Human-facing project documentation
└── .git/              # Git internals
```

---

## Testing

> Update this section once a test framework is chosen.

- Run all tests: `# add command here`
- Run a single test: `# add command here`
- Tests must pass before merging to the main branch

### Guidelines

- Write tests alongside new features
- Avoid testing implementation details; prefer testing behavior
- Keep tests deterministic and side-effect free

---

## Code Style and Linting

> Update this section once a language and linter are chosen.

- Follow the linting configuration committed to the repository
- Run the linter before committing: `# add command here`
- Run the formatter before committing: `# add command here`
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
