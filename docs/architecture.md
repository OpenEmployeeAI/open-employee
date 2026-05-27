# OpenEmployee Architecture

OpenEmployee is a portable, durable runtime for AI employees.

## Core invariant

Every MCP tool selected by an LLM must execute as its own Temporal Activity boundary named:

```text
mcp__<server>__<tool>
```

Workflow logic may choose, validate, and schedule Activities, but it must not execute external tools directly.

## Anchors

- `temporal-community/temporal-ai-agent` is the upstream base.
- PR #61 introduced `mcp_tool_dispatcher`, typed MCP invocation/result/error models, deterministic Activity names, retries, and structured error handling.
- FoundationAgents/OpenManus is the future computer/browser execution layer.
- Pipedream is the first-class SaaS connector layer.

## Identity spine

Portable identity fields:

- `org_id`
- `employee_id`
- `actor_user_id`
- `auth_ref`
- optional `spiffe_id`

Raw secrets must never enter Temporal workflow history.

## Policy and approval

Risky tool calls must pass policy checks and any required human approval before scheduling the Temporal Activity.

## Claim-check outputs

Large Activity outputs should be stored outside workflow history and represented by artifact references.
