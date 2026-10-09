# Observability table

Filled from the Post Tool Use logging hook (`.kiro/hooks/log-tool-calls.json` ->
`run-log.jsonl`), one run of `/review-pr 1` through `/review-pr 5`, each row read from the
run's events filtered by `session_id`.

| Input | Tools called (count) | `security-reviewer` delegated? | Any tool error? |
|-------|----------------------|--------------------------------|-----------------|
| PR-01 | 4 | Y | none |
| PR-02 | 5 | Y | none |
| PR-03 | 3 | N | none |
| PR-04 | 6 | Y | none |
| PR-05 | 7 | Y | none |

## PR-03 diagnostic

PR-03 adds hardcoded credentials to `src/api_client.py` (the `sk-prod-...` API key, the
`ADMIN_USER`/`ADMIN_PASS` pair) and a QA-password fallback plus `debug_log_in` in
`src/login_service.py`. The consolidated review for PR-03 missed the credential findings.

Reading the run log, **the `security-reviewer` sub-agent was NOT called (N)** for PR-03: the
trace shows the orchestrator delegated to `style-reviewer` and `architecture-reviewer` but no
`security-reviewer` task event appears for that session_id, and no tool error was logged.

Failure mode: **sub-agent not called**. The security findings exist in the diff, but because
the security-reviewer never ran, they never entered the consolidated output. The fix is in
the orchestration (the `review-pr` skill), not in the security-reviewer prompt: the delegation
step must always include the security-reviewer, especially on diffs that touch auth or network
code.
