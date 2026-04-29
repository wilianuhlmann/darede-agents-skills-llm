# [PROJECT_NAME] — Architecture Documentation

## Overview

[One paragraph describing what this codebase does, the runtime model (serverless, container, monolith), and how it fits into the larger system.]

---

## Critical Architectural Principles

[List the non-negotiables that shape every component. Examples:]

1. **Multi-tenant isolation** — every storage row is keyed by `[tenant_id]`.
2. **Backend-only validation** — never trust client-supplied authorization claims.
3. **Single source of truth for auth** — JWT issuer is `[provider]`; business attributes are resolved from `[users table]`.
4. **One [Lambda / service / handler] per [endpoint / responsibility]** — no router/proxy patterns.
5. **Idempotent infrastructure** — `[IaC tool]` is the only way to change cloud resources.

---

## Technology Stack

| Category | Technology | Version |
|----------|------------|---------|
| Language | [Lang] | [x.y] |
| IaC | [Tool] | [x.y] |
| Compute | [Lambda / ECS / EC2] | - |
| API | [Gateway / Ingress] | - |
| Database | [DynamoDB / Postgres / Mongo] | - |
| Auth | [Cognito / Auth0 / Keycloak] | - |
| AI / LLM | [Bedrock / OpenAI / Anthropic] | - |
| Storage | [S3 / GCS] | - |
| Email | [SES / SendGrid] | - |
| Payments | [Stripe / etc] | - |
| Monitoring | [CloudWatch / Datadog] | - |

---

## Architecture Diagram

```
┌──────────────────────────────────────────────────────────────────────────┐
│                          [Edge / API Layer]                               │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐                    │
│  │   Route 1    │  │   Route 2    │  │     ...      │                    │
│  └──────┬───────┘  └──────┬───────┘  └──────┬───────┘                    │
└─────────┼──────────────────┼──────────────────┼──────────────────────────┘
          ▼                  ▼                  ▼
┌──────────────────────────────────────────────────────────────────────────┐
│                       [Compute / Service Layer]                           │
│   handler → service → repository → [database / external API]              │
└──────────────────────────────────────────────────────────────────────────┘
          │                  │
          ▼                  ▼
   ┌──────────────┐    ┌──────────────────────┐
   │  [Database]  │    │ [External services]  │
   └──────────────┘    └──────────────────────┘
```

> Replace with the real diagram. ASCII is fine. The point is to show the request lifecycle and data flow at a glance.

---

## Module Layout

| Layer | Responsibility | Folder |
|-------|----------------|--------|
| Controllers / Handlers | Request parsing, validation, response shaping | `src/controllers/` |
| Services | Business logic, orchestration | `src/services/` |
| Repositories | Data access, queries | `src/database/` |
| Helpers | Pure utilities, auth, response builders | `src/helpers/` |
| Models | Validation schemas / DTOs | `src/models/` |

---

## Storage / Data Model

| Table / Collection | Partition Key | Sort Key | Purpose | GSIs |
|---|---|---|---|---|
| `[organizations]` | `organization_id` | - | Tenant root | - |
| `[users]` | `organization_id` | `user_id` | Members | `EmailIndex` |
| `[[other tables]]` | `organization_id` | `[id]` | [purpose] | - |

### Tenant isolation rule

Every query MUST include `organization_id` (or equivalent tenant key) in its key condition.

---

## Auth Flow

1. Client authenticates with `[provider]` and receives a JWT.
2. Client calls protected endpoint with `Authorization: Bearer <token>`.
3. `[auth helper]` verifies the JWT, extracts `email`, queries `[users table]` to resolve `organization_id` and `role`.
4. Handler proceeds only if `role` matches the endpoint's requirement.

---

## Cross-Cutting Concerns

### Logging
- Use `[logging library]` only. Never `print` / `console.log`.
- Log `request_id`, `organization_id`, `user_id` on every entry.

### Error Handling
- Standard error envelope (see `contract.md`).
- Specific exception classes — no bare `except`.

### Observability
- Structured logs → `[CloudWatch / Datadog]`.
- Metrics: `[business metrics list]`.
- Tracing: `[X-Ray / OpenTelemetry]`.

### Security
- Secrets in `[Secrets Manager / Vault / SSM]` — never in code.
- All requests over HTTPS.
- CORS allowlist limited to the production origin.

---

## Deployment Topology

| Stage | URL | Account | Notes |
|-------|-----|---------|-------|
| dev | [dev url] | [account id] | Open to team |
| prod | [prod url] | [account id] | Tagged releases only |

---

## Decision Log

| Date | Decision | Reason |
|------|----------|--------|
| [YYYY-MM] | [e.g. Migrated from Serverless Framework to Terraform] | [reason] |

---

**Last Updated**: [YYYY-MM]
