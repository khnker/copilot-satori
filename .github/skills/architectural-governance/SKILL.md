# Skill: architectural-governance

# Core Directive

Your primary goal is NOT to generate code quickly.
Your goal is to preserve cumulative architectural coherence.

Assume that:
- the repository contains implicit constraints
- undocumented historical decisions exist
- logic duplication has exponential future cost

# Mandatory Pre-Implementation Audit

BEFORE creating: classes, hooks, DTOs, services, repositories, adapters, schemas, components, endpoints, utilities, validators.

YOU MUST:
1. Search for similar implementations
2. Identify existing source of truth
3. Detect dominant naming conventions
4. Review upstream and downstream contracts
5. Identify semantic ownership

# Decision Table: Should You Create a New Abstraction?

| Condition | YES — create | NO — alternative |
|-----------|-------------|------------------|
| Same pattern appears 3+ times | Extract shared abstraction | Use a copy with comment if only 2 occurrences |
| Need a wrapper to "protect" consumption | NO — prefer minimal interface | Use the type/subtype directly |
| Want to make it "extensible for the future" | NO — YAGNI | Implement only what's needed today |
| Code is in the wrong layer | Move ownership, don't create a wrapper | Local refactor, not a new layer |
| DTO repeats entity fields | Use mapped types / partial | Do NOT recreate from scratch |

# Hard Constraints

DO NOT:
- create premature abstractions
- create files if extending the existing one is sufficient
- duplicate validations
- introduce new patterns without systemic need
- move cross-layer logic without justifying causality
- create unnecessary wrappers
- generate helper explosion

# Anti-Patterns and Fixes

| Anti-pattern | Signal | Fix |
|-------------|--------|-----|
| **Helper Explosion** | `utils/`, `helpers/`, `common/` with dozens of unrelated files | Place each function in the module that uses it. If 3+ modules need it, extract to shared with a semantic name |
| **Fat Interface** | Interface with 10+ methods where most are `Optional` or throw `NotImplemented` | Split into small role-specific interfaces (ISP) |
| **Mock Layer** | An abstraction layer that only delegates 1:1 to another (e.g., ServiceWrapper → Service) | Remove the intermediate layer. Use directly |
| **Copy-Paste Inheritance** | Class B extends A but overrides 80% of methods | Prefer composition. Extract a common interface |
| **God Module** | Module with 2000+ lines that "does everything" | Split by responsibility. Minimum 1 reason to change |

# Refactor Heuristic

Prefer: local extension over new global abstraction.
Prefer: composition over inheritance.
Prefer: explicitness over cleverness.

# Architecture Preservation

Before modifying multiple layers:
- identify bounded contexts
- map downstream impact
- detect hidden coupling
- verify side-effects

# Production Checklist — Pre-Commit

- [ ] Does the new abstraction reduce total system complexity?
- [ ] Does something similar already exist that could be extended?
- [ ] Does the change respect existing bounded contexts?
- [ ] Are there side-effects in unrelated modules?
- [ ] Is naming consistent with the rest of the project?
- [ ] Is any directional dependency violated (FE → API ← DB)?
- [ ] Is the change testable without excessive mocking?

# Anti-Entropy Rule

Each new line must justify:
- reduction of complexity, or
- clear increase in capability

Otherwise: DO NOT implement it.

# References

- **Clean Architecture** (Robert C. Martin, 2017) — separated layered systems
- **Building Evolutionary Architectures** (Ford, Parsons, Kua, 2017) — fitness functions, incremental change
- **A Philosophy of Software Design** (John Ousterhout, 2018) — deep modules vs shallow modules
