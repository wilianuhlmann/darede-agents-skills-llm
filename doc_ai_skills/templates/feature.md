# Feature: [Feature Name]

> **Related Documentation**
> - Backend: `backend/.ai/features/[feature]/[feature].md`
> - Frontend: `frontend/.ai/features/[feature]/[feature].md`
> - Agentcore: `agentcore/.ai/features/[feature].md`
> - Contract entries: `[contract.md#section]`
> - Architecture: `[ARCHITECTURE.md#section]`

## Overview

[What this feature does, why it exists, and the key capabilities it delivers. 2–4 sentences.]

### Architecture Principle

[Optional. The single most important rule for this feature — e.g. "All operations are scoped by `organization_id`. Cross-tenant access is forbidden."]

---

## Endpoints (backend) / Routes (frontend) / Tools (agentcore)

> Use the section that matches your project type.

### Backend — Endpoints

| Method | Path | Auth | Description |
|--------|------|------|-------------|
| POST | `/[resource]` | Bearer | Create [resource] |
| GET | `/[resource]` | Bearer | List [resource] (paginated) |
| GET | `/[resource]/{id}` | Bearer | Get [resource] details |
| PUT | `/[resource]/{id}` | Bearer (admin) | Update [resource] |
| DELETE | `/[resource]/{id}` | Bearer (admin) | Delete [resource] |

### Frontend — Routes / Components

| Route | Component | Auth | Description |
|-------|-----------|------|-------------|
| `/[path]` | `[PageComponent]` | protected | [purpose] |

| Component | Path | Props | Notes |
|-----------|------|-------|-------|
| `[Component]` | `src/components/[domain]/[Component].tsx` | `{ id, onSave }` | [purpose] |

### Agentcore — Tools

| Tool name | Schema | Lambda | Mode | Purpose |
|-----------|--------|--------|------|---------|
| `[tool_name]` | `src/schemas/[name].json` | `agentcore-[name]-lambda` | read / write | [purpose] |

---

## Data Model

### Tables / Collections

| Table | Partition Key | Sort Key | GSIs | Notes |
|-------|---------------|----------|------|-------|
| `[table]` | `organization_id` | `[id]` | `[GSI]` | [notes] |

### Entity shape

```json
{
  "[id]": "uuid",
  "organization_id": "uuid",
  "name": "string",
  "status": "draft | published | archived",
  "created_by": "uuid",
  "created_at": "2026-01-01T00:00:00Z",
  "updated_at": "2026-01-01T00:00:00Z"
}
```

---

## Implementation

### Files

| Layer | File |
|-------|------|
| Handler / Controller | `src/controllers/[feature]/handler.[ext]` |
| Service | `src/services/[feature]_service.[ext]` |
| Repository | `src/database/[feature]_repository.[ext]` |
| Models / Schemas | `src/models/[feature]_models.[ext]` |
| Infrastructure | `[infra-folder]/[feature].[tf|yml]` |

### Canonical handler example

```[python|typescript]
# Replace with the project's actual canonical pattern
```

### Validation rules

- [Rule 1]
- [Rule 2]

### State transitions (if applicable)

```
draft ─► published ─► archived
   ▲          │
   └──────────┘
```

---

## Frontend State (if applicable)

| Store | Path | Purpose |
|-------|------|---------|
| `[useFeatureStore]` | `src/store/[feature]Store.ts` | [purpose] |

---

## CURL Examples

```bash
# Create
curl -X POST "$BASE_URL/[resource]" \
  -H "Authorization: Bearer $TOKEN" \
  -H "Content-Type: application/json" \
  -d '{"name":"Example"}'

# List
curl "$BASE_URL/[resource]" -H "Authorization: Bearer $TOKEN"

# Get
curl "$BASE_URL/[resource]/$ID" -H "Authorization: Bearer $TOKEN"
```

---

## Error Codes

| Code | Status | Meaning |
|------|--------|---------|
| `[FEATURE]_NOT_FOUND` | 404 | Resource missing or out of scope for the tenant |
| `[FEATURE]_INVALID_STATE` | 409 | Operation blocked by current status |
| `VALIDATION_ERROR` | 400 | Request did not match schema |

---

## Open Questions / TODO

- [ ] [Pending design decision]
- [ ] [Pending implementation]

---

**Last Updated**: [YYYY-MM]
