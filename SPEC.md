# Spec

Stage 02. Source of truth for *how* the system behaves. Each section maps to a PLAN item; acceptance criteria map one-to-one to tests.
Design artifacts (diagrams, mockups) live in Confluence / SharePoint / Azure Blob Storage — link them, don't copy them.

## 1. Service skeleton

**Endpoints**

| Method | Path | Response |
|---|---|---|
| GET | `/` | 200, HTML page with `<h1>studious-happiness</h1>` |
| GET | `/health` | 200, `{"status":"ok"}` — used by the deploy smoke test |
| * | anything else | 404, `{"error":"not_found"}` |

**Config:** `PORT` (default `3000`).

**Acceptance criteria → tests**
- Health returns ok → `tests/unit/app.test.ts`, `tests/e2e/home.spec.ts`
- Home page renders heading → `tests/e2e/home.spec.ts`
- Unknown route returns 404 → `tests/unit/app.test.ts`

## 2. _Next feature_
_Copy the structure above._
