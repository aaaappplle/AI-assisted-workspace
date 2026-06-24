# MVP scope

## Product decisions

- A task has zero or one primary assignee. One accountable owner is clearer than multiple assignees.
- All active workspace members can see all workspace projects.
- Task statuses and priorities are fixed rather than configurable.
- Project state and task state are independent.
- Task deletion is archival in normal use.
- The AI assistant produces read-only project summaries and has no mutation tools.
- Normal work management remains operational when AI is unavailable.

## Included

- OAuth or email authentication, secure sessions, onboarding, and workspace switching.
- Workspace invitations and Owner, Admin, Project Manager, Member, and Viewer roles.
- Project create, edit, state-change, archive, and restore flows.
- Task list, status board, detail, create, edit, assignment, transition, archive, and restore flows.
- One primary assignee, fixed statuses/priorities, due dates, workspace labels, and optimistic concurrency.
- Comments, mentions, in-app notifications, private attachments, and activity history.
- Search, combined filters, sorting, cursor pagination, URL-backed views, and personal task presets.
- Dashboard KPIs, distributions, completion trend, at-risk projects, activity, and drill-down links.
- On-demand AI project summaries with task citations, source cutoff, durable status, and history.
- Responsive accessible UI with loading, empty, error, forbidden, offline/degraded, and stale-conflict states.
- Sentry, structured logs, CI quality gates, Vitest suites, and Playwright critical journeys.

## Excluded

- Custom workflows, custom fields, subtasks, dependencies, recurring tasks, estimates, time tracking, sprints, and Gantt charts.
- Multiple assignees, watchers, team assignment, and workload capacity planning.
- Private projects, per-project roles, field permissions, public links, and external guests.
- Real-time collaborative editing, presence, chat, and WebSocket-driven live updates.
- Email or push notifications beyond invitation email.
- AI task mutations, autonomous agents, semantic/vector search, and predictive scheduling.
- Jira, GitHub, Slack, or calendar integrations; bulk import/export.
- Billing, marketplace, localization, offline-first mode, and native mobile applications.
- Custom dashboard/report builders and permanently materialized analytics.

## Success criteria

- A new manager can create a workspace, project, task, assignment, and deadline without guidance.
- An assignee can find `Assigned to me`, update status, comment, and complete the task.
- Dashboard cards reconcile exactly with their linked filtered task lists at the same cutoff and timezone.
- A project summary covers completed, active, overdue, and risky work and cites task keys for concrete claims.
- A Member cannot administer projects or arbitrary tasks; a Viewer cannot mutate through UI or direct request.
- Concurrent task edits do not silently overwrite each other.
- Refresh during AI generation recovers the same durable summary rather than creating a duplicate.
- Critical end-to-end tests pass in CI, and unexpected production failures include a supportable request ID.

## MVP risks and controls

| Risk | Control |
| --- | --- |
| Permissions become inconsistent | Central capability service plus matrix tests at every server boundary. |
| Dashboard numbers disagree with lists | Shared query predicates, explicit cutoff/timezone, and reconciliation tests. |
| Board drag-and-drop harms accessibility | Keyboard controls and explicit status menu are first-class alternatives. |
| AI invents progress | Immutable source snapshot, task citations, fact/inference labeling, and read-only behavior. |
| Search infrastructure is overbuilt | Start with PostgreSQL full-text/trigram search and measure before extraction. |
| Feature breadth hides the primary journey | Milestones deliver identity → tasks → collaboration → reporting → AI in that order. |
