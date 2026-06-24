# TASK-007 — Production hardening

| Field | Value |
| --- | --- |
| Status | Proposed |
| Milestone | 6 |
| Depends on | TASK-001 through TASK-006 |
| Owner | Unassigned |

## Goal

Verify that the completed system is secure, accessible, observable, recoverable, performant, and reproducible.

## Required context

- [Production requirements](../product/requirements.md)
- [Testing and observability](../technical/testing-and-observability.md)
- [Roadmap milestone 6](../delivery/roadmap.md)
- All accepted ADRs

## In scope

- Full permission and tenant-isolation audit.
- Accessibility, responsive, and cross-browser verification.
- Performance budgets and representative data profiling.
- Migration, backup restore, archival/deletion, queue recovery, rate-limit, and upload cleanup drills.
- Sentry source maps, alerts, traces, error IDs, and redaction review.
- Reproducible seed/demo/reset workflow and architecture/demo documentation.

## Out of scope

- New product capabilities or opportunistic expansion of the MVP.

## Acceptance criteria

- All CI and staging operational gates pass.
- No known critical/high authorization or tenant-isolation defect remains.
- Core pages meet agreed performance and accessibility targets with representative data.
- Restore and queue-recovery procedures are exercised successfully.
- Telemetry redaction review finds no prohibited content.
- Demo data, reset procedure, and presentation flow are reproducible.
- Known limitations and deferred risks are documented explicitly.

## Verification

- Full automated suite, manual accessibility pass, cross-browser matrix, load/profile report, operational drill evidence, and final security/telemetry review.
