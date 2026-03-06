# Subagents and responsibilities

Agent structure lives under `.cursor/`. The canonical subagent definitions are in `.cursor/subagents/`.

## Structure

```
.cursor/
├── subagents/
│   ├── frontend/
│   │   ├── instructions.md
│   │   ├── context.md
│   │   ├── rules.md
│   │   └── tools.json
│   ├── backend/
│   │   ├── instructions.md
│   │   ├── context.md
│   │   ├── rules.md
│   │   └── tools.json
│   ├── tests/
│   │   ├── instructions.md
│   │   ├── context.md
│   │   └── rules.md
│   ├── devops/
│   │   ├── instructions.md
│   │   ├── context.md
│   │   ├── rules.md
│   │   └── tools.json
│   ├── docs/
│   │   ├── instructions.md
│   │   ├── context.md
│   │   ├── rules.md
│   │   └── tools.json
│   ├── security/
│   │   ├── instructions.md
│   │   ├── context.md
│   │   ├── rules.md
│   │   └── tools.json
│   └── shared/
│       ├── coding-standards.md
│       ├── architecture.md
│       └── glossary.md
└── global.json
```

## Subagent roles

| Subagent   | Responsibility           | When to use |
|-----------|---------------------------|-------------|
| **frontend** | UI code and patterns       | Components, layouts, styling, a11y, client-side behavior |
| **backend**  | API and domain logic       | Endpoints, services, business rules, data access |
| **tests**    | Unit/integration testing   | Test coverage, fixing failing tests, mocks/fixtures |
| **devops**   | CI/CD, infrastructure     | Pipelines, containers, deployment, env config |
| **docs**     | Documentation             | README, API docs, runbooks, architecture notes |
| **security** | Audit and policy          | Security review, auth, secrets, dependencies, compliance |

## Per-subagent files

- **instructions.md** — Role, “when invoked” steps, and what to deliver.
- **context.md** — Scope, file patterns, and pointers to shared docs.
- **rules.md** — Concrete do’s and don’ts for that subagent.
- **tools.json** — (Optional) Preferred or restricted tools and relevant globs. Omitted for `tests` in this template.

## Shared

- **coding-standards.md** — Project-wide style and practices.
- **architecture.md** — High-level structure, boundaries, deployment.
- **glossary.md** — Canonical terms used in code and docs.

## Global config

- **global.json** — Lists subagents, paths, and default options (e.g. `readonly` for security). Your tooling can read this to discover and invoke subagents.

## Customization

- Edit any `instructions.md`, `context.md`, or `rules.md` under `.cursor/subagents/<name>/`.
- Add project-specific terms and architecture in `shared/glossary.md` and `shared/architecture.md`.
- Adjust `tools.json` per subagent if your runner respects it.
