# Database design

## Database choice

PostgreSQL is the system of record. It provides relational integrity, transactions, cursor-friendly indexes, and sufficient full-text/trigram search for the MVP. An external search service or vector database is deferred until measured volume or retrieval quality justifies it.

Files remain in private object storage. PostgreSQL stores their metadata and object keys, never permanent public URLs.

## Relationships

```text
User ──< WorkspaceMember >── Workspace ──< Project ──< Task ──< Comment
                 │                │           │          ├──< TaskStatusEvent
                 │                │           │          ├──< TaskLabel >── Label
                 │                │           │          └──< Attachment
                 │                │           ├──< AiSummary
                 │                │           └──< ProjectActivity
                 │                ├──< Invitation
                 │                ├──< Notification
                 │                └──< AuditEvent
                 └── role
```

## Tables

Identifiers are UUIDs. Mutable records include `created_at` and `updated_at`; actor fields are retained where they provide attribution. Timestamps are stored in UTC.

| Table | Important fields and constraints |
| --- | --- |
| `users` | `id`, unique `auth_subject`, normalized unique `email`, `display_name`, `avatar_url`, `timezone`, `last_seen_at`, `deleted_at`. |
| `workspaces` | `id`, `name`, unique `slug`, `timezone`, `working_week`, `created_by`, `ai_monthly_budget`, `deleted_at`. |
| `workspace_members` | `workspace_id`, `user_id`, `role`, `joined_at`; composite primary key. |
| `invitations` | `id`, `workspace_id`, normalized `email`, `role`, hashed `token`, `expires_at`, `accepted_at`, `revoked_at`, `invited_by`; one active invitation per workspace/email. |
| `projects` | `id`, `workspace_id`, workspace-unique `key`, `name`, `description`, `status`, `start_date`, `target_date`, `created_by`, `archived_at`, `version`. |
| `tasks` | `id`, `workspace_id`, `project_id`, project-unique `sequence_number`, `title`, `description`, `status`, `priority`, nullable `assignee_id`, `due_date`, `created_by`, `completed_at`, `archived_at`, `version`. Display key is project key plus sequence, such as `WEB-42`. |
| `task_status_events` | `id`, `workspace_id`, `task_id`, `from_status`, `to_status`, `changed_by`, `changed_at`; append-only. |
| `comments` | `id`, `workspace_id`, `task_id`, `author_id`, `body`, `edited_at`, `deleted_at`, `idempotency_key`; deleted comments retain a tombstone. |
| `comment_mentions` | `comment_id`, `mentioned_user_id`, `created_at`; unique pair. |
| `labels` | `id`, `workspace_id`, `name`, `color`, `created_by`; normalized unique name in a workspace. |
| `task_labels` | `task_id`, `label_id`; composite primary key and same-workspace invariant. |
| `attachments` | `id`, `workspace_id`, `task_id`, unique `object_key`, `original_name`, `media_type`, `byte_size`, `checksum`, `processing_status`, `uploaded_by`. |
| `notifications` | `id`, `workspace_id`, `recipient_id`, `type`, `actor_id`, `target_type`, `target_id`, safe `metadata`, `read_at`, `created_at`. |
| `project_activities` | `id`, `workspace_id`, `project_id`, `actor_id`, `action`, `target_type`, `target_id`, safe `metadata`, `created_at`. |
| `ai_summaries` | `id`, `workspace_id`, `project_id`, `requested_by`, `status`, `summary`, `snapshot_cutoff`, `source_fingerprint`, `provider`, `model`, `prompt_version`, token counts, `error_code`, lifecycle timestamps, `idempotency_key`. |
| `audit_events` | `id`, `workspace_id`, `actor_id`, `action`, `target_type`, `target_id`, safe `metadata`, `request_id`, `created_at`; append-only. |

## Index strategy

- `tasks(workspace_id, project_id, status, archived_at)` for project lists and boards.
- `tasks(workspace_id, assignee_id, status, due_date)` for personal work and overdue queries.
- `tasks(workspace_id, updated_at desc, id)` for stable cursor pagination.
- GIN search vector over task key, title, and description.
- `task_status_events(workspace_id, changed_at, to_status)` for completion trends.
- `project_activities(project_id, created_at desc, id)` for project history.
- `notifications(recipient_id, read_at, created_at desc)` for the notification center.
- Partial indexes for active tasks, active invitations, and unread notifications.

Comments are excluded from general task search in the MVP to avoid noisy results and unintended discovery behavior.

## Integrity and consistency rules

- Every tenant-owned table carries `workspace_id`, including records reachable through projects or tasks.
- Data repositories require a workspace context. Unscoped tenant queries are prohibited outside explicit administration jobs.
- Parent and child workspace identity must match for tasks, labels, comments, mentions, attachments, notifications, and AI summaries.
- Project task-sequence allocation is transactional so concurrent task creation cannot duplicate keys.
- Task and project mutations submit an expected `version`; mismatches return a conflict without applying the stale write.
- Entering `Done` sets `completed_at`; leaving `Done` clears it. `task_status_events` preserves historical transitions.
- An assignee must be an active workspace member. Removing a member unassigns active tasks transactionally but preserves historical actor references.
- Activity records are team-facing. Audit records are administrative/security-facing. Neither embeds full comments, descriptions, or AI payloads.
- Dashboard values are computed from authoritative tables. Cached aggregates are added only after profiling.
- Archival is reversible and excluded by default. Workspace deletion first revokes access, then purges data and objects according to retention policy.
- AI summaries are immutable after reaching a terminal state. Regeneration inserts a new row.

## Search and pagination

- Normalize and validate search text and filters at the server boundary.
- Use opaque cursor pagination based on stable sort columns plus ID tie-breaker.
- Cap result page size and member-combobox result size.
- Include `generatedAt` and workspace timezone on dashboard/search responses when the cutoff affects interpretation.
- Keep normalized filters in query keys so cached results cannot cross workspace or filter boundaries.
