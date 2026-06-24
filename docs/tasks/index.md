# Task index

Task briefs turn the roadmap into implementation-sized, verifiable work. They are delivery records, not sources of product truth.

## Status values

- `Proposed`: scope exists but may still depend on an unresolved ADR.
- `Ready`: definition of ready is satisfied.
- `In progress`: active implementation.
- `Blocked`: named dependency prevents progress.
- `Done`: acceptance criteria and definition of done pass.

## Tasks

| ID | Task | Status | Depends on |
| --- | --- | --- | --- |
| TASK-001 | [Foundation](./task-001-foundation.md) | Ready | None |
| TASK-002 | [Authentication and tenancy](./task-002-auth-and-tenancy.md) | Proposed | TASK-001 |
| TASK-003 | [Projects and tasks](./task-003-projects-and-tasks.md) | Proposed | TASK-002 |
| TASK-004 | [Collaboration and search](./task-004-collaboration-and-search.md) | Proposed | TASK-003 |
| TASK-005 | [Dashboard](./task-005-dashboard.md) | Proposed | TASK-003 |
| TASK-006 | [AI summaries](./task-006-ai-summaries.md) | Proposed | TASK-003, TASK-005 |
| TASK-007 | [Production hardening](./task-007-production-hardening.md) | Proposed | TASK-001 through TASK-006 |

TASK-004 and TASK-005 may proceed in parallel after TASK-003. A task should be split into smaller child tasks during planning if one implementation change would be too large to review safely.

## Task template

Each task must contain:

- Metadata: stable ID, status, owner, dependencies, milestone.
- Goal and user outcome.
- Required context and ADR links.
- In scope and explicitly out of scope.
- Acceptance criteria.
- Permission, data, accessibility, observability, and test notes.
- Completion evidence when marked Done.
