# TASK-001 — Foundation

| Field | Value |
| --- | --- |
| Status | Ready |
| Milestone | 0 |
| Depends on | None |
| Owner | Unassigned |

## Goal

Create a production-ready project foundation so later features share strict boundaries, repeatable checks, environment validation, accessible UI conventions, and safe telemetry.

## Required context

- [AI working agreement](../ai-working-agreement.md)
- [System architecture](../technical/architecture.md)
- [Frontend architecture](../technical/frontend.md)
- [Testing and observability](../technical/testing-and-observability.md)
- [ADR-001](../decisions/adr-001-server-first-nextjs.md)
- [ADR-004](../decisions/adr-004-client-state-ownership.md)

## In scope

- Next.js, strict TypeScript, Tailwind, and shadcn/ui baseline.
- Module boundaries for domain, data, server, UI, and provider adapters.
- Environment schema with safe server/public separation.
- Vitest and Playwright foundations.
- Sentry environment/release setup and redaction baseline.
- CI for type-check, lint, test, production build, and dependency/secret scanning.
- Health endpoint and preview deployment readiness.

## Out of scope

- Authentication, database domain schema, project/task UI, and AI provider integration.

## Acceptance criteria

- Production build succeeds with strict type checking.
- Server-only modules cannot be imported into Client Components without a build/lint failure.
- Missing required environment values fail fast with a safe message.
- Unit and Playwright smoke tests run in CI.
- Health response contains no secrets or dependency details.
- A safe test error reaches the correct non-production Sentry environment with release metadata.
- Baseline shell passes automated accessibility checks at desktop and 360 px.

## Verification

- Static checks, production build, one unit smoke test, one Playwright smoke test, and staging Sentry verification.
