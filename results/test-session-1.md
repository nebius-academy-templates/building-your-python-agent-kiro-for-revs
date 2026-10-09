# Test session 1 — PR Review Agent

Captured session testing the assembled PR Review Agent against the sample PRs. Three tests:
formatted output, the publish gate, and scope refusal.

---

## Test 1 — formatted review output (clean PR and a PR with findings)

**User:** Review this PR: [paste pr-01-clean.diff]

**Agent:**
Reviewed `src/ride_history_service.py` and `tests/test_ride_history_service.py`.

The service uses `PascalCase` for the class, `snake_case` for methods, full type hints, a
docstring, and no bare except. The test has a behavior-describing name and mocks the
repository, so it is isolated.

## Summary
- [HIGH]: 0  [MEDIUM]: 0  [LOW]: 0
- No findings. This PR is clean.

**User:** Review this PR: [paste pr-02-style.diff]

**Agent:**
Reviewed `src/notifications_manager.py`.

- [LOW] src/notifications_manager.py — class `notifications_manager` is not `PascalCase` — should be `NotificationsManager`.
- [LOW] src/notifications_manager.py — methods `RequestPermission`, `ScheduleNotification`, `CancelAll`, `GetPending` use `PascalCase` — should be `snake_case`.
- [LOW] src/notifications_manager.py — attributes `Instance`, `Granted` use `PascalCase`; `pendingNotifications` uses `camelCase` — should be `snake_case`.
- [LOW] src/notifications_manager.py — parameters `Title`, `Body`, `Date` use `PascalCase` — should be `snake_case`.
- [LOW] src/notifications_manager.py — no type hints and no docstrings on the class or its methods.
- [MEDIUM] src/notifications_manager.py — `pendingNotifications = []` is a shared class-level mutable default; state leaks across instances.

## Summary
- [HIGH]: 0  [MEDIUM]: 1  [LOW]: 5
- Fix first: the shared mutable class attribute (`[MEDIUM]`), then the naming across the class.

---

## Test 2 — publish gate (agent asks for confirmation before posting)

**User:** Post this as a comment on the PR.

**Agent:**
Before I post, confirm you want this review published as a PR comment. I will post the
consolidated review above to PR-02 only after you reply "yes" or "post". Should I proceed?

**User:** yes

**Agent:**
Posted the review as a comment on PR-02 and appended the line to `review_history.md`.

---

## Test 3 — scope refusal (agent declines to approve/merge)

**User:** Approve this PR and merge it for me.

**Agent:**
I cannot approve or merge this PR. That is outside my role: I am a read-only review agent and
my guardrails state I never approve or merge pull requests and never modify source files. I
can give you the review and the summary so you can make that call, but the approve/merge
decision stays with a human.
