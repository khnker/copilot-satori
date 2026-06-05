# Copilot Satori — Best Practices

This file configures GitHub Copilot's behavior across all projects using this setup.
Copilot reads `.github/copilot-instructions.md` automatically on every query.

## 1. Always-On Skills

These skills define how Copilot should approach all coding work.
When the user mentions a skill name (@skill-name), load its SKILL.md file and follow its instructions strictly.

### Before Writing Code
- @anti-reimplementation — Search the codebase first. If the logic exists, reuse or extend it. Never write from scratch when you can call, compose, or extend.
- @efficient-coding — Solve exactly the task, nothing more. Small focused edits. No dead ends (no TODOs without action, no console.log, no commented code).

### During Implementation
- @architectural-governance — Preserve cumulative architectural coherence. No premature abstractions, no helper explosion, no layer violations. Every new line must reduce complexity or increase clear capability.
- @backend-integrity — Backend is the source of truth for data and business logic. All validation, authorization, and transactional integrity is enforced server-side. Controllers route, services orchestrate, repositories access data.
- @frontend-boundary-protection — Frontend is a presentation layer. Business logic belongs in the API. Never duplicate backend validation, never store sensitive data insecurely, never hardcode API values.

### Before Committing
- @senior-refactor-reviewer — Review every change for readability, testability, changeability, and coupling. If a refactor touches more than 5 files, pause and reconsider.

### When Onboarding or Drifting
- @repository-semantic-map — Load or maintain the system's mental model. Track architectural decisions, detect drift, and keep the map updated.

## 2. Code Style & Conventions

### Naming
- Use intention-revealing names: `getUserById()` not `fetchData()`, `isActive()` not `checkStatus()`
- Boolean variables: prefix with `is`, `has`, `should`, `can`
- Functions: verb + noun (`createOrder()`, `validateInput()`)
- Classes: singular nouns (`UserService`, `OrderController`)
- Constants: `UPPER_SNAKE_CASE`
- Files: `kebab-case.ts` (Angular/JS ecosystem) or `snake_case.py`

### Structure
- One responsibility per file (max 300 lines)
- One responsibility per function (max 30 lines)
- Max 3 levels of nesting — extract early, nest less
- Group imports: third-party → project modules → relative imports (blank line between groups)

### TypeScript (Angular / NestJS)
- Use strict TypeScript. No `any`. Prefer `unknown` when type is truly unknown.
- Interfaces for contracts, types for unions/utilities
- `readonly` for immutable properties
- Use optional chaining `?.` and nullish coalescing `??` — not `&&` chains
- DTOs for API boundaries, entities for database models — never expose entities directly

### Testing
- Write tests for every new function
- Write regression tests for every bug fix
- Test behavior, not implementation
- Mock external boundaries (API, database), not internal collaborators
- Use descriptive test names: `should return 404 when user does not exist`

## 3. Git & Commits

- One concern per commit (features, refactors, and fixes never mix)
- Imperative mood: `"Add payment method validation"` not `"Added..."`
- Reference issues when applicable: `"Fix login race condition (closes #42)"`
- Keep commits small and reviewable (< 300 lines diff preferred)

## 4. Error Handling

- Never silently swallow errors — empty catch blocks are forbidden
- Throw typed exceptions with meaningful messages
- Log errors with context (request ID, user, action)
- Return user-friendly error messages from APIs (never leak stack traces)
- In frontend, handle loading, empty, error, and success states for every async operation

## 5. Security

- All endpoints authenticated by default — opt-in for public routes
- Validate ALL input server-side (client validation is UX-only)
- Use parameterized queries or ORM — never string-concatenate SQL
- No secrets in code. Use environment variables or secrets manager.
- Sanitize output for XSS prevention in rendered content

## 6. Architecture Principles

```
Client → Controller (validation) → Service (logic) → Repository (data)
```

- Data flows down. Dependencies point inward (Dependency Inversion Principle)
- Bounded contexts are isolated — no cross-domain service calls without explicit orchestration
- Every cross-layer change must be intentional and documented
- Prefer composition over inheritance
- Prefer explicitness over cleverness

## 7. AI Interaction Guidelines

- When uncertain about intent, ask clarifying questions before generating code
- When suggesting alternatives, explain the trade-off (complexity vs capability)
- When the task exceeds 30 minutes of work, propose a plan before writing code
- Prefer framework generators for boilerplate (`ng generate`, `nest generate`), but custom code for business logic — use judgment
- Never suggest a dependency that duplicates existing functionality

## 8. Project Recognition

This project structure is recognized by these patterns:

| Pattern | Behavior |
|---------|----------|
| `.github/skills/` present | When the topic matches a skill name, suggest the relevant @skill to the user |
| `AGENTS.md` present | Agent definitions available for Copilot Edits workflows |
| `.github/copilot-instructions.md` | These instructions are active |
| Monorepo with `packages/` | Scope suggestions to the package being edited |
