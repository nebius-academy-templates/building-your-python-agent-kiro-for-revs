---
name: "test-conventions"
description: "Python test conventions. Load when reviewing test files under tests/ or named test_*.py, or any file that imports pytest."
---

# Test code conventions

Apply these when reviewing **test code**: files under `tests/`, files named `test_*.py`,
or any file that imports `pytest`. The `.py` extension alone does not identify a test file,
so the trigger is the `tests/` location, the `test_*.py` name, or a `pytest` import.

## Test naming

- Test functions are named `test_<behavior>` and describe the behavior under test:
  `test_load_rides_returns_recent_rides_from_repository`, not `test_1` or `test_load`.
- One behavior per test.

## Fixtures

- Use `pytest` fixtures for shared setup instead of duplicating setup code across tests.
- A fixture that builds a mocked collaborator is reused, not copy-pasted into each test.

```python
import pytest
from unittest.mock import Mock

@pytest.fixture
def ride_repository() -> Mock:
    repo = Mock()
    repo.recent_rides.return_value = ["ride-1", "ride-2"]
    return repo

def test_load_rides_returns_recent_rides_from_repository(ride_repository: Mock) -> None:
    service = RideHistoryService(ride_repository)
    assert service.load_rides() == ["ride-1", "ride-2"]
```

## Isolation

- Tests never hit the real network, a real database, or the real filesystem. Mock those
  collaborators (`unittest.mock.Mock`, `monkeypatch`, or a fixture) so a test is fast and
  deterministic.
- A test that makes a real network call is a finding.

## Severity

- `[LOW]` — weak test naming.
- `[MEDIUM]` — duplicated setup that should be a fixture.
- `[HIGH]` — a test that is not isolated (real network/DB/filesystem).
