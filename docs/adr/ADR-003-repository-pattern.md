# ADR-003: Access data through repositories, not the API client directly

- Status: Accepted
- Date: 2025-11-03
- Deciders: Python team

## Context

With a layered architecture in place ([[ADR-001-layered-architecture]]) and a shared
`ApiClient` wrapper, services began calling `ApiClient` directly. This coupled business logic
to the network layer and meant the same endpoint was called and parsed in several services
with slightly different handling.

## Decision

Services obtain data exclusively through a **Repository**. A repository is the only type that
may depend on `ApiClient` (or a database/ORM). Services depend on a repository, never on
`ApiClient` directly.

Rules:

- A service must not import or reference `ApiClient` directly.
- A service must not construct its own HTTP client or call a database/ORM directly.
- Each domain area exposes a repository; concrete repositories live in the data layer and
  wrap `ApiClient`.

## Consequences

- Caching, retries, and offline behavior can be added inside a repository without touching
  services.
- One canonical data-access path per domain area.
- A service that calls the API client directly is an architecture violation and should be
  reported, for example:
  "[MEDIUM] Service calls API client directly — violates ADR-003 (Repository Pattern)".
