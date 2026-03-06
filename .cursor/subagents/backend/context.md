# Backend — context

## Scope

- Services, endpoints, domain logic, data layer, server config.
- Do **not** own: UI or client code (frontend), pipelines or containers (devops).

## File patterns

- API routes: `**/routes/**`, `**/api/**`, `**/controllers/**`
- Services and domain: `**/services/**`, `**/domain/**`, `**/use-cases/**`
- Data access: `**/repositories/**`, `**/models/**`, `**/db/**`, `**/migrations/**`
- Server config: `**/server.*`, `**/app.*` (backend entry only)

## References

- See `../shared/coding-standards.md` and `../shared/architecture.md` for project-wide constraints.
