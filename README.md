# AI-assisted Team Workspace

A team work-management application where teams organize projects, create and assign tasks, track progress, discuss work, search and filter tasks, inspect dashboard statistics, and generate grounded AI summaries of project progress.

AI supports the workflow but does not control it. Core project and task management remains fully usable when the AI provider is unavailable.

## Project status

**Architecture and planning.** Product requirements, technical boundaries, delivery tasks, and architecture decisions are documented. Application implementation has not started.

## MVP capabilities

- Secure authentication, workspace onboarding, invitations, and workspace switching.
- Owner, Admin, Project Manager, Member, and Viewer permissions.
- Project lifecycle management.
- Task creation, one primary assignee, status, priority, due dates, and labels.
- Task comments, mentions, notifications, private attachments, and activity history.
- Search, combined filters, personal task views, and accessible list/board layouts.
- Dashboard KPIs, completion trends, project risk indicators, and task drill-downs.
- Read-only AI project summaries with source cutoff and task-key citations.

## Technology stack

- Next.js App Router and TypeScript
- Tailwind CSS and shadcn/ui
- Zustand and TanStack Query
- PostgreSQL with a typed data-access layer
- Vitest and Playwright
- Sentry
- Private S3-compatible object storage
- Provider-neutral authentication and AI adapters

## Architecture at a glance

- Server Components provide authorized initial reads and rendering.
- Client Components are limited to interactive UI boundaries.
- Server Actions handle form-oriented application mutations.
- Route Handlers handle search, uploads, AI streaming, webhooks, and health checks.
- PostgreSQL is the source of truth for tenant-scoped product data.
- URL parameters own shareable filters, TanStack Query owns live remote state, and Zustand owns ephemeral browser UI state.
- AI generates immutable, read-only summaries and has no task mutation tools.

## Documentation

Start with the [documentation index](./docs/index.md).

| Area | Document |
| --- | --- |
| Product direction | [Vision](./docs/product/vision.md) |
| Product behavior | [Requirements](./docs/product/requirements.md) |
| MVP boundaries | [Scope](./docs/product/scope.md) |
| User workflows | [User stories](./docs/product/user-stories.md) |
| Authorization | [Roles and permissions](./docs/product/roles-and-permissions.md) |
| System design | [Technical architecture](./docs/technical/architecture.md) |
| Delivery sequence | [Roadmap](./docs/delivery/roadmap.md) |
| Implementation work | [Task index](./docs/tasks/index.md) |
| Technical decisions | [ADR index](./docs/decisions/index.md) |
| AI collaboration rules | [AI working agreement](./docs/ai-working-agreement.md) |

The documentation uses explicit source-of-truth ownership, stable story/task/decision IDs, testable acceptance criteria, and a defined conflict-precedence order so humans and coding agents can work from the same intent.

## Repository structure

```text
docs/
├── product/      # Vision, requirements, stories, permissions, scope, and UX
├── technical/    # Architecture, frontend, backend, database, and testing
├── delivery/     # Milestone roadmap
├── tasks/        # Implementation-ready task briefs
└── decisions/    # Architecture decision records
```

Setup, development, test, and deployment commands will be added after the application foundation is implemented in `TASK-001`.
