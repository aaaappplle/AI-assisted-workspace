# Application data models

## Model layers

Database records are not passed directly into components. The application separates four layers:

1. **Persistence models** mirror database columns and relations.
2. **Domain models** express capabilities, workflow transitions, due-state calculations, and invariants.
3. **Command/input models** validate untrusted form, action, route, and webhook data.
4. **View models** expose only the serialized fields needed by a page or component.

This prevents ORM details from becoming the public application contract and limits data sent to Client Components.

## Core domain models

| Model | Responsibility |
| --- | --- |
| `WorkspaceContext` | Workspace identity/timezone, active membership, role, and calculated capabilities. |
| `ProjectSummary` | Project lifecycle, dates, deterministic counts/progress, risk flags, activity, and latest summary state. |
| `TaskSummary` | ID/key, title, status, priority, assignee, due state, labels, version, and timestamps. |
| `TaskDetail` | Task summary plus sanitized description, attachment summaries, comments, and activity cursor. |
| `TaskFilters` | Validated query, projects, assignees, statuses, priorities, labels, creator, due rules, sort, and cursor. |
| `DashboardSnapshot` | Cutoff/timezone, KPIs, distributions, trend points, risk flags, and drill-down filter links. |
| `AiProjectSummary` | Durable generation status, immutable cutoff/fingerprint, cited output, model metadata, usage, and safe error. |
| `NotificationView` | Recipient-safe actor/action/target summary and read state. |
| `ActivityView` | Team-safe actor/action/target description and timestamp. |
| `PaginatedResult<T>` | Items, opaque next cursor, generated time, and optional cheap total. |

## Task workflow

```text
Backlog ──> To do ──> In progress ──> In review ──> Done
   │          │            │              │           │
   └──────────┴────────────┴──────────────┴──────────> Cancelled

Done or Cancelled → To do is the explicit reopen path.
Backward movement among active states is allowed and recorded.
```

Archived is not a task status. It is a separate visibility and retention state.

A transition checks:

- Workspace membership and capability.
- Project is not archived.
- Current task version matches expected version.
- Relationship restrictions for Members.
- Target status is valid.
- Completion timestamp and status history update atomically.

## Project workflow

```text
Planned → Active ↔ On hold → Completed
   │        │          │          │
   └────────┴──────────┴──────────> Archived

Completed may reopen to Active.
Archived must be restored before normal edits.
```

## AI summary workflow

```text
queued → running → completed
   │        │
   └────────┴────→ failed
```

Terminal records never transition. A new request creates a new summary rather than resetting or overwriting an old one.

The summary source model includes:

- Project identity, description, dates, and deterministic metrics.
- Active and recently completed task summaries.
- Bounded recent comments when required for blocker context.
- Stable task keys and links used for citations.
- Snapshot cutoff and source fingerprint.

Attachments are not sent to the model in the MVP.

## Serialization rules

- Convert dates to an explicit ISO representation at server boundaries; localize only in presentation.
- Represent enum values with closed domain types, not arbitrary strings.
- Return capability booleans needed by the UI, but still re-authorize every mutation.
- Never serialize auth-provider subjects, storage object keys, audit metadata, prompts, or internal provider errors to general components.
- Sanitize Markdown before rendering and treat AI output with the same untrusted-content policy.
