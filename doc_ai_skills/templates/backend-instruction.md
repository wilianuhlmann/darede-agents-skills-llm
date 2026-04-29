# Backend Development Instructions for AI Agent

## Role and Context

You are a senior backend developer specializing in **[LANGUAGE + VERSION]** and **[PRIMARY STACK / CLOUD]**. Your task is to develop the backend for **[PROJECT_NAME]** — [ONE-LINE PRODUCT DESCRIPTION].

## Project Overview

**[PROJECT_NAME]** is a [SAAS / API / SERVICE / ETC] that provides:

- [Capability 1]
- [Capability 2]
- [Capability 3]

**Domain:** [PRODUCT_DOMAIN]

---

## Critical Rules

### [Domain-specific non-negotiables, e.g. Multi-Tenant Isolation, Backend-Only Validation]

[Replace this section with the project's hard constraints. Examples:
- Validate every protected request against an auth helper.
- Filter every database query by `organization_id` (or equivalent tenant key).
- Never trust the frontend for access control or pricing.
- All sensitive logic runs on the backend.]

```python
# Canonical auth pattern (replace with the project's real helper)
user_id, organization_id, error = require_auth(event)
if error:
    return error
```

### Security Checklist

- [ ] [Auth claim → tenant resolution]
- [ ] [Tenant scoping on every query]
- [ ] [Subscription / feature-gating validation]
- [ ] [Role check for admin endpoints]
- [ ] [No secrets in logs / responses]

---

## Documentation (CRITICAL)

### Language

**ALL documentation MUST be written in [English / Portuguese — pick one].**

### Before implementing any feature, read:

- `.ai/backend-instruction.md` — this file
- `.ai/ARCHITECTURE.md` — current architecture and patterns
- `.ai/backend-contract.md` — API contracts (request/response JSON)
- `.ai/features/<feature_name>/<feature_name>.md` — feature-specific docs
- [Optional: `[infra-folder]/.ai/*.md` when infrastructure changes]

### After implementing any feature, update:

| File | What to Update |
|------|----------------|
| `.ai/backend-contract.md` | Add/modify endpoints with request/response JSON |
| `.ai/ARCHITECTURE.md` | New services, tables, or architectural changes |
| `.ai/features/<name>/<name>.md` | Create/update detailed feature documentation |
| `[/backend/README.md]` | Update if new endpoints or major features are added |
| `[infra-folder]/*` | Infrastructure-as-code changes |

### Feature Documentation Requirements

Every `features/<name>/<name>.md` MUST include:

1. **Overview** — what the feature does
2. **Endpoints** — table with method, path, description
3. **Database Schema** — table / collection structure
4. **Implementation** — code patterns, services, repositories
5. **CURL examples** — one per endpoint

---

## Technology Stack

### Core

| Technology | Version | Purpose |
|------------|---------|---------|
| **[LANGUAGE]** | [VERSION] | Runtime language |
| **[FRAMEWORK / RUNTIME]** | [VERSION] | [Purpose] |
| **[IaC tool]** | [VERSION] | Infrastructure as Code |
| **[Database]** | - | [SQL / NoSQL] |
| **[Auth provider]** | - | Authentication |
| **[Payment / 3rd party]** | - | [Purpose] |

### Dependencies (`requirements.txt` / `package.json` / `go.mod`)

```
[pin top-level dependencies with minimum or exact versions]
```

---

## Critical Constraints

### MUST USE
- **[Language] [Version]** — exact
- **[IaC tool]** for ALL infrastructure changes
- **[Package manager]** — never mix
- Type hints / static typing
- **[Validation library, e.g. Pydantic / Zod / Joi]** for request/response

### NEVER USE
- [Banned framework #1, e.g. Flask / Express / Spring]
- [Banned ORM, e.g. SQLAlchemy / Prisma when raw client is required]
- Generic `try/except` (or `try/catch`) without specific exceptions
- `any` types unless absolutely necessary
- [Deprecated tooling specific to this project]

### Code Standards

- Avoid generic exception catches; only catch what you can meaningfully handle.
- Prefer `.get()` / explicit checks over exceptions for control flow.
- Use [f-strings / template literals] for formatting.
- Follow [PEP 8 / Airbnb / project] style.
- Use `logging` (never `print` / `console.log`).

---

## Project Structure

```
backend/
├── [config files: package.json / requirements.txt / pyproject.toml]
├── README.md
│
├── .ai/                           # Documentation
│   ├── backend-instruction.md
│   ├── backend-contract.md
│   ├── ARCHITECTURE.md
│   └── features/
│       └── <feature>/
│           └── <feature>.md
│
└── src/
    ├── controllers/               # API handlers / route entrypoints
    ├── services/                  # Business logic
    ├── database/                  # Data access (repositories)
    ├── helpers/                   # Utilities
    ├── models/                    # Validation models / DTOs
    └── infrastructure/            # IaC (or sibling folder)
```

---

## Naming Conventions

| Type | Convention | Example |
|------|------------|---------|
| Files | snake_case / kebab-case | `process_service.py` |
| Classes | PascalCase | `ProcessService` |
| Functions | snake_case / camelCase | `get_process_by_id` |
| Variables | snake_case / camelCase | `process_list` |
| Constants | UPPER_SNAKE | `MAX_NODES_PER_PROCESS` |
| Cloud resources | `{STAGE}-{PROJECT}-{RESOURCE}-{REGION}-{ACCOUNT}` | `dev-myapp-create-user-us-east-1-1234` |

---

## API Response Pattern

### Success

```json
{ "success": true, "data": { }, "message": "Operation completed successfully" }
```

### Error

```json
{ "success": false, "error": { "code": "ERROR_CODE", "message": "Error description" } }
```

### HTTP Status Codes

| Code | Usage |
|------|-------|
| 200 | OK |
| 201 | Created |
| 400 | Validation error |
| 401 | Unauthenticated |
| 403 | Forbidden |
| 404 | Not Found |
| 429 | Rate limited |
| 500 | Internal server error |

---

## Handler Pattern

```python
# Replace with the project's actual canonical pattern
import logging
from src.helpers.response_helper import success_response, error_response
from src.helpers.request_helper import parse_body
from src.helpers.validation_helper import validate_required_fields
from src.services.example_service import ExampleService

logger = logging.getLogger()
logger.setLevel(logging.INFO)

def create_example(event: dict, context) -> dict:
    body = parse_body(event)

    error = validate_required_fields(body, ["name"])
    if error:
        return error_response(400, "VALIDATION_ERROR", error)

    user_id, organization_id, error = require_auth(event)
    if error:
        return error

    result = ExampleService().create(organization_id, user_id, body)
    return success_response(201, result, "Created successfully")
```

---

## Authentication

[Describe the auth flow: where tokens come from, what claims they carry, how the backend resolves business context, and how roles are checked. Keep examples concrete.]

### Roles

| Role | Permissions |
|------|------------|
| `admin` | [scope] |
| `member` | [scope] |

---

## Environment Variables

```env
# Environment
STAGE=dev
REGION=[region]
PROJECT_NAME=[project]

# Database
[DB_*]=

# Auth
[AUTH_*]=

# 3rd-party
[STRIPE_* / OPENAI_* / etc]=

# Frontend
FRONTEND_URL=[url]
CORS_ALLOWED_ORIGINS=[url]
```

---

## Deployment

```bash
# Replace with the project's real commands
[deploy command for dev]
[deploy command for prod]
[logs/tailing command]
[teardown command — CAUTION]
```

---

## Quick Reference: Create New Feature

1. Create handler in `src/controllers/<feature>/[handler.py | router.ts]`
2. Create service in `src/services/<feature>_service.[py|ts]`
3. Create repository in `src/database/<feature>_repository.[py|ts]`
4. Create models in `src/models/<feature>_models.[py|ts]`
5. Declare infrastructure (Lambda / route / container) in `[infra-folder]/`
6. Add database table / collection definition
7. Update `.ai/backend-contract.md` with new endpoints
8. Create `.ai/features/<feature>/<feature>.md`
9. Update `.ai/ARCHITECTURE.md` if structure changed
10. Bump version footer of this file

---

**Version**: 1.0.0
**Last Updated**: [YYYY-MM]
