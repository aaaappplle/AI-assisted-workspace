# User roles and permissions

## Authorization model

Roles are scoped to a workspace. All MVP workspace members can see all projects in that workspace; private projects and project-level ACLs are excluded.

Application code authorizes named capabilities such as `task.update.any` or `member.invite`, rather than scattering role-name checks. UI visibility improves usability but never replaces server authorization.

## Roles

- **Owner:** full control, including ownership transfer and workspace deletion.
- **Admin:** manages workspace configuration, members, all projects/tasks, moderation, audit, and AI limits; cannot transfer ownership or manage Owner/Admin promotion.
- **Project Manager:** manages projects, all tasks, and labels; cannot administer members or security settings.
- **Member:** performs normal assigned work, creates tasks, collaborates, and edits tasks they created or own.
- **Viewer:** read-only stakeholder access.

## Capability matrix

| Capability | Owner | Admin | Project Manager | Member | Viewer |
| --- | :---: | :---: | :---: | :---: | :---: |
| View projects, tasks, comments, dashboard, and activity | Yes | Yes | Yes | Yes | Yes |
| Create a task | Yes | Yes | Yes | Yes | No |
| Edit any task or its assignee, priority, labels, and due date | Yes | Yes | Yes | No | No |
| Edit a task created by or assigned to self | Yes | Yes | Yes | Yes | No |
| Change status of a task assigned to self | Yes | Yes | Yes | Yes | No |
| Archive or restore any task | Yes | Yes | Yes | No | No |
| Comment and mention workspace members | Yes | Yes | Yes | Yes | No |
| Edit or delete own comment | Yes | Yes | Yes | Yes | No |
| Moderate another user's comment | Yes | Yes | No | No | No |
| Create or edit a project | Yes | Yes | Yes | No | No |
| Complete, reopen, archive, or restore a project | Yes | Yes | Yes | No | No |
| Generate an AI project summary | Yes | Yes | Yes | Yes | No |
| View AI summaries | Yes | Yes | Yes | Yes | Yes |
| Invite a Member or Viewer | Yes | Yes | No | No | No |
| Assign Project Manager role | Yes | Yes | No | No | No |
| Promote or demote Admin | Yes | No | No | No | No |
| Remove members or revoke invitations | Yes | Yes | No | No | No |
| Manage labels | Yes | Yes | Yes | No | No |
| Manage workspace settings and AI limits | Yes | Yes | No | No | No |
| View audit log and AI usage | Yes | Yes | No | No | No |
| Transfer ownership or delete workspace | Yes | No | No | No | No |

## Relationship and lifecycle rules

- A Member may choose any active workspace member as the initial assignee when creating a task.
- A Member may edit a task they created or are assigned, but cannot change its project, creator, archival state, or arbitrary assignee after creation.
- A Project Manager may manage all projects and tasks but cannot invite, remove, or change roles.
- A Viewer cannot mutate, including comments and AI generation.
- Archived projects are read-only until restored by a permitted role.
- The last Owner cannot leave, be removed, or demote themselves before transferring ownership.
- A user cannot assign or mention a removed or outside-workspace account.
- Cross-workspace resource requests generally return not found to avoid leaking resource existence.

## Enforcement requirements

Every Server Action and Route Handler must:

1. Authenticate the session.
2. Resolve workspace membership from server-controlled data.
3. Load the tenant-scoped target record.
4. Calculate capabilities, including ownership/assignment relationship.
5. Reject unauthorized behavior before mutation or sensitive serialization.
6. Record security-relevant changes in the audit log.

Permission tests are table-driven across role, resource relationship, project archival state, and boundary type. Direct request tests are required; hiding a button is not a permission test.
