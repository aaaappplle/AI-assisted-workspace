# ADR-001 — Server-first Next.js architecture

| Field | Value |
| --- | --- |
| Status | Accepted |
| Date | 2026-06-23 |
| Owners | Architecture |

## Context

The application is read-heavy across dashboards, task lists, details, and settings, but also needs interactive filters, comments, inline task controls, and streamed AI progress. Authentication and tenant authorization must remain close to data access, while browser JavaScript should stay controlled.

## Decision

Use Next.js App Router with Server Components as the default rendering model.

- Server Components perform authorized initial reads and render page shells.
- Client Components are limited to interactive boundaries.
- Server Actions handle same-application form mutations.
- Route Handlers handle client-driven search, uploads, SSE, webhooks, and health protocols.
- Domain and data-access modules remain framework-independent where practical.

## Consequences

Positive:

- Less client JavaScript and fewer browser data waterfalls.
- Secrets and database access remain server-side.
- Authorization naturally sits near initial data reads.
- Pages can stream meaningful server-rendered regions.

Costs:

- Server/client import boundaries require discipline and automated checks.
- Cache invalidation spans Next.js rendering and TanStack Query on interactive screens.
- Team must understand serialization constraints and avoid passing oversized models.

## Alternatives considered

- Client-side SPA: rejected because it increases initial client work and duplicates server authorization/data orchestration.
- Separate public API plus frontend from day one: rejected as unnecessary operational surface for the MVP.
- Server Actions for every interaction: rejected because search, streaming, uploads, and webhooks have clearer HTTP protocol needs.
