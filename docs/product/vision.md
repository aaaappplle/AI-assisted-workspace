# Product vision

## Vision

Give small and medium teams one calm, understandable place to coordinate work and obtain trustworthy project insight without turning control over to an AI agent.

## Problem

Teams often split work status across task boards, chat, meetings, and personal notes. Managers spend time reconstructing progress, contributors struggle to find their current priorities, and stakeholders receive summaries that become stale quickly.

## Target users

- **Team owner:** creates the workspace, controls access, and is accountable for data and policy.
- **Administrator:** manages membership, workspace configuration, safety, and operational limits.
- **Project manager:** plans projects, assigns work, resolves blockers, and communicates progress.
- **Contributor:** performs assigned work, updates status, and collaborates through comments.
- **Stakeholder/viewer:** follows progress without changing work.

## Value proposition

- One authoritative view of projects, tasks, ownership, status, and deadlines.
- Fast personal and team views that reduce coordination overhead.
- Metrics that reconcile to the underlying task list.
- AI summaries grounded in visible work and linked back to task evidence.
- Permissions and auditability suitable for a credible production system.

## Product principles

1. **Work management first.** Core project and task flows do not depend on AI.
2. **One accountable owner.** A task has at most one primary assignee in the MVP.
3. **Evidence over magic.** Dashboard numbers drill into matching tasks; AI claims cite task keys.
4. **Humans retain control.** AI cannot mutate projects or tasks.
5. **Permissions are product behavior.** Server enforcement and non-leaking failures are designed, not bolted on.
6. **Useful defaults beat configurability.** Fixed workflow and roles keep the MVP coherent.
7. **Accessible by construction.** Every interaction has keyboard and non-drag behavior.

## Desired outcomes

- Contributors can identify and update their work without asking a manager.
- Managers can identify overdue, unassigned, blocked, and completed work quickly.
- Stakeholders can understand project progress without edit access.
- AI reduces summary-writing effort while keeping every concrete claim verifiable.

## Non-goals

The MVP is not a general-purpose enterprise project-management suite, team chat, autonomous agent platform, source-code integration hub, time-tracking system, or configurable analytics builder. Detailed exclusions live in [MVP scope](./scope.md).

## Success signals

- Primary onboarding-to-completed-task journey succeeds without guidance.
- Users regularly use personal task views and dashboard drill-downs.
- Dashboard metrics reconcile exactly with task results.
- Generated summaries are reviewed through cited tasks rather than trusted blindly.
- Permission violations and cross-tenant access are prevented and observable safely.
