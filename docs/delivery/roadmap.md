# Delivery roadmap

## Milestone 0 — Architecture and delivery foundation

Deliver:

- Confirm vocabulary, metric definitions, retention, deployment target, and ADR process.
- Establish strict TypeScript, Tailwind, shadcn/ui conventions, environment validation, CI, and previews.
- Establish server/client module boundaries, test infrastructure, and Sentry releases.

Exit criteria:

- Production build and preview deployment pass.
- Health endpoint works.
- Baseline accessibility check passes.
- A safe test error reaches Sentry with correct release/environment metadata.

## Milestone 1 — Identity, tenancy, roles, and shell

Deliver:

- Authentication adapter and user provisioning.
- Workspace, membership, invitation, and capability models.
- Onboarding, workspace switcher, protected shell, team, and member settings.
- Tenant isolation and role matrix tests.

Exit criteria:

- Owner invites a Member and Viewer.
- Viewer mutation fails at the server.
- Cross-workspace access fails without disclosing resource existence.

## Milestone 2 — Projects and task core

Deliver:

- Project/task schema, status history, labels, archive state, task sequences, and optimistic versions.
- Project list/detail and task list/detail/create/edit/assign/status flows.
- Personal task presets and meaningful activity records.

Exit criteria:

- Member creates a task and an assignee completes it.
- Manager sees accurate project progress.
- Concurrent stale edit cannot overwrite current state.

## Milestone 3 — Collaboration and task discovery

Deliver:

- Comments, mentions, notifications, private attachments, activity views.
- Combined search/filter/sort, cursor pagination, URL state, and accessible board.
- Responsive and keyboard-complete task interactions.

Exit criteria:

- Users can find and discuss work across projects.
- Search, notification, attachment, accessibility, and authorization suites pass.

## Milestone 4 — Dashboard and reporting

Deliver:

- KPI queries, distributions, completion trend, risk flags, recent activity, and drill-down links.
- Explicit cutoff/timezone semantics and reconciliation tests.

Exit criteria:

- Dashboard values exactly match linked filtered lists for seeded edge cases and timezone boundaries.
- Charts have equivalent accessible text or tables.

## Milestone 5 — AI progress summaries

Deliver:

- Snapshot builder, provider gateway, durable lifecycle, structured output/citations, limits, and background execution.
- Summary generation, SSE progress/reconnect, history, failure, and verification UI.

Exit criteria:

- Concrete claims cite accessible tasks.
- Refresh does not duplicate a request.
- Provider failure does not impair task management.
- The model has no mutation path.

## Milestone 6 — Production hardening and interview polish

Deliver:

- Complete isolation, authorization, accessibility, responsive, browser, performance, and operational suites.
- Migration, restore, archival/deletion, rate-limit, queue-recovery, source-map, alert, and redaction verification.
- Reproducible demo workspace, ADRs, demo script, reset procedure, trade-offs, and known limitations.

Exit criteria:

- All CI and staging gates pass.
- Dashboard and AI demo data are reproducible.
- The application resets safely and can be explained from product requirement through implementation boundary.

## Architecture decisions required before implementation

1. Authentication provider and account-linking policy.
2. ORM/query layer and migration workflow.
3. Hosting, PostgreSQL, object storage, email service, and durable queue.
4. AI SDK/provider, model policy, budget, structured output/citation schema, and data-processing terms.
5. Comment editor/Markdown scope, edit policy, mentions, and notification retention.
6. Attachment types, limits, malware scanning, and abandoned-upload cleanup.
7. Dataset assumptions and thresholds for external search or cached analytics.
8. Data export/deletion, audit retention, and deployment region.

## Delivery rationale

Build identity and authorization before task behavior, task behavior before reporting, and trustworthy reporting before AI. The AI summary is credible only when it is grounded in a coherent, tested, and auditable work-management system.
