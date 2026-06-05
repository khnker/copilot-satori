---
name: repository-semantic-map
description: Maintains a persistent mental model of the system; detects architectural drift, tracks decisions, and onboards you faster
type: agent
---

# Skill: repository-semantic-map

# Core Directive

Maintain a persistent mental model of the system and detect architectural drift.
Every session starts by loading the semantic map. Every change validates against it.

# Semantic Map Structure

```
Repository: <name>
Language: <primary language>
Framework: <framework + version>
Architecture: <pattern — e.g., layered, modular monolith, microservices>
Database: <DBMS + ORM>

/// DOMAINS ///
<domain> → <modules/packages> — <one-line purpose>

/// LAYERS ///
<layer> → <directory> — <responsibility>

/// KEY CONTRACTS ///
<contract name> → <provider> — <consumer(s)>

/// GOD MODULES ///
<module path> — <why it's central, risk level>

/// DECISIONS ///
<ADR #> — <title> — <status>
```

# Initial Load

When starting work on a new (to you) repository:

1. Scan the top-level directory structure
2. Read `package.json` / config files to understand the stack
3. Identify the main entry point and routing
4. Map the data flow: request → controller → service → repository → database
5. Identify test patterns and locations
6. Create the semantic map in `AGENTS.md` or `copilot-instructions.md`

# Drift Detection

While working, watch for:

- New files in unexpected locations
- Imports crossing layer boundaries (FE importing DB logic, API importing UI code)
- Duplication of existing functionality
- Patterns that differ from the established conventions
- Missing tests for new code

When drift is detected:
1. Flag it to the user
2. Suggest the correction
3. Update the semantic map

# Session Protocol

## Start
- Load or create the semantic map
- Review recent changes (git log since last session)
- Identify what part of the map this session will touch

## During Work
- Validate each change against the map
- Update the map if the architecture is evolving

## End
- Record any structural changes made
- Flag any outstanding drift or concerns
- Persist the updated map in `DECISIONS.md` or `docs/architecture/`

# Decision Tracking

Record every architectural decision with:

```
Status: [proposed | accepted | deprecated | superseded]
Context: <what prompted this decision>
Decision: <what was decided>
Consequences: <trade-offs, follow-up work>
```

Keep decisions in a `DECISIONS.md` or `ADRs/` directory.
See `DECISIONS.md` (included in this repo) for a template.
