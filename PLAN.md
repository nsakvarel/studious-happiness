# Plan

Stage 01. Source of truth for *what* we build and *why*, in priority order. Detailed tickets live in Jira / ADO Boards; link them here.
Update this file in the PR that starts or finishes an item.

## Goals
- _Describe the product outcome in one or two sentences._

## Roadmap

| # | Item | Tracker | Status | Spec |
|---|------|---------|--------|------|
| 1 | Service skeleton with health check, CI/CD to Azure | — | Done | [SPEC §1](SPEC.md#1-service-skeleton) |
| 2 | _Next feature_ | _JIRA-123_ | Planned | — |

## Out of scope
- _List what we are explicitly not doing._

## Decisions log
| Date (Tbilisi) | Decision | Why |
|---|---|---|
| 2026-10-23 | Node.js 22 + TypeScript, Azure App Service, GitHub Actions | Matches team stack; OIDC deploy avoids stored secrets |
