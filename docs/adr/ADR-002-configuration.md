# ADR-002: Load configuration from the environment

- Status: Accepted
- Date: 2025-10-14
- Deciders: Python team

## Context

Configuration values and secrets were hardcoded across multiple modules, or read from
environment variables ad hoc wherever they were needed. This made it hard to know what
configuration existed and easy to accidentally commit a secret.

## Decision

All configuration and secrets are loaded from environment variables or a single config
module. Modules do not hardcode configuration values or call `os.environ` directly outside
that module.

## Consequences

- One place to see every configuration value the app depends on.
- Secrets can be rotated without touching business logic.
- A hardcoded API key, token, or password anywhere else is both a configuration violation
  and, most often, a security finding.
