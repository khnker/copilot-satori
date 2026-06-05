<div align="center">
  <img src="logo.svg" alt="Copilot Satori" width="180" height="180">
  <h1>Copilot Satori</h1>
</div>

> Portable Agent Skills for GitHub Copilot — drop into any project and give Copilot superpowers.
> Architectural governance, refactoring discipline, layer integrity, and code review standards — all in a copy-pasteable format.

## What's Inside

7 professional-grade **Agent Skills** plus a **`copilot-instructions.md`** with comprehensive best practices — all designed to transform GitHub Copilot from a chatty autocomplete into a disciplined engineering assistant.

| Skill | Purpose |
|-------|---------|
| 🏗️ **architectural-governance** | Preserves cumulative architectural coherence. Prevents premature abstractions, helper explosion, and layer violations. |
| ♻️ **anti-reimplementation** | Prevents redundant code. Always searches for existing implementations before writing new ones. Reuse ladder: call → extend → compose → move → copy → write. |
| ⚡ **efficient-coding** | Eliminates cognitive and structural waste. Small focused edits, no dead ends, names that reveal intent, no premature abstraction. |
| 🛡️ **backend-integrity** | Enforces backend as source of truth. Transactional integrity, security boundaries, API contract stability, proper layering. |
| 🧩 **frontend-boundary-protection** | Prevents logic contamination in the frontend. Business logic belongs in the API — FE is presentation only. |
| 🔍 **senior-refactor-reviewer** | Acts as a senior code reviewer. Checks readability, testability, changeability, complexity budget, and coupling before every merge. |
| 🗺️ **repository-semantic-map** | Maintains a persistent mental model of the system. Detects architectural drift, tracks decisions, and onboards you faster. |

Also includes:
- **5 custom agents** (`prompts/*.agent.md`): planner, backend, frontend, debugger, arch-review — with YAML frontmatter and tool permissions. Appear in the Copilot Chat agent selector.
- **`.github/copilot-instructions.md`**: 8 sections covering coding style, testing, git, security, error handling, architecture principles, and AI interaction guidelines — automatically read by Copilot on every query.
- **`.vscode/settings.json`**: VS Code configuration enabling Copilot agents and instruction files.
- **`docs/DECISIONS.md`**: Architecture Decision Record template for tracking design decisions.
- **CI workflow**: Validates skill formatting on every PR and push.

---

## How Skills and Agents Work in VS Code

### 🧩 Skills — `SKILL.md`

Skills are **instruction files** that Copilot loads **when you mention them explicitly**. They do not activate automatically.

**Where they live:**
- **Personal** (all projects): `~/.copilot/skills/<name>/SKILL.md`
- **Per-project**: `.github/skills/<name>/SKILL.md`

**How Copilot discovers them:**
VS Code searches for `SKILL.md` in the paths configured in your `settings.json`:

```json
{
  "github.copilot.chat.skills.instructionFiles": [
    "~/.copilot/skills/*/SKILL.md"
  ]
}
```

**How to invoke a skill:**
In Copilot Chat, reference the skill by name:

```
@architectural-governance Review this controller for me
```

> Skills are **not chat agents**. They are instruction files that Copilot reads as additional context when you mention them.

### 🤖 Custom Agents — `.agent.md`

Agents are **full chat modes** that appear in the **agent selector dropdown** in Copilot Chat. VS Code discovers them automatically from your prompts directory.

**Where they must live:**

| OS | Path |
|----|------|
| **macOS** | `~/Library/Application Support/Code/User/prompts/` |
| **Linux** | `~/.config/Code/User/prompts/` |
| **Windows** | `%APPDATA%\Code\User\prompts\` |

**Required format (YAML frontmatter is mandatory):**

```yaml
---
name: Satori Planner
description: Visible in the agent selector dropdown
tools:
  - read
  - search
  - edit
---
Your agent instructions here...
```

**How to invoke an agent:**
In Copilot Chat, click the agent selector dropdown (where it says "Ask" or the current agent name) → your agents appear in the list.

### 🔗 How Skills and Agents Connect

```
User selects agent from dropdown (.agent.md)
        ↓
Agent instructions say "load @skill-name"
        ↓
Copilot reads the SKILL.md as context
        ↓
Agent responds with combined behavior
```

---

## How to Install

### Step 1: Install Skills

Skills can be personal (available in all projects) or per-project.

**Personal skills (recommended):**

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

**Per-project skills:**

```bash
# Copy the skills directory into your project
cp -r .github/skills /path/to/your/project/.github/skills
```

### Step 2: Install Custom Agents

Copy the `.agent.md` files to your VS Code prompts directory:

**macOS:**
```bash
cp prompts/*.agent.md ~/Library/Application\ Support/Code/User/prompts/
```

**Linux:**
```bash
cp prompts/*.agent.md ~/.config/Code/User/prompts/
```

**Windows (PowerShell):**
```powershell
copy-item prompts/*.agent.md $env:APPDATA\Code\User\prompts\
```

### Step 3: Configure VS Code

Add to your VS Code `settings.json` (`Cmd+Shift+P` → `Open User Settings (JSON)`):

```json
{
  "github.copilot.chat.agents.enabled": true,
  "github.copilot.chat.skills.instructionFiles": [
    "~/.copilot/skills/*/SKILL.md"
  ],
  "github.copilot.chat.codeGeneration.instructions": [
    { "file": ".github/copilot-instructions.md" }
  ]
}
```

### Step 4: Verify Installation

Open Copilot Chat in VS Code and test a skill:

```
@architectural-governance What is your core directive?
```

It should respond with: *"Your primary goal is NOT to generate code quickly. Your goal is to preserve cumulative architectural coherence."*

Then check the agent selector dropdown — you should see "Satori Planner", "Satori Backend", "Satori Frontend", "Satori Debugger", and "Satori Arch Review".

---

## How to Use

### Using Skills Directly

```
@architectural-governance Review this controller for me
@anti-reimplementation Is there existing code for export?
@backend-integrity Are these service methods transactional?
@frontend-boundary-protection Is this component violating FE boundaries?
@efficient-coding Simplify this function
@senior-refactor-reviewer Review this PR diff
@repository-semantic-map Load the semantic map for this repo
```

### Using Custom Agents

Select the agent from the dropdown in Copilot Chat, then describe your task:

| Agent | When to use |
|-------|-------------|
| **Satori Planner** | Starting a new feature — decomposes work into layers (DB → API → FE) |
| **Satori Backend** | Implementing API endpoints, services, database migrations |
| **Satori Frontend** | Building UI components, pages, API integration |
| **Satori Debugger** | Fixing bugs, investigating test failures |
| **Satori Arch Review** | Validating cross-layer alignment before committing |

### Using AGENTS.md (Project-level)

The `AGENTS.md` file in the project root is also recognized by Copilot and provides agent definitions that stay with the codebase. This is useful for team consistency without each member needing to install `.agent.md` files manually.

### Advanced: copilot-instructions.md

The included `.github/copilot-instructions.md` is automatically read by GitHub Copilot on every query.
It contains 8 sections covering:

1. **Always-On Skills** — which skills to invoke before writing code, during implementation, and before committing
2. **Code Style & Conventions** — naming, structure, TypeScript/React/Python rules
3. **Git & Commits** — one concern per commit, imperative mood, small diffs
4. **Error Handling** — never swallow errors, typed exceptions, user-friendly messages
5. **Security** — authentication by default, server-side validation, no secrets in code
6. **Architecture Principles** — data flow direction, bounded contexts, composition over inheritance
7. **AI Interaction Guidelines** — when to ask questions, how to propose trade-offs, when to plan
8. **Project Recognition** — auto-detection patterns for skills, agents, monorepo structure

You can customize it per-project — Copilot reads it automatically.

---

## Repository Structure

```
copilot-satori/
├── README.md                          ← This file
├── AGENTS.md                          ← Project-level agent definitions
├── logo.svg                           ← Project logo
├── prompts/                           ← .agent.md files (copy to VS Code prompts dir)
│   ├── satori-planner.agent.md
│   ├── satori-backend.agent.md
│   ├── satori-frontend.agent.md
│   ├── satori-debugger.agent.md
│   └── satori-arch-review.agent.md
├── .vscode/
│   └── settings.json                  ← VS Code Copilot configuration
├── .github/
│   ├── copilot-instructions.md         ← Best practices (Copilot auto-reads)
│   ├── workflows/
│   │   └── validate-skills.yml         ← CI validation workflow
│   └── skills/
│       ├── architectural-governance/
│       │   └── SKILL.md               ← Preserves architectural coherence
│       ├── anti-reimplementation/
│       │   └── SKILL.md               ← Prevents redundant code
│       ├── efficient-coding/
│       │   └── SKILL.md               ← Eliminates cognitive waste
│       ├── backend-integrity/
│       │   └── SKILL.md               ← Backend as source of truth
│       ├── frontend-boundary-protection/
│       │   └── SKILL.md               ← FE/BE layer separation
│       ├── senior-refactor-reviewer/
│       │   └── SKILL.md               ← Obsessive maintainability review
│       └── repository-semantic-map/
│           └── SKILL.md               ← Persistent system model
└── docs/
    └── DECISIONS.md                   ← Architecture Decision Record template
```

## Compatibility

- ✅ **GitHub Copilot** (VS Code) — full support (Agent Skills standard)
- ✅ **GitHub Copilot** (JetBrains, Neovim) — should work via same format
- ✅ **OpenCode** — native support
- ✅ **Cline / Roo Code** — compatible via Agent Skills standard
- ✅ **Any agent tool** — the SKILL.md format is a documented standard

## License

MIT — use freely in any project, personal or commercial.
