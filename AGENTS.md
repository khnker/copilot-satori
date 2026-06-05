# Copilot Satori — Agent Definitions

This file defines custom agents for GitHub Copilot (VS Code).
Place this in your project root. Copilot reads it automatically.

## Available Agents

### @planner
Analyzes requirements, decomposes into tasks, produces a cross-layer plan.
- Loads: `architectural-governance`, `anti-reimplementation`
- Convention: writes plan to `.copilot/contracts/<feature>.json`
- Output: task breakdown with layer assignments (DB → API → FE)

### @backend-exec
Implements backend changes: NestJS/TypeORM APIs, database migrations, services.
- Loads: `backend-integrity`
- Source of truth: database is authoritative for data, backend for business logic
- Verification: runs tests after implementation

### @frontend-exec
Implements frontend changes: Angular, React, or other SPA frameworks.
- Loads: `frontend-boundary-protection`
- Contract: consumes API via service layer, never duplicates backend logic
- Verification: runs tests after implementation

### @debugger
Diagnoses and fixes bugs, failing tests, regressions.
- Loads: `senior-refactor-reviewer`, `efficient-coding`
- Approach: reproduce → isolate → fix minimum → verify with test
- Escalation: after 2+ consecutive failures, reports to user

### @arch-review
Post-change quality gate — validates cross-layer contract alignment.
- Loads: `architectural-governance`, `repository-semantic-map`
- Scope: verifies FE ↔ API ↔ DB contract alignment
- Output: approved or rejected with reasons

## How It Works

1. User requests a feature
2. `@planner` decomposes the work
3. Backend/frontend agents implement per plan
4. `@arch-review` validates the result
5. `@debugger` fixes any issues found

## Loading Skills

Skills are loaded from `.github/skills/<name>/SKILL.md`.
Copilot automatically discovers skills in this path.
