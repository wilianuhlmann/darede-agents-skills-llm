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

Follow these steps to add uma nova skill ao repositório, mantendo o padrão existente:

### 1. Crie a pasta da skill

Crie uma nova pasta na raiz do projeto com um nome descritivo em snake_case:

```
agents_skills/
└── minha_nova_skill/
```

### 2. Crie o `SKILL.md`

Dentro da pasta, crie um arquivo `SKILL.md` com o frontmatter obrigatório e as instruções completas da skill:

```markdown
---
name: nome-da-skill
description: Descrição clara e objetiva do que a skill faz e quando deve ser usada pelo agente.
---

# Título da Skill

Instruções detalhadas de como o agente deve executar a skill...
```

**Campos obrigatórios no frontmatter:**
- `name` — identificador único em kebab-case (ex: `code-review`, `test-generator`)
- `description` — frase que descreve **o que faz** e **quando usar**. Os agentes usam essa descrição para decidir se devem ativar a skill

### 3. Adicione templates (opcional)

Se a skill utiliza templates ou arquivos auxiliares, organize-os em uma subpasta `templates/`:

```
minha_nova_skill/
├── SKILL.md
└── templates/
    ├── template-a.md
    └── template-b.md
```

### 4. Atualize este README

Adicione uma nova entrada na seção **Available Skills** seguindo o formato:

```markdown
### N. `nome_da_pasta/` — Título Curto

**Path:** `nome_da_pasta/`
**Entry point:** `nome_da_pasta/SKILL.md`

Descrição do que a skill faz e quando usá-la.

**When to use:**
- Situação 1
- Situação 2

**Included templates:** (se houver)
- `templates/nome.md`
```

### 5. Atualize a árvore de estrutura

Na seção **Structure** deste README, adicione a nova pasta à árvore:

```
agents_skills/
├── README.md
├── doc_ai_skills/
├── minha_nova_skill/    # <-- nova skill
│   ├── SKILL.md
│   └── templates/
└── ...
```

### Checklist rápido

```
- [ ] Pasta criada na raiz com nome em snake_case
- [ ] SKILL.md com frontmatter (name + description) e instruções completas
- [ ] Templates em subpasta templates/ (se necessário)
- [ ] README.md atualizado (seção Available Skills + Structure)
- [ ] Commit e push
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
