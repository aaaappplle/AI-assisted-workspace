# ADR-004 — Frontend state ownership

| Field | Value |
| --- | --- |
| Status | Accepted |
| Date | 2026-06-23 |
| Owners | Frontend architecture |

## Context

The chosen stack includes Server Components, TanStack Query, Zustand, URL routing, and React local state. Without explicit ownership, the same tasks, permissions, or filters could be duplicated across stores and become inconsistent.

## Decision

Assign exactly one primary owner to each state category:

- URL: shareable navigation, filters, sorting, view, date range, and pagination cursor.
- PostgreSQL/Server Components: durable product state and initial rendering.
- TanStack Query: live remote client cache, pagination, polling, and optimistic server mutations.
- Zustand: ephemeral cross-component browser UI only.
- React local state: component-local interaction.

Zustand must not hold canonical projects, tasks, comments, roles, permissions, dashboard values, or AI summaries.

## Consequences

Positive:

- Refresh and deep links preserve meaningful view state.
- Durable data has one authority.
- Zustand remains small and avoids hydration/tenant leakage risks.
- Query invalidation can be workspace- and resource-scoped.

Costs:

- Developers must choose boundaries deliberately rather than defaulting everything to one store.
- Interactive pages may coordinate server-rendered initial data with TanStack Query hydration/refetch.
- URL filter schemas require stable parsing and normalization.

## Alternatives considered

- Zustand for all frontend data: rejected because it duplicates server cache and authorization-sensitive records.
- TanStack Query for all first-load data: rejected because it underuses server rendering and adds client waterfalls.
- Local component state for filters: rejected because filtered views must be shareable and restorable.
