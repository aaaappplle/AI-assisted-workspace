# TASK-002 — Authentication and tenancy

| Field | Value |
| --- | --- |
| Status | Proposed |
| Milestone | 1 |
| Depends on | TASK-001 |
| Owner | Unassigned |

## Goal

Allow users to sign in, create or join workspaces, switch tenants, and manage permitted membership actions without cross-workspace leakage.

## Required context

- [Requirements](../product/requirements.md)
- [Roles and permissions](../product/roles-and-permissions.md)
- [Database](../technical/database.md)
- [Backend](../technical/backend.md)
- [ADR-002](../decisions/adr-002-postgresql-multi-tenancy.md)

## In scope

- Auth adapter, session handling, user provisioning, and sign-out.
- Workspace, membership, and invitation persistence.
- Central capability service.
- Onboarding, workspace switcher, protected shell, team list, and member settings.
- Invite accept/reject/expired/revoked flows.
- Owner transfer and last-owner protection.

## Out of scope

- Projects, tasks, billing, project-level ACLs, and external guest access.

## Acceptance criteria

- New user can authenticate and create a workspace.
- Invited user joins exactly once with the intended role.
- Owner/Admin actions match the permission matrix.
- Viewer and Member cannot call membership administration boundaries directly.
- Workspace B cannot read or mutate workspace A data using guessed IDs.
- Last Owner cannot leave or be removed before ownership transfer.
- Protected pages recover safely from expired sessions.
- Role and membership changes produce audit events without personal content leakage.

## Verification

- Capability unit matrix, database tenant-isolation integration tests, invitation idempotency tests, and Playwright onboarding/invitation journey.
