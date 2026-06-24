# User experience and page structures

## Public and authentication routes

| Route | Structure |
| --- | --- |
| `/` | Product proposition, demo CTA, feature summary, architecture highlights, and sign-in link. |
| `/sign-in` | OAuth/email options, validation, authentication errors, and legal links. |
| `/invite/[token]` | Workspace and proposed role, auth handoff, accept/reject, expired, revoked, and already-used states. |

## Application shell

The authenticated shell contains:

- Workspace switcher.
- Dashboard, My Tasks, Projects, All Tasks, and Team navigation.
- Global task-search/command trigger.
- Create-task action.
- Notification center.
- User menu.
- Breadcrumbs for project and task depth.

Desktop uses a sidebar; mobile uses a sheet. Navigation and content remain keyboard accessible.

## Authenticated routes

| Route | Structure |
| --- | --- |
| `/app` | Server redirect to last accessible workspace or onboarding. |
| `/app/onboarding` | Profile basics, workspace creation or pending invitation, optional demo data. |
| `/app/[workspaceSlug]` | KPI cards, trend/distribution charts, at-risk projects, activity, and metric drill-downs. |
| `/app/[workspaceSlug]/my-tasks` | Assigned work with status, priority, due-date filters and overdue/due-soon groups. |
| `/app/[workspaceSlug]/projects` | Project search/status filters, cards or table, create action, progress and date indicators. |
| `/app/[workspaceSlug]/projects/new` | Focused project form and unsaved-change protection. |
| `/app/[workspaceSlug]/projects/[projectId]` | Project metadata, deterministic progress, task counts, risk signals, recent activity, latest AI summary. |
| `/app/[workspaceSlug]/projects/[projectId]/tasks` | Project list/board, filters, sorting, and create action. |
| `/app/[workspaceSlug]/projects/[projectId]/tasks/[taskId]` | Task fields, description, attachments, comments, and activity. |
| `/app/[workspaceSlug]/projects/[projectId]/summaries` | AI summary history, generation state, cutoff, citations, regeneration, and failure states. |
| `/app/[workspaceSlug]/tasks` | Cross-project task table with URL-backed search/filter/sort and cursor pagination. |
| `/app/[workspaceSlug]/team` | Member directory, roles, workload counts, and authorized management actions. |
| `/app/[workspaceSlug]/notifications` | Unread/all feed, resource links, individual/bulk mark-read. |
| `/app/[workspaceSlug]/activity` | Paginated workspace activity for permitted users. |
| `/app/[workspaceSlug]/settings/general` | Name, slug, timezone, working week, and authorized destructive controls. |
| `/app/[workspaceSlug]/settings/members` | Members, roles, pending invitations, and invitation dialog. |
| `/app/[workspaceSlug]/settings/labels` | Workspace label management. |
| `/app/[workspaceSlug]/settings/ai` | AI allowance, usage, service status, and admin limits. |
| `/app/[workspaceSlug]/settings/audit` | Admin-only audit table and filters. |
| `/app/profile` | Profile, timezone, and personal notification preferences. |

## Dashboard composition

1. Date-range selector and visible data cutoff/timezone.
2. KPI cards: active, completed, overdue, unassigned, and due soon.
3. Completion trend with accessible tabular alternative.
4. Status, priority, project, and assignee distributions.
5. At-risk project list with deterministic reasons.
6. Recent activity.
7. Every metric and chart segment links to a task list with matching URL filters.

## Task list composition

1. Header with title, result count, preset state, and create action.
2. Search plus filters for project, assignee, status, priority, label, creator, and due date.
3. Active filter chips, clear action, sort, and list/board toggle.
4. Table rows containing key, title, project, status, priority, assignee, due date, and update time.
5. Board columns grouped by status, with drag and explicit accessible status controls.
6. Cursor pagination or load-more behavior with scroll/focus preservation.
7. Empty states that distinguish no tasks from no filter matches.

## Task detail composition

1. Task key, title, archive indicator, and authorized actions.
2. Status, priority, assignee, due date, labels, creator, and timestamps.
3. Sanitized Markdown description.
4. Attachment list with upload, processing, error, and download states.
5. Comment composer with member mentions.
6. Chronological comments with ownership-aware actions.
7. Activity timeline for meaningful changes.
8. Stale-update conflict UI that offers refresh and preserves the user's attempted text where safe.

## Required page states

Every relevant route specifies:

- Loading and streaming skeleton.
- True empty and filter-empty states.
- Recoverable and terminal error states with request ID where applicable.
- Not found and non-leaking forbidden behavior.
- Archived/read-only mode.
- Stale-write conflict state.
- AI unavailable, rate-limited, and budget-exhausted states without disabling core work.
- Responsive layout from 360 px.
- Keyboard navigation, focus restoration, and screen-reader announcements for asynchronous changes.
