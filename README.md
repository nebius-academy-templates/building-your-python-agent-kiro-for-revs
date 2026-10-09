# PR Review Agent — practice project

You are building a PR Review Agent for the Python team. It reviews Python pull requests for
style, security, and architecture issues, organizes findings per file with `[SEVERITY]` tags,
ends with a `## Summary`, and always asks for confirmation before posting a comment to GitHub.

## Files to complete in this task

- `AGENTS.md` — the agent system prompt. Cover four sections: **Role**, **Review criteria**,
  **Output format** (with `[SEVERITY]` tags and a closing `## Summary`), and **Guardrails**.
- `results/test-session-1.md` — a transcript of your three tests: the clean-PR format check, the
  publish-gate confirmation, and the scope refusal.
