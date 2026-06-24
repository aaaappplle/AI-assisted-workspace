# Testing and observability

## Test pyramid

| Layer | Tool | Scope |
| --- | --- | --- |
| Static | TypeScript and linting | Strict types, imports, server/client boundaries, formatting, and accessibility rules. |
| Unit | Vitest | Capabilities, schemas, workflows, dates/timezones, dashboard predicates, AI source selection, and error mapping. |
| Component | Vitest with DOM testing utilities | Forms, filters, inline controls, comments, mentions, charts, keyboard board movement, and UI states. |
| Data/integration | Vitest with isolated PostgreSQL | Tenant scoping, transactions, constraints, key allocation, concurrency, queries, metrics, idempotency, actions, and handlers. |
| End-to-end | Playwright | Critical journeys against a production build with desktop Chromium and focused mobile/cross-browser coverage. |
| Operational | Staging checks | Sentry, source maps, health, migrations, restore, limits, private attachments, queue recovery, and AI degradation. |

## Critical test suites

### Authorization

- Table-driven role/capability tests.
- Member relationship tests for created-by and assigned-to tasks.
- Archived-project behavior.
- Direct Server Action and Route Handler rejection.
- Owner transfer and last-owner protection.

### Tenant isolation

Workspace B cannot read, mutate, search, mention, assign, download, or summarize a workspace A resource, including guessed identifiers.

### Task lifecycle

- Concurrent task-key allocation.
- Valid and invalid status transitions.
- Stale version conflict.
- Assignment to removed/outside member.
- Done, reopen, and completion timestamps.
- Archive and restore behavior.

### Dashboard reconciliation

- KPI totals equal linked task-list predicates at identical cutoff/timezone.
- Completion trends use status events rather than current status alone.
- Due-today, overdue, no-due-date, archived, cancelled, reopened, and daylight-saving boundaries.

### Search and collaboration

- Combined filters, normalization, cursor stability, archived exclusion, and injection-shaped inputs.
- Comment idempotency, author edits, moderation, mentions, notification deduplication, safe Markdown, and deleted tombstones.
- Upload type/size/checksum rejection, signed URL expiry, cleanup, and cross-tenant denial.

### AI summaries

- Authorized and bounded snapshot.
- Valid task citations and malformed-citation rejection or safe fallback.
- Timeout, provider error, budget/rate limit, reconnect, and duplicate request.
- Prompt-injection-shaped task/comment text.
- No model-accessible mutation capability.

## Playwright MVP journeys

1. New user signs in, creates a workspace, invites a member, and creates a project.
2. Member creates and assigns a task; assignee finds it in My Tasks, comments, changes status, and completes it.
3. Manager filters list/board and verifies dashboard completion and overdue drill-downs.
4. Member generates a cited project summary, refreshes mid-generation, and resumes the same record.
5. Viewer reads dashboard/project/task/comments but UI and direct requests reject every mutation.
6. Two sessions edit one task; the stale mutation cannot overwrite the newer change.

CI mocks the AI adapter deterministically, including delay, failure, malformed structure, and hostile source text. A quarantined staging smoke test may call a real provider but is not a merge gate.

Coverage is risk-based. Authorization, tenancy, transitions, metrics, and AI source boundaries target complete branch coverage. Generated shadcn/ui primitives and trivial visual wrappers are not coverage goals.

## Sentry and telemetry

- Initialize server and browser instrumentation separately; add edge instrumentation only if used.
- Tag events with environment, release, route, safe workspace ID, request ID, and summary ID where relevant.
- Never attach task/comment text, emails, attachment URLs/keys, auth headers, prompts, model output, or secrets.
- Expected validation, permission, conflict, and rate-limit results are metrics or structured logs, not noisy exceptions.
- Trace task creation, task search, dashboard generation, upload finalization, and AI summary generation.
- Alert on non-AI error/latency regression, search latency, conflict spikes, failed jobs, queue age, AI failure/latency/cost, webhook failures, and notification backlog.
- Show users a supportable error/request ID without internal details.

## CI and release gates

- Type-check and lint.
- Unit and component tests.
- Isolated database integration tests and migration validation.
- Production build.
- Critical Playwright journeys.
- Dependency and secret scanning.
- Staging health, Sentry source-map, migration, and rollback/readiness checks before production promotion.
