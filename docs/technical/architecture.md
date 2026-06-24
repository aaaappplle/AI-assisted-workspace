# System architecture

## Context

The system is a multi-tenant work-management web application with optional AI-generated project summaries. Next.js acts as both the web frontend and backend-for-frontend. PostgreSQL is the system of record.

## Container view

```text
Browser
  │ HTTPS
  ▼
Next.js application
  ├── Server Components: authorized reads and initial rendering
  ├── Server Actions: form-oriented mutations
  ├── Route Handlers: search, uploads, AI streams, webhooks, health
  └── Capability/domain services
       │
       ├── PostgreSQL: durable tenant data
       ├── Private object storage: attachments
       ├── Durable queue/worker: email, processing, AI work
       ├── Auth provider: identity/session verification
       ├── AI provider through gateway: summaries only
       └── Sentry/structured logs: safe telemetry
```

## Architectural boundaries

| Boundary | Owns | Must not own |
| --- | --- | --- |
| Product/domain | Workflow, capability, metric, and validation rules | Framework request objects or provider SDK details. |
| Data access | Tenant-scoped queries, transactions, persistence mapping | UI serialization or role-specific presentation. |
| Server application | Authentication, orchestration, actions, routes, revalidation | Browser-only state. |
| UI | Accessible rendering, interaction, and view-model consumption | Authorization truth or direct database/provider access. |
| Provider adapters | Auth, AI, storage, email, queue normalization | Product rules. |
| Observability | Safe errors, traces, metrics, and correlation | Raw workspace content or secrets. |

## Request paths

### Read path

1. Route authenticates and resolves workspace membership.
2. Server Component calls a tenant-scoped query/service.
3. Domain data is mapped to a minimal view model.
4. Server renders the initial page.
5. Interactive islands use TanStack Query only when live refresh or pagination is needed.

### Mutation path

1. Server Action or Route Handler authenticates and validates input.
2. Capability service authorizes role and resource relationship.
3. Domain service validates workflow and expected version.
4. Transaction persists state plus activity/audit data.
5. Durable follow-up work is queued after commit.
6. Affected route/query data is narrowly invalidated.

### AI summary path

1. Authorized request creates an idempotent queued summary record.
2. Worker builds a bounded project snapshot and source fingerprint.
3. AI gateway calls the provider with no mutation tools.
4. Structured output and citations are validated and persisted.
5. Browser receives progress through SSE and reconciles with durable state.

## Cross-cutting requirements

- Workspace ID is present on every tenant-owned database row and cache/query key.
- Authorization occurs on every server boundary, not only in layouts.
- Background work is durable; no critical operation relies on post-response in-process execution.
- Time is stored in UTC and interpreted using explicit workspace/user timezone rules.
- Public and authenticated caching are isolated.
- AI provider failure never blocks project or task operations.
- Sensitive content is excluded from logs and Sentry.

## Detailed references

- [Frontend architecture](./frontend.md)
- [Backend architecture](./backend.md)
- [Database design](./database.md)
- [Application data models](./data-models.md)
- [Testing and observability](./testing-and-observability.md)
- [Decision records](../decisions/index.md)
