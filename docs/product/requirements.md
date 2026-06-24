# Product and production requirements

## Product definition

The workspace helps a team plan and manage work. Users can authenticate, join a workspace, view projects, create and assign tasks, track task status, collaborate through comments, search and filter work, view operational statistics, and ask an AI assistant to summarize project progress.

The core journey is:

1. A user signs in and creates or joins a workspace.
2. A manager creates a project.
3. A member creates a task with priority, due date, labels, and one accountable assignee.
4. The assignee advances the task through its workflow and discusses it in comments.
5. The team finds work through project views, personal views, search, filters, and a status board.
6. The dashboard reports workload, completion, overdue work, and project risk.
7. A user generates a project summary grounded in current task data.

## Functional requirements

### Identity and onboarding

- Sign in using OAuth or email magic link and securely sign out.
- Create a workspace or accept, reject, or inspect an invitation.
- Switch between accessible workspaces without cross-tenant data leakage.
- Update profile name, avatar, timezone, and notification preferences.
- Recover cleanly from expired sessions and invalid or already-used invitations.

### Workspace and team management

- View members, their roles, and pending invitations.
- Invite, resend, revoke, change permitted roles, and remove members according to capability.
- Configure workspace name, slug, timezone, and working week.
- Require ownership transfer before the final Owner leaves.
- Expose administrative and security changes through an audit log.

### Projects

- List, search, and filter projects by lifecycle state.
- Create and edit project key, name, description, start date, and target date.
- Move projects through `Planned`, `Active`, `On hold`, `Completed`, and `Archived`.
- Show deterministic progress, task counts, overdue tasks, active assignees, recent activity, and latest AI summary.
- Archive and restore projects; normal product flows do not hard-delete them.

### Tasks

- Create a task in a project with title and optional Markdown description.
- Set one primary assignee, status, priority, due date, and workspace labels.
- Use the fixed statuses `Backlog`, `To do`, `In progress`, `In review`, `Done`, and `Cancelled`.
- Use the priorities `Low`, `Medium`, `High`, and `Urgent`.
- Edit and reassign tasks only when authorized by role and relationship to the task.
- Track creator, assignee, dates, comments, attachments, and meaningful field changes.
- Detect stale edits and reject them with a resolvable conflict instead of overwriting newer data.
- Archive and restore tasks while preserving their history.
- Reject assignment to users who are not active workspace members.

### Collaboration

- Add, edit, and delete one's own comments.
- Allow privileged roles to moderate comments.
- Mention active workspace members and create in-app notifications.
- Attach validated files to tasks using private object storage.
- Display project activity and task-specific activity chronologically.
- Mark notifications read individually or in bulk.

### Search and organization

- Search task key, title, and description within the active workspace.
- Filter by project, assignee, status, priority, label, creator, due-date range, overdue state, or unassigned state.
- Combine filters and sort by updated date, created date, priority, due date, or title.
- Provide `Assigned to me`, `Created by me`, `Overdue`, and `Due soon` personal views.
- Keep shareable filter and sort state in URL search parameters.
- Offer list and status-board views with keyboard-accessible alternatives to drag and drop.

### Dashboard and reporting

- Show active, completed, overdue, unassigned, and due-soon task counts.
- Show task distribution by status, priority, project, and assignee.
- Show completion trend for 7-, 30-, and 90-day periods.
- Identify at-risk projects using deterministic rules.
- Show recent workspace activity.
- Link each metric or chart segment to the corresponding filtered task list.
- Display metric cutoff time and apply the configured workspace timezone.

Metric definitions:

- **Active:** not archived and not `Done` or `Cancelled`.
- **Completed:** first entered `Done` during the selected period.
- **Overdue:** active with a due date before the start of today in the workspace timezone.
- **Due soon:** active and due from today through the next seven calendar days.
- **Unassigned:** active with no primary assignee.
- **Project progress:** done tasks divided by non-cancelled, non-archived tasks. It is not an effort estimate.
- **At risk:** active project with overdue work, a missed target date, or no task activity for a configured period.

### AI assistant

- Generate an on-demand, read-only project progress summary.
- Cover completed work, work in progress, overdue items, potential blockers, risks, and attention areas.
- Cite relevant task keys for concrete claims.
- Show source cutoff time and distinguish stored facts from AI inference.
- Preserve summary history; regeneration creates a new record.
- Remain optional: AI failure or budget exhaustion cannot block normal project and task flows.

## Non-functional requirements

| Area | MVP target |
| --- | --- |
| Availability | 99.9% monthly for core work-management paths. |
| Web performance | p75 LCP below 2.5 seconds on representative mobile hardware. |
| Server performance | p95 non-AI server response below 500 ms. |
| Search | p95 common task search/filter response below 750 ms at expected MVP volume. |
| AI feedback | Request acknowledged below 1 second; progress or first content targets below 3 seconds when healthy. |
| Accessibility | WCAG 2.2 AA, full keyboard flows, visible focus, semantic chart alternatives, and non-drag task movement. |
| Browsers | Current and previous major Chromium, Firefox, and Safari releases; responsive from 360 px. |
| Integrity | Foreign keys, constrained enums, transactions, unique task keys, UTC timestamps, explicit timezones, and recoverable archival. |
| Security | Server-side authorization, secure cookies, CSRF-safe mutations, rate limits, secret isolation, upload validation, and dependency scanning. |
| Privacy | Data minimization, retention/deletion controls, export path, and no provider training on workspace data without explicit opt-in. |
| Recovery | Automated backups with tested restore; initial RPO 24 hours and RTO 4 hours. |
| Observability | Safe errors, traces, releases, source maps, health checks, and request/AI-summary correlation IDs. |

## AI safety requirements

- Treat task text, comments, attachments, and AI output as untrusted.
- Send only authorized records from the selected project and cap task/comment volume.
- Exclude secrets, emails, signed URLs, private attachment content, and unrelated workspace data.
- Record provider, model, prompt version, snapshot cutoff, token use, latency, and sanitized failure category.
- Enforce workspace/user token, request, concurrency, and spend limits.
- Require task citations in the output and label inferred risks.
- Do not expose task mutation tools to the model in the MVP.
- Never send raw task/comment content or model output to Sentry.
- Apply bounded timeouts and transient retries with idempotency.

## Production boundaries

| Concern | Boundary |
| --- | --- |
| Next.js application | UI, server rendering, authorization boundary, Server Actions, and BFF endpoints. |
| PostgreSQL | Source of truth for tenant-scoped relational and searchable product data. |
| Typed data-access layer | Migrations, transactions, tenant-scoped repositories, and query composition. |
| Authentication adapter | Identity, verification, and sessions; application roles remain in PostgreSQL. |
| Private object storage | Attachments accessed with short-lived signed URLs. |
| AI gateway | Provider normalization, grounding, limits, streaming, errors, and usage. |
| Durable queue/worker | Email, notifications, file processing, AI summaries, retries, and cleanup. |
| Sentry and structured logs | Safe errors, traces, releases, alerts, and correlation. |

`local`, `preview`, `staging`, and `production` use isolated databases, buckets, credentials, and Sentry environments. Deployments require controlled, backward-compatible migrations and feature flags for experimental or costly behavior.
