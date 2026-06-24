# TASK-006 — AI project summaries

| Field | Value |
| --- | --- |
| Status | Proposed |
| Milestone | 5 |
| Depends on | TASK-003, TASK-005 |
| Owner | Unassigned |

## Goal

Generate durable, grounded, read-only project summaries that save reporting time while keeping concrete claims verifiable.

## Required context

- [AI requirements and safeguards](../product/requirements.md)
- [User stories US-040 and US-041](../product/user-stories.md)
- [System architecture](../technical/architecture.md)
- [Backend](../technical/backend.md)
- [Testing and observability](../technical/testing-and-observability.md)
- [ADR-005](../decisions/adr-005-read-only-ai-summaries.md)

## In scope

- Authorized bounded project snapshot and source fingerprint.
- Provider gateway, structured output, citation validation, limits, and normalized errors.
- Durable queue/running/completed/failed lifecycle and idempotency.
- SSE progress, reconnect, history, source cutoff, citations, and retry/regenerate UI.
- Safe usage and latency telemetry.

## Out of scope

- Task mutations, autonomous tools, attachment content, semantic retrieval, conversational chat, and multi-provider UI.

## Acceptance criteria

- Requester must have summary-generation capability for the project workspace.
- Snapshot contains only bounded, authorized project data.
- Concrete claims link to valid accessible task keys; inferred risks are labeled.
- Refresh/reconnect resolves to the same durable summary request.
- Regeneration creates history rather than overwriting a terminal summary.
- Rate, concurrency, spend, timeout, and provider failures are recoverable and do not affect task flows.
- Model has no callable mutation path.
- Sentry/logs contain no task, comment, prompt, or output content.

## Verification

- Mock-provider contract tests, snapshot authorization tests, hostile-content tests, citation validation, idempotency/reconnect integration tests, limit/failure tests, and Playwright summary journey.
