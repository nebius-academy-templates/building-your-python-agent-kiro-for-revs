# ADR-001: Adopt a layered architecture

- Status: Accepted
- Date: 2025-09-02
- Deciders: Python team

## Context

Route handlers and script entrypoints had grown to hundreds of lines each, mixing request
parsing, business logic, and data access. This made the codebase hard to test and hard to
reason about.

## Decision

We adopt a layered architecture for all new code. An entrypoint — a route handler, a CLI
command, or a script's `main()` — parses input and formats output only. A **service** module
holds business logic. Entrypoints must not contain business logic or perform data access
directly.

## Consequences

- Business logic becomes unit-testable independent of the entrypoint.
- A new service module is required for new functionality, adding some structure upfront.
- See [[ADR-003-repository-pattern]] for how services obtain their data.
