# Coding standards (shared)

Project-wide standards applied by all subagents. Override only when a subagent’s `rules.md` explicitly allows.

## General

- **Naming**: Use consistent naming: `camelCase` for variables/functions, `PascalCase` for types/classes, `kebab-case` for files and URLs where the stack expects it.
- **Formatting**: Follow project formatter (Prettier, Black, gofmt, etc.); no ad-hoc style.
- **Imports**: Group and order imports per project convention; remove unused imports.
- **Comments**: Comment why, not what; keep comments up to date when code changes.
- **Errors**: Handle errors explicitly; do not swallow or ignore without a documented reason.
- **Secrets**: Never commit secrets, API keys, or credentials; use env vars or a secret manager.

## By area

- **Frontend**: Functional components, semantic HTML, a11y by default.
- **Backend**: Thin controllers, business logic in services, clear API contracts.
- **Tests**: One concern per test, stable and fast; mock at boundaries.
- **DevOps**: Idempotent scripts, no secrets in repo, clear pipeline failures.
- **Docs**: Scannable structure, accurate code samples, single source of truth for architecture/glossary.
- **Security**: Report by severity; recommend fixes; do not change code unless asked.

## References

- Glossary: `./glossary.md`
- Architecture: `./architecture.md`
