# Agents Skills

Central repository of reusable skills for AI coding agents.
Compatible with **Cursor**, **Claude Code**, **Opencode**, **Kiro**, and any agent that supports skill/instruction files.

Each subfolder contains an independent skill with its own `SKILL.md` and associated resources.

**Base path:** `/Users/wilianuhlmann/Documents/Projetos/agents_skills/`

---

## Available Skills

### 1. `doc_ai_skills/` — AI Project Documentation Pattern

**Path:** `doc_ai_skills/`
**Entry point:** `doc_ai_skills/SKILL.md`

Creates and maintains a standardized `.ai/` documentation structure for backend, frontend, and agentcore projects. Ensures coding agents always have authoritative instructions, architecture, API contracts, and per-feature docs to read before implementing any change.

**When to use:**
- Bootstrap documentation for a new project
- Create or update `instruction.md`, `contract.md`, `ARCHITECTURE.md`, or feature docs
- Add a new feature that requires keeping docs in sync

**Included templates:**
- `templates/backend-instruction.md`
- `templates/frontend-instruction.md`
- `templates/agentcore-instruction.md`
- `templates/architecture.md`
- `templates/contract.md`
- `templates/feature.md`

---

## How to Add a New Skill

1. Create a new folder at the root with a descriptive name (e.g. `code_review_skill/`)
2. Add a `SKILL.md` at the folder root with the standard frontmatter (`name`, `description`) and full skill instructions
3. Include templates or auxiliary resources in subfolders as needed
4. Update this README by adding a new entry under **Available Skills**

---

## Structure

```
agents_skills/
├── README.md              # this file (general index)
├── doc_ai_skills/         # .ai/ documentation skill
│   ├── SKILL.md
│   └── templates/
│       ├── agentcore-instruction.md
│       ├── architecture.md
│       ├── backend-instruction.md
│       ├── contract.md
│       ├── feature.md
│       └── frontend-instruction.md
└── <new_skill>/           # next skills go here
    └── SKILL.md
```
