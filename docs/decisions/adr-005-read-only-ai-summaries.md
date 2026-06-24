# ADR-005 — Grounded read-only AI summaries

| Field | Value |
| --- | --- |
| Status | Accepted |
| Date | 2026-06-23 |
| Owners | Product, AI architecture |

## Context

AI can reduce the effort of communicating project status, but autonomous task mutation creates safety, authorization, audit, and user-trust risks. Summaries can also hallucinate if source boundaries and evidence are unclear.

## Decision

Limit the MVP AI assistant to on-demand, read-only project summaries.

- Build a bounded authorized snapshot with cutoff and fingerprint.
- Generate structured completed/active/overdue/risk sections.
- Require valid task-key citations for concrete claims.
- Label inferred risks and show the source cutoff.
- Persist each generation as an immutable durable record.
- Expose no task/project mutation tools to the model.
- Enforce budget, rate, concurrency, timeout, and data-redaction controls.

## Consequences

Positive:

- AI value is visible without risking silent work changes.
- Users can verify claims through task links.
- Provider failure remains isolated from core work management.
- Historical summaries remain explainable snapshots.

Costs:

- Citation validation and bounded snapshot construction add engineering work.
- Summaries can become stale immediately after their cutoff and must say so.
- Users cannot ask the assistant to update tasks or hold an open-ended conversation.

## Alternatives considered

- Autonomous task-management agent: rejected for MVP safety and authorization complexity.
- Free-form chatbot over the workspace: rejected because grounding, scope, and predictable evaluation are weaker.
- Ephemeral summaries only: rejected because refresh/retry, auditability, usage accounting, and history require durable state.
