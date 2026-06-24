# TASK-005 — Dashboard

| Field | Value |
| --- | --- |
| Status | Proposed |
| Milestone | 4 |
| Depends on | TASK-003 |
| Owner | Unassigned |

## Goal

Provide trustworthy workspace health statistics whose values can be reproduced by drilling into the underlying task list.

## Required context

- [Dashboard requirements and metric definitions](../product/requirements.md)
- [User stories US-030 and US-031](../product/user-stories.md)
- [Dashboard composition](../product/user-experience.md)
- [Database](../technical/database.md)
- [Testing and observability](../technical/testing-and-observability.md)

## In scope

- Active, completed, overdue, due-soon, and unassigned KPIs.
- Status, priority, project, and assignee distributions.
- 7/30/90-day completion trends.
- Deterministic at-risk projects and recent activity.
- Visible cutoff/timezone and URL-filter drill-down links.
- Accessible chart alternatives.

## Out of scope

- Custom reports, forecasting, capacity planning, time estimates, materialized analytics infrastructure, and AI-generated risk rules.

## Acceptance criteria

- Each KPI equals the corresponding filtered task-list result at the same cutoff.
- Completion trend uses status events and handles reopened tasks correctly.
- Due-date calculations use workspace timezone, including daylight-saving boundaries.
- Every interactive metric produces a valid shareable filter URL.
- Charts expose equivalent labels/data without relying on color or pointer input.
- Dashboard clearly displays generation time and degraded/error states.

## Verification

- Shared-predicate unit tests, seeded PostgreSQL reconciliation tests, timezone edge cases, chart accessibility tests, and Playwright dashboard drill-down journey.
