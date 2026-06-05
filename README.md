# Copilot Satori

> Portable Agent Skills for GitHub Copilot — migrated from OpenCode format.
> Drop into any project and give Copilot superpowers: architectural governance, refactoring discipline, layer integrity, and code review standards.

## What's Inside

7 professional-grade **Agent Skills** (compatible with the [Agent Skills standard](https://agentskills.io)) that transform GitHub Copilot from a chatty autocomplete into a disciplined engineering assistant.

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

For even deeper integration, create a `.github/copilot-instructions.md` that loads the skills you use most:

```markdown
> Always load architectural-governance before designing new modules.
> Always load anti-reimplementation before writing new utilities.
> Always load efficient-coding during implementation.
> Always load senior-refactor-reviewer before committing.
```

## Repository Structure

```
copilot-satori/
├── README.md                          ← This file
├── AGENTS.md                          ← Agent definitions (Copilot auto-reads)
└── .github/
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

## What's NOT Included

These capabilities existed in the original OpenCode setup but are not portable to Copilot:

| Feature | Alternative |
|---------|-------------|
| **Graphify** (knowledge graph) | Use Copilot's built-in codebase indexing |
| **Engram** (persistent memory) | Document in `AGENTS.md` or `copilot-instructions.md` |
| **Contract manifests** (`contracts/<feature>.json`) | Track in your project management tool |
| **Orchestration flow** (fan-out/fan-in) | Manual sequential prompting |
| **Background execution** (PTY) | Use VS Code tasks (`tasks.json`) |
| **File system tools** (read/write/edit) | Use VS Code native editing |

## License

MIT — use freely in any project, personal or commercial.
