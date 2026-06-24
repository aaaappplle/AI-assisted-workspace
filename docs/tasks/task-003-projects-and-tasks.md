# TASK-003 — Projects and tasks

| Field | Value |
| --- | --- |
| Status | Proposed |
| Milestone | 2 |
| Depends on | TASK-002 |
| Owner | Unassigned |

## Goal

Deliver the authoritative project and task workflow, including ownership, status history, labels, archival, personal work, and stale-write protection.

## Required context

- [Requirements](../product/requirements.md)
- [User stories US-010 through US-015](../product/user-stories.md)
- [Roles and permissions](../product/roles-and-permissions.md)
- [User experience](../product/user-experience.md)
- [Database](../technical/database.md)
- [Data models](../technical/data-models.md)
- [ADR-003](../decisions/adr-003-task-ownership-and-workflow.md)

## In scope

- Project and task schema, repositories, services, actions, and view models.
- Transactional project key/task sequence allocation.
- Fixed lifecycle/status/priority rules.
- Create/edit/assign/transition/archive/restore flows.
- Labels, task status history, meaningful activity, and optimistic version checks.
- Project list/detail, task list/detail, and My Tasks.

## Out of scope

- Comments, attachments, global search, dashboard charts, AI, bulk editing, custom workflows, and subtasks.

## Acceptance criteria

- Authorized user can create a project and uniquely keyed tasks under concurrent requests.
- A task has at most one active-workspace assignee.
- Member relationship restrictions match the permission matrix at UI and server boundaries.
- Status transitions update completion fields and append history atomically.
- Reopening preserves historical completion events.
- Stale version returns a resolvable conflict and does not overwrite current state.
- Archived project becomes read-only until restored.
- My Tasks shows only current user's active assignments in the active workspace.

## Verification

- Workflow/capability unit tests, transaction/concurrency integration tests, stale-edit test, tenant tests, and Playwright task completion journey.
