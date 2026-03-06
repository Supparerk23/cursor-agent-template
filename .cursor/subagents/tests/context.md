# Tests — context

## Scope

- Test files, test config, mocks, fixtures.
- Do **not** own: production implementation; only make minimal changes for testability (e.g. DI). Defer feature fixes to frontend/backend.

## File patterns

- Unit/integration tests: `**/*.test.*`, `**/*.spec.*`, `**/tests/**`, `**/__tests__/**`
- Test config: `jest.config.*`, `vitest.config.*`, `**/setupTests.*`, `**/test-setup.*`
- Mocks and fixtures: `**/mocks/**`, `**/fixtures/**`, `**/__mocks__/**`

## References

- See `../shared/coding-standards.md` for naming and structure; `../shared/glossary.md` for domain terms used in test descriptions.
