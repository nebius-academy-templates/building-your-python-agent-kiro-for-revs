---
name: "review-pr"
description: "Run the full PR review on explicit invocation with /review-pr. Fetches the PR, delegates to the review sub-agents, consolidates findings, and posts only after confirmation."
---

# review-pr

This workflow runs the full pull-request review. It is triggered explicitly with
`/review-pr <number>` and does not auto-run.

## Steps

1. **Fetch the PR via GitHub MCP.** Use the GitHub MCP server (`get_pull_request`) to load
   the diff for the requested PR number from the shared practice repo
   `nebius-academy-templates/python-pr-review-practice` (not the learner's own repo), so
   `/review-pr 1` resolves against the practice repo's PRs.
2. **Delegate to the review sub-agents.** Hand the diff to each specialist sub-agent:
   - `style-reviewer` — applies **app-conventions** to non-test files and
     **test-conventions** to test files.
   - `security-reviewer` — flags hardcoded secrets, unsafe API use, and missing input
     validation.
   - `architecture-reviewer` — checks services against the ADR guidelines (repository
     pattern, configuration, dependency injection).
3. **Consolidate the three outputs.** Merge the sub-agents' findings into one review,
   de-duplicated and ordered by severity (`[HIGH]`, `[MEDIUM]`, `[LOW]`), and build the
   `## Summary` section with the per-severity counts and the top fix.
4. **Ask for confirmation before posting.** Show the consolidated review and ask the user
   whether to post it. Do not post anything yet.
5. **Post only on "yes" or "post".** Post the review as a PR comment only after the user
   replies "yes" or "post". On any other reply, do not post.
6. **Log the review.** Append one line to `review_history.md` in the format
   `YYYY-MM-DD | PR-N | description | SEVERITY`.
