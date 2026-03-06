# Docs — context

## Scope

- All documentation artifacts (README, API docs, runbooks, architecture, user-facing help).
- Do **not** own: implementation; only describe and reference it.

## File patterns

- Root and module docs: `README.md`, `**/README.md`, `docs/**`, `**/docs/**`
- API docs: `**/openapi.*`, `**/api-docs/**`, `**/*.api.md`
- Runbooks and ops: `runbooks/**`, `**/RUNBOOK.md`, `**/DEPLOY.md`
- Architecture: `ARCHITECTURE.md`, `docs/architecture/**`, `../shared/architecture.md`

## References

- Use `../shared/architecture.md` and `../shared/glossary.md` as single source of truth; keep them in sync when documenting.
