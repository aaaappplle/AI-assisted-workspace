# Frontend architecture

## Server Components by default

Use Server Components for:

- Authenticated layouts and route-level authorization.
- Initial project, task, dashboard, team, settings, and AI-summary reads.
- Metadata and canonical navigation.
- Initial not-found, forbidden, empty, and error decisions.
- Read-heavy tables and detail shells.

Benefits are smaller browser bundles, secrets remaining server-only, and data access next to the authorization boundary.

Fetch independent data in parallel. Add Suspense boundaries around meaningful slower regions such as dashboard charts or activity, not around every small component. Avoid nested request waterfalls.

Server Components pass small serializable view models. They do not pass ORM entities, repositories, authorization services, AI clients, or large workspace graphs.

## Client Component boundaries

Use Client Components only where browser interactivity requires them:

- Project/task forms with live validation and unsaved-change protection.
- Search, filters, comboboxes, list/board switching, and accessible board interactions.
- Inline status, priority, assignee, due date, and label controls.
- Comment composer, mentions, optimistic comment display, and upload progress.
- Menus, dialogs, sheets, tabs, toasts, and notification popover.
- Dashboard chart rendering with an accessible table or text alternative.
- AI progress/stream display and retry controls.
- TanStack Query and Zustand consumers.

Keep boundaries low. A task page remains server-rendered even when field controls and comments update independently.

## State ownership

| State | Owner | Examples |
| --- | --- | --- |
| Shareable navigation | URL segments/search params | Query, filters, sort, view, dashboard range, cursor. |
| Durable product state | PostgreSQL | Projects, tasks, roles, comments, notifications, metrics inputs, AI summaries. |
| Initial rendered server state | Server Components | Page view models and authorization-aware shells. |
| Live remote state | TanStack Query | Inline task refresh, comments, notifications, search pages, AI progress. |
| Cross-component ephemeral UI | Browser-created Zustand store | Sidebar, command palette, detail-panel width, draft filter drawer. |
| Local interaction | React state | Open dialog, focus, temporary selection, field editing mode. |

Zustand must not store canonical tasks, permissions, comments, dashboard values, or AI summaries. It is created intentionally for the browser and is never a global server singleton.

TanStack Query is used only when post-hydration freshness, pagination, polling, or optimistic mutation adds value. Query keys always start with workspace identity and include normalized resource identifiers and filters.

## Interaction rules

- Status and priority updates may be optimistic because rollback is obvious.
- Assignment, archival, project state, role changes, and destructive operations wait for server confirmation.
- New comments may appear optimistically using a client idempotency key, then reconcile to the server record.
- URL filters are updated without losing keyboard focus or scroll unnecessarily.
- Board movement has a menu/keyboard path equal to drag-and-drop.
- Revalidation targets only affected task, project, list, dashboard, activity, and notification data.
- Dashboard freshness is explicit; the MVP does not promise real-time analytics.

## Caching

- Authenticated workspace routes are dynamic and never publicly cached.
- Public marketing pages may use static rendering and controlled revalidation.
- Personalized responses must not be cached across users or workspaces.
- Route and TanStack Query invalidation use workspace-scoped keys/tags.
- Search and dashboard responses expose their generation time.
- AI summaries remain immutable historical snapshots even after project data changes.
