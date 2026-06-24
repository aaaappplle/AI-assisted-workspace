# TASK-004 — Collaboration and search

| Field | Value |
| --- | --- |
| Status | Proposed |
| Milestone | 3 |
| Depends on | TASK-003 |
| Owner | Unassigned |

## Goal

Help teams discuss, find, and organize work through comments, mentions, notifications, attachments, search, combined filters, and an accessible board.

## Required context

- [User stories US-013, US-014, and US-020 through US-022](../product/user-stories.md)
- [Requirements](../product/requirements.md)
- [User experience](../product/user-experience.md)
- [Frontend](../technical/frontend.md)
- [Backend](../technical/backend.md)
- [Database](../technical/database.md)

## In scope

- Comment create/edit/delete/moderation and safe Markdown.
- Mentions and in-app notifications.
- Private direct uploads, attachment validation, access, and cleanup.
- Workspace task search, combined filters, sorting, and cursor pagination.
- URL-backed filters, personal presets, list/board toggle, and keyboard status movement.

## Out of scope

- Real-time presence, chat, email/push notifications, comment search, saved custom views, and bulk operations.

## Acceptance criteria

- Comment author and moderator behavior matches permissions.
- Mentioning an active member creates at most one notification per comment/member.
- Unauthorized users cannot enumerate or download attachments.
- Search/filter results are tenant-scoped, stable across cursors, and exclude archived records by default.
- Filter URL can be shared and recreates the same view.
- Board functions without drag-and-drop and announces status changes accessibly.
- Search and upload errors are recoverable without losing relevant user input.

## Verification

- Comment/notification idempotency tests, safe-content tests, attachment security tests, query/cursor integration tests, accessibility component tests, and Playwright collaboration/search journey.
