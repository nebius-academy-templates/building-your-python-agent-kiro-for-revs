# Test session 3 — selective skill loading and the workflow run

Captured session testing that the convention skills load selectively by file type, then a
full `/review-pr 1` run with the publish gate.

---

## Test A — application-file PR (PR #2): only app-conventions loads

**User:** Review PR #2. It changes `src/profile_service.py` only.

**Agent:**
PR #2 touches an application file (`src/profile_service.py`).

app-conventions loaded and was applied to this review.

test-conventions was not invoked, because this PR has no test file.

Findings for `src/profile_service.py`:
- [LOW] `src/profile_service.py` — method `LoadProfile` is `PascalCase`; should be `load_profile` (app-conventions naming rule).
- [LOW] `src/profile_service.py` — missing type hints and docstring (app-conventions).

## Summary
- [LOW]: 2 — naming and documentation only.

---

## Test B — test-file PR (PR #4): only test-conventions loads

**User:** Review PR #4. It changes `tests/test_trip_service.py` only.

**Agent:**
PR #4 touches a test file (`tests/test_trip_service.py`).

test-conventions loaded and was applied to this review.

app-conventions was not invoked, because this PR has no application code.

Findings for `tests/test_trip_service.py`:
- [MEDIUM] `tests/test_trip_service.py` — duplicated setup across three tests; extract a fixture (test-conventions fixtures rule).
- [HIGH] `tests/test_trip_service.py` — `test_fetch_trips` makes a real network call; mock the repository so the test is isolated (test-conventions isolation rule).

## Summary
- [HIGH]: 1  [MEDIUM]: 1 — a non-isolated test and duplicated setup.

---

## Test C — the workflow run and the publish gate

**User:** /review-pr 1

**Agent:**
Fetched PR-01 via the GitHub MCP server (`get_pull_request`) — `src/ride_history_service.py`
and its test. Ran the review.

## Summary
- No findings. PR-01 is clean.

Would you like me to post this as a comment on PR-01? I will post only on "yes" or "post".

**User:** no

**Agent:**
Understood — I will not post anything. The review stays here for your reference and nothing
was published.
