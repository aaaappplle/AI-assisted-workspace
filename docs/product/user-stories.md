# User stories

User stories describe outcomes, not implementation. Detailed permissions remain owned by [roles and permissions](./roles-and-permissions.md).

## Identity and workspace

### US-001 — Sign in

As a user, I want to sign in securely so that I can access only my workspaces.

Acceptance notes:

- Successful authentication returns the user to the intended safe application route.
- Expired sessions require reauthentication without leaking workspace data.
- Authentication failure is recoverable and does not expose provider internals.

### US-002 — Create or join a workspace

As a new user, I want to create a workspace or accept an invitation so that I can start collaborating with the correct role.

### US-003 — Manage team access

As an Owner or Admin, I want to invite members and manage permitted roles so that workspace access reflects team responsibilities.

## Projects and tasks

### US-010 — Create a project

As a Project Manager, I want to define a project with dates and lifecycle state so that related work has a clear container and target.

### US-011 — Create and assign a task

As a team member, I want to create a task and select one accountable assignee so that ownership is explicit.

Acceptance notes:

- The displayed task key is unique within the project.
- The assignee is an active workspace member or the task remains unassigned.
- Members may choose initial assignment but cannot later reassign arbitrary tasks.

### US-012 — Update assigned work

As an assignee, I want to update task details and status so that the team sees current progress.

Acceptance notes:

- A stale update cannot overwrite a newer version.
- Meaningful changes appear in activity.
- Completion and reopening are recorded historically.

### US-013 — Find my work

As a contributor, I want an `Assigned to me` view with due and priority filters so that I can focus on the right tasks.

### US-014 — Organize work

As a Project Manager, I want list and board views with combined search and filters so that I can inspect and manage project flow.

### US-015 — Archive completed or obsolete work

As a Project Manager, I want to archive and restore projects or tasks so that active views stay useful without destroying history.

## Collaboration

### US-020 — Discuss a task

As a Member, I want to comment on a task and mention a teammate so that decisions and blockers remain attached to the work.

### US-021 — Receive relevant notifications

As a team member, I want in-app notifications for mentions and assignment changes so that I do not miss work requiring my attention.

### US-022 — Attach supporting files

As a Member, I want to attach a validated private file to a task so that supporting material is available to authorized teammates.

## Reporting

### US-030 — View workspace health

As a manager, I want dashboard statistics for active, completed, overdue, due-soon, and unassigned tasks so that I can identify where attention is needed.

Acceptance notes:

- Each value uses the documented workspace timezone and cutoff.
- Selecting a metric opens a filtered task list with the same records.

### US-031 — Understand project risk

As a Project Manager, I want deterministic at-risk indicators so that I can investigate missed dates, overdue work, or inactivity.

## AI assistance

### US-040 — Generate a progress summary

As a team member, I want the AI assistant to summarize project progress so that I can communicate status quickly.

Acceptance notes:

- Summary covers completed, active, overdue, and risky work when present.
- Concrete claims cite task keys accessible to the requester.
- Source cutoff and inference warning are visible.
- AI cannot mutate project data.

### US-041 — Recover summary generation

As a user, I want refresh and provider failure to preserve a durable summary request so that I can retry or continue without duplicate work.

## Administration and read-only access

### US-050 — Follow progress as a Viewer

As a Viewer, I want to inspect dashboards, projects, tasks, comments, and summaries without edit access so that I can remain informed safely.

### US-051 — Investigate administrative changes

As an Owner or Admin, I want an audit log so that I can review role, membership, security, and AI-limit changes.
