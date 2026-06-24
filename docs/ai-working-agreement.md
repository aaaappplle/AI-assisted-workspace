# AI working agreement

## Purpose

This file tells coding agents how to use and maintain the documentation. It does not replace product or technical sources of truth.

## Context-loading protocol

Before implementing a task:

1. Read the selected task brief completely.
2. Read every document listed in its `Required context` section.
3. Read referenced ADRs and confirm their status is `Accepted`.
4. Inspect the current code and tests; documentation describes intent but code may expose incomplete migration work.
5. State assumptions when a requirement is genuinely unspecified.

Do not load every document reflexively. Small, relevant context reduces contradictions and improves agent focus.

## Change protocol

- Product behavior changes update the owning product document first.
- Authorization changes update `product/roles-and-permissions.md` and its matrix tests.
- Durable technical choices require a new ADR or a superseding ADR.
- Implementation sequencing changes update `delivery/roadmap.md` and task dependencies.
- Implementation details and discovered constraints update the active task brief.
- A task is complete only when its acceptance criteria and required tests pass.
- Keep documentation and code changes in the same change set when behavior changes.

## Documentation rules

- One concept has one owner document; other files link to it instead of copying it.
- Use stable IDs: `US-###` for user stories, `TASK-###` for tasks, and `ADR-###` for decisions.
- Use exact domain vocabulary from the glossary.
- Write testable statements with actors, conditions, results, and failure behavior.
- Distinguish current decisions from proposals and future ideas.
- Record dates as `YYYY-MM-DD` and use explicit timezones where behavior depends on time.
- Never place secrets, production identifiers, tokens, personal data, or raw customer content in documentation.
- Prefer small diagrams and tables only when they clarify relationships.

## Definition of ready

A task is ready when:

- Its goal and user outcome are explicit.
- Dependencies are complete or intentionally mocked.
- Required context and ADRs are linked.
- In-scope and out-of-scope boundaries are clear.
- Acceptance criteria are observable.
- Permission, accessibility, telemetry, migration, and test impacts are identified.
- No unresolved decision materially changes its implementation.

## Definition of done

A task is done when:

- All acceptance criteria pass.
- Authorization is enforced at server boundaries.
- Unit/integration/end-to-end tests are added in proportion to risk.
- Loading, empty, error, forbidden, stale, and responsive states are handled where relevant.
- Accessibility checks pass.
- Telemetry contains safe correlation data and no sensitive content.
- Documentation and ADR references remain accurate.
- The production build and required CI gates pass.

## Domain glossary

| Term | Meaning |
| --- | --- |
| Workspace | Tenant and permission boundary containing members, projects, and tasks. |
| Project | Container for related work with its own lifecycle and target dates. |
| Task | Smallest assignable work item with one optional primary assignee. |
| Activity | Team-visible history of meaningful product changes. |
| Audit event | Restricted security/administration record. |
| AI summary | Immutable, read-only project progress snapshot generated from authorized data. |
| Archive | Reversible removal from normal active views; not hard deletion. |
| Capability | Named server-authorized action derived from role and resource relationship. |
