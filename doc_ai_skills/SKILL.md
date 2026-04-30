---
name: ai-project-docs
description: Creates and maintains a standardized `.ai/` documentation structure for backend, frontend, and agentcore (AI agent) projects so coding agents always have authoritative instructions, architecture, API contracts, and per-feature docs to read before implementing changes. Use when the user asks to bootstrap project docs, set up `.ai/` folders, write/update an `instruction.md`, `contract.md`, `ARCHITECTURE.md`, or feature documentation, or when adding a new feature that requires keeping these docs in sync.
---

# AI Project Documentation Pattern

This skill installs and maintains a consistent documentation layout that an AI agent should always read **before implementing** and always update **after implementing** any change. It works across three project types:

- **backend** — APIs, Lambdas, infrastructure
- **frontend** — web/mobile clients
- **agentcore** — AI agents, runtimes, MCP gateways, memory, knowledge bases

Each project type gets its own `.ai/` folder at the project root.

---

## When to Apply

Apply this skill whenever the user:

- Asks to "bootstrap docs", "create the `.ai` folder", "scaffold AI instructions", or similar.
- Starts a new backend / frontend / agentcore project.
- Adds a new feature (`features/<feature_name>/<feature_name>.md` must be created and the contract/architecture updated).
- Asks the agent to follow or enforce the documentation pattern.
- Mentions `instruction.md`, `contract.md`, `ARCHITECTURE.md`, or `.ai/features/`.

If unclear which project type applies, ask: backend, frontend, agentcore, or all three.

---

## Folder Layout

Each project type uses the same shape, with project-specific filenames:

```
<project_root>/
└── .ai/
    ├── <type>-instruction.md     # main entry point the agent reads first
    ├── ARCHITECTURE.md           # current architecture, components, data flow
    ├── contract.md               # API / interface / tool contracts
    └── features/
        └── <feature_name>/
            └── <feature_name>.md
```

### Filename conventions per project type

| Project type | Instruction file | Contract file | Architecture file | Feature file path |
|---|---|---|---|---|
| backend | `backend-instruction.md` | `backend-contract.md` | `ARCHITECTURE.md` | `features/<name>/<name>.md` |
| frontend | `frontend-instruction.md` | `contract.md` | `ARCHITECTURE.md` | `features/<name>/<name>.md` |
| agentcore | `instruction.md` | `contract.md` | `architecture.md` | `features/<name>.md` |

Filenames are intentional. Keep them as listed so the instruction file references resolve.

---

## The Instruction File Is the Entry Point

The instruction file (`<type>-instruction.md` or `instruction.md`) is what the agent reads **first**. Every other doc is linked from it. It must contain, in this order:

1. **Role and Context** — who the agent is acting as (e.g. "senior backend developer specializing in Python 3.12, AWS Lambda, Terraform").
2. **Project Overview** — one paragraph describing what the product does.
3. **Critical Rules** — non-negotiables (security, multi-tenancy, language, naming).
4. **Documentation map** — explicit list of files the agent MUST read before any change AND files the agent MUST update after any change. Include a table of `File → What to Update`.
5. **Technology Stack** — table of tools and exact pinned versions.
6. **MUST USE / NEVER USE** — explicit allow/deny list of frameworks, libraries, patterns.
7. **Code Standards** — language style rules, naming conventions, error handling patterns.
8. **Project Structure** — tree of the source folder.
9. **Patterns** — handler/controller/service/repository templates, response shape, auth pattern.
10. **Environment Variables** — sample `.env` block.
11. **Deployment** — exact commands and scripts.
12. **Quick Reference: Create New Feature** — numbered checklist that ends with "Update `.ai/contract.md`" and "Create `.ai/features/<name>/<name>.md`".
13. **Version + Last Updated** footer.

Keep the instruction file authoritative and current — when versions, structure, or rules change, update it in the same change.

---

## Workflow

### Bootstrapping a new project

Copy this checklist and execute in order:

```
- [ ] Confirm project type (backend / frontend / agentcore) and project root path
- [ ] Create `.ai/` and `.ai/features/` directories at the project root
- [ ] Copy the matching instruction template from `templates/` and rename per the table above
- [ ] Copy `templates/architecture.md` → `.ai/ARCHITECTURE.md` (or `architecture.md` for agentcore)
- [ ] Copy `templates/contract.md` → `.ai/<contract filename>`
- [ ] Fill in product overview, stack versions, env vars, deployment commands
- [ ] Remove placeholder sections that do not apply
- [ ] Commit
```

### Adding a feature

```
- [ ] Read the instruction file, ARCHITECTURE, and contract first
- [ ] Implement the feature
- [ ] Create `features/<feature_name>/<feature_name>.md` from `templates/feature.md`
- [ ] Append/modify endpoints in the contract file
- [ ] Update ARCHITECTURE if components, tables, or data flow changed
- [ ] Update the instruction file's project structure or "MUST USE" lists if needed
- [ ] Bump the `Version` + `Last Updated` in the instruction file footer
```

### Maintaining docs (drift check)

When asked to "audit", "sync", or "update" the docs:

1. Walk the source tree and list real endpoints/components/tools.
2. Diff against `contract.md` — flag missing/extra entries.
3. Diff against `ARCHITECTURE.md` — flag missing components.
4. List `.ai/features/` and confirm each top-level feature has a doc.
5. Report a concise diff and ask before rewriting large sections.

---

## Templates

Use the templates as starting points. They are intentionally generic; replace the bracketed `[PLACEHOLDERS]` with project specifics before saving.

- Backend instruction: [templates/backend-instruction.md](templates/backend-instruction.md)
- Frontend instruction: [templates/frontend-instruction.md](templates/frontend-instruction.md)
- Agentcore instruction: [templates/agentcore-instruction.md](templates/agentcore-instruction.md)
- Architecture: [templates/architecture.md](templates/architecture.md)
- Contract: [templates/contract.md](templates/contract.md)
- Feature: [templates/feature.md](templates/feature.md)

When copying, preserve the section order — agents rely on it as a map.

---

## Authoring Rules

1. **Single source of truth** — the instruction file owns the rules; other files own details. Never duplicate rules; link instead.
2. **Explicit MUST USE / NEVER USE** — vague guidance gets ignored by agents. Pin versions and ban specific frameworks by name.
3. **Tables over prose** for endpoints, env vars, tech stack, and roles.
4. **Code blocks for patterns** — show the canonical handler / component / tool shape so agents can mirror it.
5. **English only** — all generated documentation, file names, comments, commit messages, and section headings MUST be written in English. This applies to every `.ai/` file, template, and skill output. Do not mix languages; English is the single standard for this repository and all skills it produces.
6. **Reference, don't deep-nest** — link directly from the instruction file to other `.ai/` files, one level deep.
7. **Feature docs are mandatory** — every feature with a backend endpoint, a frontend route, or an agent tool gets its own `features/<name>/<name>.md` (or `features/<name>.md` for agentcore).
8. **Old features** — move deprecated docs to `features/old-consult/` (or similar) instead of deleting; agents may need to reference past patterns.
9. **No time-sensitive instructions** in the body. Put dates only in the footer (`Version` + `Last Updated`).

---

## Anti-Patterns

- A single `README.md` instead of split `.ai/` files. Agents skim READMEs; they read instruction files.
- Contracts that document only happy paths. Always include error response shape and status codes.
- Architecture docs that are pure prose. Agents need diagrams (ASCII is fine) and tables.
- Feature docs without endpoint tables / tool tables / curl examples.
- Instruction files longer than ~700 lines. Split detail into the architecture or feature docs and link.
