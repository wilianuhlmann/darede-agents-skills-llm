# AgentCore (AI Agent) Instructions for AI Agent

> Use this file for projects that build AI agents / runtimes (e.g. AWS Bedrock AgentCore, Strands, LangGraph, custom MCP servers). Adjust language and stack to your project.

## Reference Documentation

- [Framework docs link, e.g. https://strandsagents.com/]
- [Runtime / hosting docs link, e.g. AWS Bedrock AgentCore]
- [MCP / tooling spec link]
- [LLM provider docs link]

## Overview

**[PROJECT_NAME] AI Assistant** is implemented as a [Strands / LangGraph / custom] agent with:

- **Multi-tenant isolation** — `[tenant_id]` propagated from JWT → Backend → Runtime → Tools
- **Memory** — short-term (in-session) + long-term (cross-session) keyed by `actor_id = [tenant]:[user]`
- **MCP Gateway** — exposes [Lambda / HTTP / local] tools to the agent via the MCP protocol
- **Knowledge base / RAG** — [vector store] with metadata filter on `[tenant_id]`

---

## Tools (via MCP Gateway)

### Read tools

- **[ToolGroup1]** — `[tool_name_1]`, `[tool_name_2]`
- **[ToolGroup2]** — `[tool_name_3]`

### Write tools

- **[ToolGroup1]** — `[tool_name_4]`, `[tool_name_5]`

> Every tool receives `[tenant_id]` as a required parameter and uses it as the partition key in storage and as the metadata filter in the knowledge base.

---

## Documentation (CRITICAL)

### Before implementing any feature, read:

- `.ai/instruction.md` — this file
- `.ai/architecture.md` — runtime, gateway, memory, KB topology
- `.ai/contract.md` — payload contracts (Backend ↔ Runtime, Gateway ↔ Lambda)
- `.ai/features/<feature>.md` — feature-specific docs

### After implementing any feature, update:

| File | What to Update |
|------|----------------|
| `.ai/contract.md` | New tools, new payload fields, schema changes |
| `.ai/architecture.md` | New components, deploy steps, data flow changes |
| `.ai/features/<name>.md` | Create/update the feature doc |
| `[/agentcore/README.md]` | Major workflow changes |

---

## Infrastructure

| Component | How it is created | Source |
|-----------|-------------------|--------|
| IAM roles + tool Lambdas | `python -m src.environment.infrastructure` | `src/environment/infrastructure.py` |
| MCP Gateway + targets | `python -m src.environment.gateway` | `src/environment/gateway.py` |
| Memory resource | [AWS CLI / Terraform / SDK] | `[location]` |
| Runtime container | `python -m src.environment.deploy` | `src/environment/deploy.py` |
| Auth (M2M) | [managed by backend / self-managed] | `[location]` |

### Idempotency rules

- Re-running `gateway` does **not** create a duplicate gateway; it only refreshes targets and IAM permissions.
- Re-running `infrastructure` updates existing resources in place.
- `update_tools` performs `update_function_code` + re-registers MCP targets without rebuilding IAM/Gateway.

---

## Deploy Sequence

1. `[backend deploy]` — produces auth client (M2M) + ingress URL
2. Fill `[CLIENT_ID]`, `[CLIENT_SECRET]`, `[DOMAIN]` in `agentcore/.env`
3. `python -m src.environment.infrastructure` — IAM + tool Lambdas
4. `python -m src.environment.gateway` — MCP Gateway + targets
5. Fill `MCP_GATEWAY_URL` in `agentcore/.env`
6. Create Memory resource → fill `MEMORY_ID`
7. `python -m src.environment.deploy` — runtime container
8. Copy runtime ARN → `[BACKEND_VAR]` in backend `.env`
9. Re-deploy backend so it can invoke the runtime

---

## Backend ↔ Runtime Contract

The backend invokes the runtime via SDK (SigV4), not raw HTTP:

```python
client = boto3.client("[runtime-service]")
response = client.invoke_agent_runtime(
    agentRuntimeArn=RUNTIME_ARN,
    qualifier="DEFAULT",
    runtimeSessionId=session_id,  # >= 33 chars (UUIDv4 is fine)
    payload=json.dumps(payload).encode("utf-8"),
)
```

Payload shape:

```json
{
  "prompt": "user message",
  "[tenant_id]": "uuid",
  "user_id": "uuid",
  "user_name": "Display Name",
  "session_id": "uuid (optional)"
}
```

> **Response encoding:** the runtime may return text wrapped as a JSON-encoded string. Detect and unwrap one layer in the backend; normalize newlines on the frontend as a safeguard.

---

## Tool Lambda Response Contract

Every tool Lambda returns:

- `statusCode` (int): `200` on success, `4xx`/`5xx` on error
- `body` (string): JSON-serialized object; on error `{ "error": "...", "detail": "..." }`

The Gateway forwards `body` to the agent verbatim.

### Required env vars per Lambda

| Lambda | Env vars |
|--------|----------|
| `[name1]` | `[VAR1]`, `[VAR2]` |
| `[name2]` | `[VAR3]` |

> Do not set `AWS_REGION` (reserved by Lambda).

---

## Memory

- **Strategies**: `SEMANTIC` (facts), `USER_PREFERENCE` (preferences)
- **`actor_id`** = `[tenant_id]:[user_id]` — guarantees per-user, per-tenant isolation
- **Session lifecycle**: short-term events flushed at session end; long-term extractions persist

---

## Troubleshooting

| Symptom | Likely cause | Fix |
|---------|--------------|-----|
| `401 Unauthorized` from Gateway | Cognito/JWT config mismatch with backend | Verify `cognito_config.json` points to the same User Pool |
| `cannot be assumed by Lambda` (transient) | Eventual consistency after `update_function_code` | Retry; the script auto-retries |
| `cannot be assumed by Lambda` (persistent) | Trust policy broken | Recreate the role via `infrastructure` |
| Tool returns `{"error": ...}` | Validation or env-var problem in the Lambda | Tail CloudWatch logs for the specific Lambda |

---

**Last Updated**: [YYYY-MM]
