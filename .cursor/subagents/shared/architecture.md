# Architecture (shared)

Single source of truth for high-level structure. All subagents should align with this when making or documenting decisions.

## Overview

- **Frontend**: UI layer; consumes APIs and renders state. No direct DB or secret access.
- **Backend**: API and domain layer; business logic, persistence, integrations. Exposes APIs only; no UI.
- **Infrastructure**: CI/CD, containers, environments. Builds and deploys app; does not implement app logic.
- **Data flow**: Client → API → services → repositories → data store. No client-to-DB direct access.

## Boundaries

- **Frontend ↔ Backend**: Contract via API (REST/GraphQL). Version and document breaking changes.
- **Backend ↔ Data**: Use repositories or data layer; no raw SQL or ORM in controllers.
- **App ↔ Infra**: Config via env; secrets from vault or CI secrets, never in repo.
- **Trust**: Validate at boundaries; assume internal services are in same trust domain unless stated otherwise.

## Deployment

- Describe here: number of environments (dev/staging/prod), where they run, and how config and secrets are supplied.
- Example: “Single Docker image; env-specific config via env vars; secrets from Vault.”

## References

- Coding standards: `./coding-standards.md`
- Glossary: `./glossary.md`
