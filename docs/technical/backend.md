# Backend architecture

## Boundary rules

Every mutation boundary follows this sequence:

1. Authenticate the session.
2. Parse and validate untrusted input.
3. Resolve workspace membership from server data.
4. Load the tenant-scoped resource.
5. Authorize the named capability and relationship rule.
6. Verify expected version or idempotency key when applicable.
7. Execute the transaction.
8. Write activity and/or audit records.
9. Enqueue durable follow-up work after a committed state exists.
10. Revalidate only affected routes/query keys.
11. Return a typed success result or safe error code with request ID.

Server Actions are callable server endpoints, not trusted internal functions. They require the same security checks as Route Handlers.

## Server Actions

Use Server Actions for same-application mutations naturally coupled to forms:

- Create, edit, change state, archive, and restore a project.
- Create, edit, assign, transition, archive, and restore a task.
- Add, edit, or delete a comment.
- Mark one or all notifications read.
- Create, update, or delete a label.
- Invite/remove a member and change an allowed role.
- Update profile, workspace, timezone, working week, or preferences.

Actions return typed domain results. Expected validation, permission, conflict, and limit outcomes are not thrown as unexpected exceptions.

## Route Handlers

Use Route Handlers for client-driven queries and non-form protocols:

| Endpoint | Reason |
| --- | --- |
| `GET /api/workspaces/[id]/tasks` | Debounced search, combined filters, cursor pagination, and board refresh. |
| `GET /api/workspaces/[id]/members/search` | Bounded async mention/assignee combobox. |
| `POST /api/uploads/presign` | Authorize and validate direct private object-storage upload. |
| `POST /api/uploads/complete` | Verify checksum/object metadata and finalize attachment. |
| `POST /api/projects/[id]/summaries` | Create an idempotent durable AI-summary request. |
| `GET /api/projects/[id]/summaries/[summaryId]/events` | Server-Sent Events for progress, heartbeat, sequence, and reconnect. |
| `/api/webhooks/*` | Raw-body signature verification and replay-protected provider callbacks. |
| `GET /api/health` | Safe deployment readiness/liveness response. |

Handlers validate filters, cap page sizes, use opaque cursors, and never trust a requested workspace without verifying membership.

## AI execution

The summary request flow is:

1. Authorize user and project.
2. Enforce AI budget, rate, and concurrency limits.
3. Create a queued `ai_summaries` record using an idempotency key.
4. Build an authorized, bounded, immutable project snapshot in background work.
5. Call the provider through a gateway with timeout and normalized errors.
6. Validate structured output and task citations.
7. Persist terminal result and usage before notifying clients.
8. Stream progress through SSE; reconnecting clients reconcile with durable state.

WebSockets are unnecessary because the MVP needs one-way AI progress, not live collaborative presence. Never depend on an in-process promise continuing after an HTTP response ends.

## Error contract

All server boundaries use stable error categories such as:

- `VALIDATION_FAILED`
- `AUTHENTICATION_REQUIRED`
- `FORBIDDEN`
- `NOT_FOUND`
- `VERSION_CONFLICT`
- `RATE_LIMITED`
- `AI_LIMIT_REACHED`
- `UPLOAD_REJECTED`
- `INTERNAL_ERROR`

Responses expose a safe message, field errors when relevant, and request ID. They never expose stack traces, SQL, object keys, provider errors, or cross-tenant existence.

## Consistency and invalidation

- Project/task writes and their activity/status events share a transaction.
- Member removal and task unassignment share a transaction.
- Mention records and notification enqueueing follow a committed comment.
- Webhooks store provider event IDs for replay deduplication.
- Task mutations invalidate the task, parent project aggregates, matching task lists, dashboard, and activity as narrowly as possible.
- AI snapshot cutoff/fingerprint prevents later task changes from rewriting historical summary meaning.
