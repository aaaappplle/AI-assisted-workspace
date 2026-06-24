# ADR-002 — PostgreSQL workspace-scoped multi-tenancy

| Field | Value |
| --- | --- |
| Status | Accepted |
| Date | 2026-06-23 |
| Owners | Architecture, data |

## Context

Projects, tasks, comments, roles, notifications, metrics, and AI summaries are relational and tenant-owned. The system requires transactions, constraints, searchable task text, reproducible dashboard queries, and strong cross-workspace isolation.

## Decision

Use PostgreSQL as the source of truth with a shared schema and explicit `workspace_id` on every tenant-owned row.

- Tenant repositories require workspace context.
- Parent/child workspace identity is validated.
- Foreign keys, unique constraints, and transactions enforce integrity.
- PostgreSQL full-text/trigram search serves the MVP.
- Files stay in private object storage with metadata in PostgreSQL.
- Application authorization is mandatory even if database-level row security is later added as defense in depth.

## Consequences

Positive:

- Clear transactional model for task state, activity, membership, and counters.
- Efficient tenant-scoped indexing and easier isolation testing.
- One data system supports operational reads, search, and initial reporting.

Costs:

- Every query must carry correct workspace scope.
- Shared-schema errors can be high impact, requiring repository conventions and adversarial isolation tests.
- Very large search or analytics workloads may eventually require specialized infrastructure.

## Alternatives considered

- Database per workspace: rejected for MVP operational complexity and migration overhead.
- Document database: rejected because relationships, constraints, and reporting are central.
- External search at launch: rejected until measured need justifies synchronization and operations.
