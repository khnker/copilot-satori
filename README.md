# Copilot Satori

> Portable Agent Skills for GitHub Copilot — migrated from OpenCode format.
> Drop into any project and give Copilot superpowers: architectural governance, refactoring discipline, layer integrity, and code review standards.

## What's Inside

7 professional-grade **Agent Skills** (compatible with the [Agent Skills standard](https://agentskills.io)) plus a **`copilot-instructions.md`** with comprehensive best practices — all designed to transform GitHub Copilot from a chatty autocomplete into a disciplined engineering assistant.

| Skill | Purpose |
|-------|---------|
| 🏗️ **architectural-governance** | Preserves cumulative architectural coherence. Prevents premature abstractions, helper explosion, and layer violations. |
| ♻️ **anti-reimplementation** | Prevents redundant code. Always searches for existing implementations before writing new ones. Reuse ladder: call → extend → compose → move → copy → write. |
| ⚡ **efficient-coding** | Eliminates cognitive and structural waste. Small focused edits, no dead ends, names that reveal intent, no premature abstraction. |
| 🛡️ **backend-integrity** | Enforces backend as source of truth. Transactional integrity, security boundaries, API contract stability, proper layering. |
| 🧩 **frontend-boundary-protection** | Prevents logic contamination in the frontend. Business logic belongs in the API — FE is presentation only. |
| 🔍 **senior-refactor-reviewer** | Acts as a senior code reviewer. Checks readability, testability, changeability, complexity budget, and coupling before every merge. |
| 🗺️ **repository-semantic-map** | Maintains a persistent mental model of the system. Detects architectural drift, tracks decisions, and onboards you faster. |

Also includes **5 agent definitions** (`AGENTS.md`): planner, backend-exec, frontend-exec, debugger, arch-review — with skill-loading and workflow instructions.
Plus a **`.github/copilot-instructions.md`** with 8 sections covering coding style, testing, git, security, error handling, architecture principles, and AI interaction guidelines — automatically read by Copilot on every query.

## How to Install

### One-time setup: Personal skills (available in all projects)

> Skills you install once and use everywhere.

```bash
# Create the personal skills directory
mkdir -p ~/.copilot/skills

# Symlink each skill (or copy — symlinks update when you pull updates)
for skill in architectural-governance anti-reimplementation efficient-coding \
             backend-integrity frontend-boundary-protection \
             senior-refactor-reviewer repository-semantic-map; do
  ln -sf "$(pwd)/.github/skills/$skill" ~/.copilot/skills/
done
```

### Per-project setup: Project skills (isolated to one repo)

> Skills that travel with the codebase — ideal for team consistency.

```bash
# Just copy (or symlink) this entire repo into your project
cp -r .github/skills /path/to/your/project/.github/skills
cp AGENTS.md /path/to/your/project/AGENTS.md
```

Now Copilot will automatically discover and load the skills when you open the project in VS Code.

### Verify installation

Open Copilot Chat in VS Code and type:

```
@architectural-governance What is your core directive?
```

It should respond with: *"Your primary goal is NOT to generate code quickly. Your goal is to preserve cumulative architectural coherence."*

## How to Use

### Using Skills Directly (Copilot Chat)

In VS Code, mention a skill by name:

```
@architectural-governance Review this controller for me
@anti-reimplementation Is there existing code for export?
@backend-integrity Are these service methods transactional?
@frontend-boundary-protection Is this component violating FE boundaries?
@efficient-coding Simplify this function
@senior-refactor-reviewer Review this PR diff
@repository-semantic-map Load the semantic map for this repo
```

### Using Agents (Copilot Edits / Chat)

The `AGENTS.md` defines multi-step agents:

```
@planner I need a user profile feature with avatar upload
@backend-exec Implement the user profile API from the plan
@frontend-exec Build the profile edit form
@arch-review Verify the feature end-to-end
@debugger The tests are failing, help
```

### Advanced: copilot-instructions.md

The included `.github/copilot-instructions.md` is automatically read by GitHub Copilot on every query.
It contains 8 sections covering:

1. **Always-On Skills** — which skills to load before writing code, during implementation, and before committing
2. **Code Style & Conventions** — naming, structure, TypeScript/React/Python rules
3. **Git & Commits** — one concern per commit, imperative mood, small diffs
4. **Error Handling** — never swallow errors, typed exceptions, user-friendly messages
5. **Security** — authentication by default, server-side validation, no secrets in code
6. **Architecture Principles** — data flow direction, bounded contexts, composition over inheritance
7. **AI Interaction Guidelines** — when to ask questions, how to propose trade-offs, when to plan
8. **Project Recognition** — auto-detection patterns for skills, agents, monorepo structure

You can customize it per-project — Copilot reads it automatically.

## Repository Structure

```
copilot-satori/
├── README.md                          ← This file
├── AGENTS.md                          ← Agent definitions (Copilot auto-reads)
└── .github/
    ├── copilot-instructions.md         ← Best practices (Copilot auto-reads)
    └── skills/
        ├── architectural-governance/
        │   └── SKILL.md               ← Preserves architectural coherence
        ├── anti-reimplementation/
        │   └── SKILL.md               ← Prevents redundant code
        ├── efficient-coding/
        │   └── SKILL.md               ← Eliminates cognitive waste
        ├── backend-integrity/
        │   └── SKILL.md               ← Backend as source of truth
        ├── frontend-boundary-protection/
        │   └── SKILL.md               ← FE/BE layer separation
        ├── senior-refactor-reviewer/
        │   └── SKILL.md               ← Obsessive maintainability review
        └── repository-semantic-map/
            └── SKILL.md               ← Persistent system model
```

## Compatibility

- ✅ **GitHub Copilot** (VS Code) — full support (Agent Skills standard)
- ✅ **GitHub Copilot** (JetBrains, Neovim) — should work via same format
- ✅ **OpenCode** — native support (same `agentskills.io` format)
- ✅ **Cline / Roo Code** — compatible via Agent Skills standard
- ✅ **Any agent tool** — the SKILL.md format is a documented standard

## License

MIT — use freely in any project, personal or commercial.
