# Satori — Complete Setup Guide

## Prerequisites

- VS Code with [GitHub Copilot](https://marketplace.visualstudio.com/items?itemName=GitHub.copilot) and [GitHub Copilot Chat](https://marketplace.visualstudio.com/items?itemName=GitHub.copilot-chat) extensions installed
- Active GitHub Copilot subscription (Individual, Business, or Enterprise)
- VS Code 1.96+ (required for custom agents)

---

## Step 1: Copilot Agents (`.agent.md` files)

Custom agents appear in the Copilot Chat agent selector dropdown.

> **Important:** Custom agents are NOT invoked with `@name` in chat. The `@` syntax (`@workspace`, `@github`) only works for built-in tools. To use a custom agent, select it from the dropdown instead.

### How to select an agent

1. Open Copilot Chat in VS Code
2. Click the agent selector (shows "Ask" or current agent name)
3. Choose your agent from the dropdown list
4. The chat mode changes to that agent — now type your request

### Where agents live in the repo

```
.github/agents/
├── satori-planner.agent.md       # Feature planning & decomposition
├── satori-backend.agent.md       # API & database implementation
├── satori-frontend.agent.md      # UI implementation
├── satori-debugger.agent.md      # Bug diagnosis & fixes
└── satori-arch-review.agent.md   # Quality gate review
```

### Where to copy them (VS Code prompts directory)

| OS | Path |
|----|------|
| **macOS** | `~/Library/Application Support/Code/User/prompts/` |
| **Linux** | `~/.config/Code/User/prompts/` |
| **Windows** | `%APPDATA%\Code\User\prompts\` |

### Installation commands

**macOS:**
```bash
cp .github/agents/*.agent.md ~/Library/Application\ Support/Code/User/prompts/
```

**Linux:**
```bash
cp .github/agents/*.agent.md ~/.config/Code/User/prompts/
```

**Windows (PowerShell):**
```powershell
copy-item .github/agents/*.agent.md $env:APPDATA\Code\User\prompts\
```

### Verify

Open VS Code → Copilot Chat → click the agent selector dropdown (shows "Ask" or current agent name).

You should see: **Satori Planner**, **Satori Backend**, **Satori Frontend**, **Satori Debugger**, **Satori Arch Review**.

> **Important:** After copying, close and reopen VS Code, or reload the window (`Ctrl+Shift+P` → `Developer: Reload Window`).

---

## Step 2: Skills (`SKILL.md` files)

Skills are instruction files that Copilot loads when you reference them.

### Where skills live in the repo

```
.github/skills/
├── architectural-governance/SKILL.md
├── anti-reimplementation/SKILL.md
├── efficient-coding/SKILL.md
├── backend-integrity/SKILL.md
├── frontend-boundary-protection/SKILL.md
├── senior-refactor-reviewer/SKILL.md
└── repository-semantic-map/SKILL.md
```

### Installation

**Option A: Personal (recommended — available in ALL projects):**

```bash
mkdir -p ~/.copilot/skills

for skill in architectural-governance anti-reimplementation efficient-coding \
             backend-integrity frontend-boundary-protection \
             senior-refactor-reviewer repository-semantic-map; do
  ln -sf "$(pwd)/.github/skills/$skill" ~/.copilot/skills/
done
```

Symlinks update automatically when you `git pull` the repo.

**Option B: Per-project (isolated to one repo):**

```bash
mkdir -p /path/to/your/project/.github/skills
cp -r .github/skills/* /path/to/your/project/.github/skills/
```

---

## Step 3: VS Code Configuration (CRITICAL)

This is the **most common reason skills don't work**. You must add the following to your VS Code settings.

### Approach A: Project settings (works for this repo)

Open `.vscode/settings.json` (already included in the repo):

```json
{
  "github.copilot.chat.agents.enabled": true,
  "github.copilot.chat.codeGeneration.instructions": [
    { "file": ".github/copilot-instructions.md" }
  ],
  "github.copilot.chat.skills.instructionFiles": [
    "~/.copilot/skills/*/SKILL.md"
  ]
}
```

This works when you open this repo in VS Code. But skills in `~/.copilot/skills/` may not be discovered reliably with relative config alone.

### Approach B: User settings (RECOMMENDED — works everywhere)

Open **User** settings.json:

**macOS:** `Cmd+Shift+P` → `Open User Settings (JSON)`  
**Linux/Windows:** `Ctrl+Shift+P` → `Open User Settings (JSON)`

Add these entries:

**macOS:**
```jsonc
{
  // ... existing settings ...

  "github.copilot.chat.skills.instructionFiles": [
    "/Users/TU_USUARIO/.copilot/skills/*/SKILL.md"
  ],
  "github.copilot.chat.codeGeneration.instructions": [
    { "file": "/Users/TU_USUARIO/.copilot/skills/architectural-governance/SKILL.md" },
    { "file": "/Users/TU_USUARIO/.copilot/skills/anti-reimplementation/SKILL.md" },
    { "file": "/Users/TU_USUARIO/.copilot/skills/efficient-coding/SKILL.md" },
    { "file": "/Users/TU_USUARIO/.copilot/skills/backend-integrity/SKILL.md" },
    { "file": "/Users/TU_USUARIO/.copilot/skills/frontend-boundary-protection/SKILL.md" },
    { "file": "/Users/TU_USUARIO/.copilot/skills/senior-refactor-reviewer/SKILL.md" },
    { "file": "/Users/TU_USUARIO/.copilot/skills/repository-semantic-map/SKILL.md" }
  ]
}
```

**Linux:**
```jsonc
{
  "github.copilot.chat.skills.instructionFiles": [
    "/home/TU_USUARIO/.copilot/skills/*/SKILL.md"
  ],
  "github.copilot.chat.codeGeneration.instructions": [
    { "file": "/home/TU_USUARIO/.copilot/skills/architectural-governance/SKILL.md" },
    { "file": "/home/TU_USUARIO/.copilot/skills/anti-reimplementation/SKILL.md" },
    { "file": "/home/TU_USUARIO/.copilot/skills/efficient-coding/SKILL.md" },
    { "file": "/home/TU_USUARIO/.copilot/skills/backend-integrity/SKILL.md" },
    { "file": "/home/TU_USUARIO/.copilot/skills/frontend-boundary-protection/SKILL.md" },
    { "file": "/home/TU_USUARIO/.copilot/skills/senior-refactor-reviewer/SKILL.md" },
    { "file": "/home/TU_USUARIO/.copilot/skills/repository-semantic-map/SKILL.md" }
  ]
}
```

**Windows:**
```jsonc
{
  "github.copilot.chat.skills.instructionFiles": [
    "C:\\Users\\TU_USUARIO\\.copilot\\skills\\*\\SKILL.md"
  ],
  "github.copilot.chat.codeGeneration.instructions": [
    { "file": "C:\\Users\\TU_USUARIO\\.copilot\\skills\\architectural-governance\\SKILL.md" },
    { "file": "C:\\Users\\TU_USUARIO\\.copilot\\skills\\anti-reimplementation\\SKILL.md" },
    { "file": "C:\\Users\\TU_USUARIO\\.copilot\\skills\\efficient-coding\\SKILL.md" },
    { "file": "C:\\Users\\TU_USUARIO\\.copilot\\skills\\backend-integrity\\SKILL.md" },
    { "file": "C:\\Users\\TU_USUARIO\\.copilot\\skills\\frontend-boundary-protection\\SKILL.md" },
    { "file": "C:\\Users\\TU_USUARIO\\.copilot\\skills\\senior-refactor-reviewer\\SKILL.md" },
    { "file": "C:\\Users\\TU_USUARIO\\.copilot\\skills\\repository-semantic-map\\SKILL.md" }
  ]
}
```

> 🔑 **Key insight:** The `codeGeneration.instructions` with absolute paths is what makes Copilot actually load these skills. Without it, mentioning `@skill-name` in chat may not find the skill file.

---

## Step 4: Project Instructions (`.github/copilot-instructions.md`)

This file is **automatically** read by Copilot on every query — no configuration needed.

The repo already includes `.github/copilot-instructions.md` with 8 sections:
1. **Always-On Skills** — which skills to use
2. **Code Style & Conventions** — naming, structure, TypeScript rules
3. **Git & Commits** — best practices
4. **Error Handling** — typed exceptions, never swallow
5. **Security** — authentication, validation, no secrets
6. **Architecture Principles** — data flow, bounded contexts
7. **AI Interaction Guidelines** — how to ask and answer
8. **Project Recognition** — auto-detection patterns

You can customize this file per-project — Copilot reads it automatically when opening the repo.

---

## Step 5: Use `copilot-instructions.md` Template (optional)

If you don't want the full `copilot-instructions.md` in your project's `.github/`, copy the template:

```bash
cp .github/copilot-instructions.md /path/to/your/project/.github/copilot-instructions.md
```

Then customize it to your project's stack and conventions.

---

## Step 6: Understanding `AGENTS.md` vs `.agent.md` files

| File | Purpose | Read by VS Code? |
|------|---------|-----------------|
| `.github/agents/*.agent.md` | Custom agents (appear in dropdown) | ✅ Yes — after copying to prompts/ |
| `AGENTS.md` (root) | Project-level agent definitions for OpenCode/Cline | ❌ No — VS Code Copilot ignores it |

> **Important:** The `AGENTS.md` in the root is kept for compatibility with other tools (OpenCode, Cline). For VS Code Copilot, only the `.agent.md` files in `~/.config/Code/User/prompts/` matter.

---

## Reference: VS Code Valid Tools

In VS Code Copilot `.agent.md` files, only these tools are valid for the `tools:` field:

| Tool | Purpose |
|------|---------|
| `read` | Read files from the workspace |
| `search` | Search/grep the workspace |
| `edit` | Write and edit files |
| `runCommand` | Execute terminal commands |
| `tentativeEdit` | Propose diffs that user applies manually |

> `command` and `execute` are **NOT valid** in VS Code Copilot — use `runCommand` instead.

---

## Reference: Order of Precedence

Copilot loads instructions from multiple sources. When there's a conflict, **the more specific source wins**:

```
codeGeneration.instructions (User settings.json)
       ↓ (higher precedence)
copilot instructions template (if configured)
       ↓
.github/copilot-instructions.md (project-level)
       ↓
.agent.md file instructions (agent-specific)
       ↓ (lower precedence — base/default)
```

- **`codeGeneration.instructions`** in User `settings.json` has the **highest** precedence — it overrides everything below
- **`.github/copilot-instructions.md`** is project-level defaults — loaded automatically
- **`.agent.md`** instructions apply only when that specific agent is selected

If you add skill paths to `codeGeneration.instructions` (as shown in Step 3B), those skills will be loaded on **every query**, not just when you mention them.

---

## Verify Installation

### 1. Test if a Skill is Loaded

The most reliable way: ask a specific question that only someone who read the SKILL.md would know.

**Test 1 — Core directive (basic load):**
```
@architectural-governance What is your core directive?
```
Expected: *"Your primary goal is NOT to generate code quickly. Your goal is to preserve cumulative architectural coherence."*

**Test 2 — Specific content (deep load):**
```
@architectural-governance What are the five anti-patterns you look for?
```
Expected: Helper Explosion, Fat Interface, Mock Layer, Copy-Paste Inheritance, God Module.

**Test 3 — Skill from codeGeneration.instructions (no @mention):**
```
Without mentioning any skill name — how should I prevent redundant code?
```
If loaded via `codeGeneration.instructions`, Copilot should answer using the anti-reimplementation principles. If it doesn't, it means the skills are not in `codeGeneration.instructions`.

### 2. Test a Custom Agent

> Select from the dropdown — do NOT type `@Satori Planner`

Open Copilot Chat → click the agent selector → choose **Satori Planner** → type:

```
I need a user profile feature with avatar upload. Plan it for me.
```

Expected: A structured plan with layers (DB → API → FE).

### 3. Test `copilot-instructions.md`

In any Copilot Chat, ask:

```
What are the code conventions for this project?
```

Expected response should reference the project's naming conventions from `copilot-instructions.md`.

### 4. Test Agent Tools

Select **Satori Backend** from the agent dropdown, then:

```
Run ls in the root directory
```

Expected: Copilot should execute the command. If it fails, your `.agent.md` may use `command` instead of `runCommand`.

---

## Troubleshooting

| Symptom | Likely cause | Fix |
|---------|-------------|-----|
| `@architectural-governance` does nothing | `skills.instructionFiles` not configured | Add to User settings.json (Step 3) |
| Agent not in dropdown | `.agent.md` not in VS Code prompts dir | Copy to correct path (Step 1), reload window |
| Copilot ignores instructions | `copilot-instructions.md` not at `.github/copilot-instructions.md` | Check file location and name |
| Skills load in this repo but not others | Configured in project settings, not user settings | Move `codeGeneration.instructions` to User settings (Step 3B) |
| Skills worked before update, now don't | Symlinks broken after git changes | Run the symlink commands again |
| "No matching skill found" | Wrong path in `skills.instructionFiles` | Double-check the glob pattern — must end with `/*/SKILL.md` |

---

## File Mapping Reference

| What | Repository source | Installation target |
|------|-------------------|-------------------|
| **Custom agents** | `.github/agents/*.agent.md` | VS Code prompts directory |
| **Skills** | `.github/skills/<name>/SKILL.md` | `~/.copilot/skills/<name>/SKILL.md` |
| **Settings** | `.vscode/settings.json` | User settings.json |
| **Instructions** | `.github/copilot-instructions.md` | Same path (auto-discovered) |
