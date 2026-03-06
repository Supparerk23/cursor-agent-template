# Tests — rules

- One logical concern per test; descriptive test names that read like specs.
- Prefer stable, fast tests; avoid time-dependent or non-deterministic behavior.
- Use project test framework and conventions; do not introduce a second framework without agreement.
- Mock external I/O (HTTP, DB, filesystem) at boundaries; avoid mocking implementation details.
- Keep setup/teardown minimal; use beforeEach/afterEach only when necessary.
- Do not commit skipped or disabled tests without a clear reason and ticket reference.
- Do not rewrite production code except for testability (e.g. inject dependencies); hand off bugs to frontend/backend.
