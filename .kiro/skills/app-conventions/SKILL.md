---
name: "app-conventions"
description: "Python conventions for application (non-test) code. Load when reviewing application or library code under src/, not test files."
---

# Application code conventions

Apply these when reviewing **application code** (non-test `.py` files under `src/`).
Test files are also `.py`, so the extension alone is not the trigger: this skill is for
non-test, application code only.

## Naming

- Classes use `PascalCase` (`RideHistoryService`, not `notifications_manager`).
- Functions, methods and variables use `snake_case` (`load_rides`, not `RequestPermission`).
- No `camelCase` and no `PascalCase` methods on a class.
- Boolean names read as a question: prefix with `is_` / `has_` (`is_loading`, `has_error`).
- No abbreviations that hide intent (`count`, not `cnt`).

## Type hints and docstrings

- Every public function and method has full type hints on parameters and return value.
- Every public class and function has a one-line docstring stating what it does.

```python
class RideHistoryService:
    """Loads a rider's recent trips through the ride repository."""

    def load_rides(self) -> list["Ride"]:
        ...
```

## Error handling

- No **bare `except:`** — always catch a specific exception type.
- No **mutable default arguments** (`def f(items=[])`); use `None` and build inside.

```python
# wrong
def collect(items=[]):      # mutable default
    try:
        ...
    except:                 # bare except
        pass

# right
def collect(items: list | None = None) -> list:
    items = [] if items is None else items
    try:
        ...
    except ValueError as exc:
        raise
```

## Severity

Tag naming and documentation gaps `[LOW]`, missing error handling `[MEDIUM]`.
