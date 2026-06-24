# ADR-003 — Single task owner and fixed workflow

| Field | Value |
| --- | --- |
| Status | Accepted |
| Date | 2026-06-23 |
| Owners | Product, architecture |

## Context

The product must make ownership and progress immediately understandable. Multiple assignees and custom workflows introduce ambiguous accountability, complex filtering, configurable transition logic, and a much larger authorization/testing surface.

## Decision

- A task has zero or one primary assignee.
- The assignee must be an active workspace member.
- Statuses are fixed: `Backlog`, `To do`, `In progress`, `In review`, `Done`, and `Cancelled`.
- Backward movement among active statuses is allowed and recorded.
- `Done` or `Cancelled` reopens through `To do`.
- Archival is separate from workflow state.
- Every status change creates an append-only status event.

## Consequences

Positive:

- Clear accountability and simple personal workload queries.
- Predictable board, dashboard, and completion behavior.
- Smaller permission and test matrix.
- Historical completion trends remain explainable.

Costs:

- Shared work needs comments rather than multiple accountable assignees.
- Teams cannot customize terminology or transitions.
- Future multiple-assignee support requires a schema and product migration, not a small toggle.

## Alternatives considered

- Multiple assignees: deferred because accountability and dashboard meaning become ambiguous.
- Configurable workflows: deferred because it multiplies transition, UI, migration, and analytics complexity.
- No status history: rejected because current state alone cannot support reliable completion trends or auditability.
