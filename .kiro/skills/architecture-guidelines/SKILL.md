---
name: "architecture-guidelines"
description: "Team architecture guidelines. Load when reviewing architecture: layered architecture, repository pattern, configuration, and dependency injection."
---

# Architecture guidelines

The authoritative source is `docs/adr/`. Search it with Grep and cite the matching ADR by
name in every finding. This skill summarizes the rules so the reviewer knows what to look for.

## Layered architecture (ADR-001)

Code is organized in layers: presentation/service -> repository -> data (API client / ORM).
A layer may depend only on the layer directly below it. A service must not reach past the
repository layer into the data layer.

## Repository pattern (ADR-003)

Services obtain data **exclusively through a repository**. A service must not import or
reference `ApiClient` directly, and must not construct its own HTTP client or call a
database/ORM directly. A service that calls the API client directly is an architecture
violation, reported as:
`[MEDIUM] <file> — service calls API client directly — violates ADR-003 (Repository Pattern)`.

## Configuration (ADR-002)

Configuration (base URLs, keys, timeouts) is injected, not hardcoded or read ad hoc inside a
service. A service that hardcodes a base URL or reads environment variables directly violates
the configuration ADR.

## Dependency injection (ADR-004)

Collaborators (repositories, clients) are passed in through the constructor, not instantiated
inside the class. A class that constructs its own dependencies instead of receiving them is a
dependency-injection violation.
