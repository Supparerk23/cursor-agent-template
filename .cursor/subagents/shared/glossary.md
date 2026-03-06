# Glossary (shared)

Canonical terms used across subagents. Use these consistently in code, tests, and docs.

| Term | Definition |
|------|------------|
| **API** | Contract between frontend and backend; REST or GraphQL endpoints. |
| **Component** | Reusable UI unit (React/Vue/Svelte component). |
| **Controller** | Entry point for an API request; parses input, calls service, returns response. |
| **Service** | Layer that holds business logic; called by controllers, uses repositories. |
| **Repository** | Data access layer; CRUD and queries, no business rules. |
| **Pipeline** | CI/CD workflow (build, test, deploy). |
| **Env / Environment** | Deployment target (e.g. dev, staging, prod) and its config. |
| **Secret** | Credential or key; must not be committed; use env or vault. |
| **a11y** | Accessibility; keyboard, screen reader, and focus support. |

Add project-specific terms below as the codebase grows.

---

*Project-specific terms:*

- (none yet)
