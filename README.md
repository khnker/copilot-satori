<div align="center">
  <img src="logo.svg" alt="Copilot Satori" width="180" height="180">
  <h1>Copilot Satori</h1>
  <p><strong>Portable Agent Skills for GitHub Copilot</strong></p>
  <p>Architectural governance, refactoring discipline, layer integrity, and code review standards — all in a copy-pasteable format.</p>
</div>

---

## What's Inside

7 professional-grade **Agent Skills** plus 5 **custom Copilot agents** and comprehensive **project instructions** — designed to transform GitHub Copilot from a chatty autocomplete into a disciplined engineering assistant.

| Skill | Purpose |
|-------|---------|
| 🏗️ **architectural-governance** | Preserves cumulative architectural coherence. Prevents premature abstractions, helper explosion, and layer violations. |
| ♻️ **anti-reimplementation** | Prevents redundant code. Always searches for existing implementations before writing new ones. |
| ⚡ **efficient-coding** | Eliminates cognitive and structural waste. Small focused edits, no dead ends, no premature abstraction. |
| 🛡️ **backend-integrity** | Enforces backend as source of truth. Transactional integrity, security boundaries, API contract stability. |
| 🧩 **frontend-boundary-protection** | Prevents logic contamination in the frontend. Business logic belongs in the API — FE is presentation only. |
| 🔍 **senior-refactor-reviewer** | Acts as a senior code reviewer. Checks readability, testability, changeability, and coupling. |
| 🗺️ **repository-semantic-map** | Maintains a persistent mental model of the system. Detects drift, tracks decisions, onboards faster. |

Also includes:
- **5 custom agents** (`.github/agents/*.agent.md`): planner, backend, frontend, debugger, arch-review — appear in the Copilot Chat agent selector.
- **`.github/copilot-instructions.md`**: 8 sections of best practices — automatically read by Copilot on every query.
- **`.vscode/settings.json`** + **`settings.example.json`**: VS Code configuration with examples for all 3 OSes (macOS, Linux, Windows).
- **`docs/DECISIONS.md`**: Architecture Decision Record template.
- **CI workflow**: Validates skill formatting on every PR and push.

---

## Installation

> 📖 **Full step-by-step guide → [`GUIDE.md`](GUIDE.md)**

**Quick summary:**

1. **Install skills** — symlink `.github/skills/*` to `~/.copilot/skills/`
2. **Install agents** — copy `.github/agents/*.agent.md` to VS Code prompts directory
3. **Configure VS Code** — add `codeGeneration.instructions` with absolute paths to User settings.json
4. **Verify** — ask `@architectural-governance What is your core directive?`

See [`GUIDE.md`](GUIDE.md) for exact commands for your OS and troubleshooting.

---

## How to Use

### Skills (in Copilot Chat)

```
@architectural-governance Review this controller for me
@anti-reimplementation Is there existing code for export?
@backend-integrity Are these service methods transactional?
@frontend-boundary-protection Is this component violating FE boundaries?
@efficient-coding Simplify this function
@senior-refactor-reviewer Review this PR diff
@repository-semantic-map Load the semantic map for this repo
```

### Custom Agents (agent selector dropdown)

Select from the Copilot Chat dropdown, then describe your task:

| Agent | When to use |
|-------|-------------|
| **Satori Planner** | Starting a new feature — decomposes work into layers |
| **Satori Backend** | Implementing API endpoints, services, database migrations |
| **Satori Frontend** | Building UI components, pages, API integration |
| **Satori Debugger** | Fixing bugs, investigating test failures |
| **Satori Arch Review** | Validating cross-layer alignment before committing |

### Project Instructions

`.github/copilot-instructions.md` is automatically loaded by Copilot on every query.
It covers: skills, code style, git, error handling, security, architecture principles, AI interaction guidelines.

---

## Repository Structure

```
copilot-satori/
├── README.md                           ← This file
├── GUIDE.md                            ← Step-by-step installation guide
├── AGENTS.md                           ← Project-level agent definitions
├── logo.svg                            ← Project logo
├── .github/
│   ├── agents/                         ← Custom .agent.md files (copy to VS Code prompts dir)
│   │   ├── satori-planner.agent.md
│   │   ├── satori-backend.agent.md
│   │   ├── satori-frontend.agent.md
│   │   ├── satori-debugger.agent.md
│   │   └── satori-arch-review.agent.md
│   ├── copilot-instructions.md         ← Best practices (Copilot auto-reads)
│   ├── workflows/
│   │   └── validate-skills.yml         ← CI validation workflow
│   └── skills/
│       ├── architectural-governance/SKILL.md
│       ├── anti-reimplementation/SKILL.md
│       ├── efficient-coding/SKILL.md
│       ├── backend-integrity/SKILL.md
│       ├── frontend-boundary-protection/SKILL.md
│       ├── senior-refactor-reviewer/SKILL.md
│       └── repository-semantic-map/SKILL.md
├── .vscode/
│   ├── settings.json                   ← Project-level Copilot config
│   └── settings.example.json           ← User settings examples (all 3 OSes)
└── docs/
    └── DECISIONS.md                    ← Architecture Decision Record template
```

---

## Compatibility

- ✅ **GitHub Copilot** (VS Code) — full support
- ✅ **Cline / Roo Code** — compatible via Agent Skills standard
- ✅ **OpenCode** — native support
- ✅ **Any agent tool** — the SKILL.md format is a documented standard

## License

MIT — use freely in any project, personal or commercial.
