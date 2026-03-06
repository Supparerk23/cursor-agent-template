# cursor-agent-template

Full structure of Cursor agent: subagents, shared context, and global config.

## Structure

```
.cursor/
├── subagents/
│   ├── frontend/    # UI code and patterns
│   ├── backend/     # API and domain logic
│   ├── tests/       # Unit/integration testing
│   ├── devops/      # CI/CD, infrastructure
│   ├── docs/        # Documentation
│   ├── security/    # Audit and policy
│   └── shared/      # coding-standards, architecture, glossary
└── global.json
```

Each subagent (except tests in this template) has: `instructions.md`, `context.md`, `rules.md`, and optionally `tools.json`. Shared docs live in `subagents/shared/`.

## Reference

- [AGENTS.md](AGENTS.md) — Full layout, roles, and how to customize.
