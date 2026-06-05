---
name: anti-reimplementation
description: Prevents redundant code; always searches existing implementations before writing new code
type: agent
---

# Skill: anti-reimplementation

# Core Directive

Prevent redundant recreation of existing logic and components.
Always assume the functionality already exists somewhere — find it before building it.

# Mandatory Pre-Flight Check

Before writing ANY new code, ask:

1. **Does this exact functionality already exist?**
   - Search for keywords, patterns, and domain terms
   - Check similar file names across the project
   - Look for utility/helper modules

2. **Does similar functionality exist that could be extended?**
   - Can I add a parameter instead of a new function?
   - Can I inherit or compose instead of duplicating?
   - Can I generalize an existing implementation?

3. **Has this been implemented in another layer or module?**
   - The API might already have the logic, the FE just needs to call it
   - The DB might already store the data, just needs a new query

# Reuse Ladder (prefer top to bottom)

1. **Call existing** — invoke what's already there (API endpoint, service method)
2. **Extend existing** — add parameter, optional behavior, new case
3. **Compose existing** — combine 2+ existing pieces to solve the problem
4. **Move/refactor existing** — relocate existing logic, do not rewrite
5. **Copy with attribution** — last resort, leave a comment pointing to source
6. **Write from scratch** — ONLY when nothing else is viable

# Red Flags

- Writing a new **service** when a similar one exists → extend or compose
- Writing a new **component** when a similar one exists → parameterize
- Writing a new **DTO** that mirrors an entity → use mapped types
- Writing a new **utility function** → check existing utilities first
- Writing a new **type/interface** → check if it already exists in the project
- Duplicating **validation logic** → validators belong in one place

# DRY Enforcement

- Same pattern 2 times → normalize (note for future)
- Same pattern 3 times → abstract into shared module
- Same string literal 3+ times → extract to constant
- Same type signature 2+ times → extract to shared type
- Same validation rule 2+ times → extract to shared validator
