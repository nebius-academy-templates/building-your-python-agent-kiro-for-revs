# ADR-004: Inject dependencies via constructors

- Status: Accepted
- Date: 2025-11-20
- Deciders: Python team

## Context

Some services reached for a module-level global or a `ServiceLocator`-style registry to get
their dependencies, making it hard to test a service in isolation or see what it depends on
just by reading its constructor.

## Decision

Dependencies are passed into a service via its constructor or function parameters. Global
singletons and the ServiceLocator pattern are not allowed.

## Consequences

- Every service's dependencies are visible in its `__init__` signature.
- Tests construct a service with mocks/fakes instead of monkeypatching a global registry.
- A module-level global instance or a ServiceLocator lookup is an architecture violation.
