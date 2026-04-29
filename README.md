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

Follow these steps to add a new skill to the repository while keeping the existing pattern:

### 1. Create the skill folder

Create a new folder at the project root with a descriptive name in snake_case:

```
agents_skills/
└── my_new_skill/
```

### 2. Create the `SKILL.md`

Inside the folder, create a `SKILL.md` file with the required frontmatter and full skill instructions:

```markdown
---
name: my-skill-name
description: Clear and concise description of what the skill does and when the agent should use it.
---

# Skill Title

Detailed instructions on how the agent should execute the skill...
```

**Required frontmatter fields:**
- `name` — unique identifier in kebab-case (e.g. `code-review`, `test-generator`)
- `description` — sentence describing **what it does** and **when to use it**. Agents use this description to decide whether to activate the skill

### 3. Add templates (optional)

If the skill uses templates or auxiliary files, organize them in a `templates/` subfolder:

```
my_new_skill/
├── SKILL.md
└── templates/
    ├── template-a.md
    └── template-b.md
```

### 4. Update this README

Add a new entry under the **Available Skills** section following this format:

```markdown
### N. `folder_name/` — Short Title

**Path:** `folder_name/`
**Entry point:** `folder_name/SKILL.md`

Description of what the skill does and when to use it.

**When to use:**
- Situation 1
- Situation 2

**Included templates:** (if any)
- `templates/name.md`
```

### 5. Update the structure tree

In the **Structure** section of this README, add the new folder to the tree:

```
agents_skills/
├── README.md
├── doc_ai_skills/
├── my_new_skill/        # <-- new skill
│   ├── SKILL.md
│   └── templates/
└── ...
```

### Quick checklist

```
- [ ] Folder created at root with a snake_case name
- [ ] SKILL.md with frontmatter (name + description) and full instructions
- [ ] Templates in templates/ subfolder (if needed)
- [ ] README.md updated (Available Skills + Structure sections)
- [ ] Commit and push
```

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
