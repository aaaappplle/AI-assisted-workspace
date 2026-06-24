# Documentation index

This directory is the source of truth for product and technical decisions for the AI-assisted team workspace.

## Recommended reading order

An engineer or coding agent starting without prior context should read:

1. [AI working agreement](./ai-working-agreement.md)
2. [Product vision](./product/vision.md)
3. [MVP scope](./product/scope.md)
4. [Requirements](./product/requirements.md)
5. [Roles and permissions](./product/roles-and-permissions.md)
6. [System architecture](./technical/architecture.md)
7. The relevant frontend, backend, database, and testing documents
8. The task brief being implemented
9. ADRs referenced by that task

Do not read every task or ADR by default. Load the smallest relevant context set.

## Directory map

```text
docs/
├── index.md
├── ai-working-agreement.md
├── product/
│   ├── vision.md
│   ├── requirements.md
│   ├── user-stories.md
│   ├── roles-and-permissions.md
│   ├── scope.md
│   └── user-experience.md
├── technical/
│   ├── architecture.md
│   ├── frontend.md
│   ├── backend.md
│   ├── database.md
│   ├── data-models.md
│   └── testing-and-observability.md
├── delivery/
│   └── roadmap.md
├── tasks/
│   ├── index.md
│   ├── task-001-foundation.md
│   ├── task-002-auth-and-tenancy.md
│   ├── task-003-projects-and-tasks.md
│   ├── task-004-collaboration-and-search.md
│   ├── task-005-dashboard.md
│   ├── task-006-ai-summaries.md
│   └── task-007-production-hardening.md
└── decisions/
    ├── index.md
    ├── adr-001-server-first-nextjs.md
    ├── adr-002-postgresql-multi-tenancy.md
    ├── adr-003-task-ownership-and-workflow.md
    ├── adr-004-client-state-ownership.md
    └── adr-005-read-only-ai-summaries.md
```

## Sources of truth

| Question | Owner document |
| --- | --- |
| Why does the product exist? | [Product vision](./product/vision.md) |
| What must the system do? | [Requirements](./product/requirements.md) |
| What is in the MVP? | [MVP scope](./product/scope.md) |
| Who may do what? | [Roles and permissions](./product/roles-and-permissions.md) |
| What should each workflow feel like? | [User stories](./product/user-stories.md) and [user experience](./product/user-experience.md) |
| How is the system divided? | [System architecture](./technical/architecture.md) |
| How is data stored? | [Database](./technical/database.md) and [data models](./technical/data-models.md) |
| How should frontend state/rendering work? | [Frontend architecture](./technical/frontend.md) |
| How should writes/APIs/background work behave? | [Backend architecture](./technical/backend.md) |
| How is quality verified? | [Testing and observability](./technical/testing-and-observability.md) |
| What is the delivery sequence? | [Roadmap](./delivery/roadmap.md) |
| What should be implemented now? | [Task index](./tasks/index.md) and the selected task brief |
| Why was a durable choice made? | [ADR index](./decisions/index.md) |

## Conflict precedence

When documents disagree, use this order:

1. Accepted ADR for the specific decision.
2. Product requirements and roles/permissions.
3. MVP scope.
4. Technical architecture documents.
5. Active task brief.
6. Roadmap.

This order does not permit a task to silently override a requirement. If a lower document discovers a necessary change, update the owning document and add or supersede an ADR before implementation.
