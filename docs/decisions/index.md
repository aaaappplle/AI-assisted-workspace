# Architecture decision records

ADRs capture durable choices and their trade-offs. They do not repeat requirements or implementation steps.

## Status values

- `Proposed`: under discussion and must not be treated as binding.
- `Accepted`: current decision.
- `Deprecated`: retained for history but discouraged.
- `Superseded`: replaced by another ADR, which must be linked.
- `Rejected`: considered and intentionally not selected.

## Records

| ID | Decision | Status | Date |
| --- | --- | --- | --- |
| ADR-001 | [Use a server-first Next.js App Router architecture](./adr-001-server-first-nextjs.md) | Accepted | 2026-06-23 |
| ADR-002 | [Use PostgreSQL with explicit workspace-scoped multi-tenancy](./adr-002-postgresql-multi-tenancy.md) | Accepted | 2026-06-23 |
| ADR-003 | [Use one primary task assignee and a fixed workflow](./adr-003-task-ownership-and-workflow.md) | Accepted | 2026-06-23 |
| ADR-004 | [Assign one owner to each frontend state category](./adr-004-client-state-ownership.md) | Accepted | 2026-06-23 |
| ADR-005 | [Limit AI to grounded read-only project summaries](./adr-005-read-only-ai-summaries.md) | Accepted | 2026-06-23 |

## ADR rules

- Never edit an accepted decision to reverse its meaning.
- Create a new ADR that supersedes the old record.
- Update the index and all affected task/context links.
- Consequences include costs and limitations, not only benefits.
- Provider/library selections should receive separate ADRs when chosen.
