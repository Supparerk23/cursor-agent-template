# cursor-agent-template

Template for a **Cursor AI Agent + Sub-Agent architecture** that you can drop into any software engineering repository.

- Agents handle large, reasoning-heavy tasks (planning, orchestration, coordination).
- Sub-agents handle deterministic, narrow-scope execution with strict schemas.

See `AGENTS.md` for the full architecture and roles.

## Structure

```bash
.cursor/
├── agents/                  # Orchestrating agents (strategic, execution, quality)
│   ├── backend-engineer.md
│   ├── frontend-engineer.md
│   ├── architect.md
│   ├── code-reviewer.md
│   ├── bug-investigator.md
│   ├── security-auditor.md
│   ├── refactor-agent.md
│   ├── migration-agent.md
│   ├── performance-optimizer.md
│   ├── dependency-upgrader.md
│   └── test-writer.md
├── subagents/               # Deterministic executors with explicit schemas
│   ├── test-writer.md
│   ├── security-reviewer.md
│   ├── refactor-agent.md
│   ├── doc-generator.md
│   └── migration-agent.md
└── global.json              # (Optional) global discovery and configuration
```

Each agent file describes:

- **role**: what kind of work it orchestrates.
- **responsibilities**: what it plans and coordinates.
- **when_to_use**: clear triggers for choosing this agent.
- **subagents_called**: which sub-agents it uses.
- **workflow**: how it plans, calls sub-agents, and aggregates results.

Each sub-agent file defines:

- **subagent** and **role**.
- **input_schema** and **output_schema** (explicit fields and types).
- **rules** for deterministic behavior.
- **constraints** that restrict scope and side effects.

## Usage

- **Clone template and copy into a new project**:

  ```bash
  # clone only the .cursor folder from the template repo
  git clone --depth 1 --filter=blob:none --sparse git@github.com:Supparerk23/cursor-agent-template.git
  cd cursor-agent-template
  git sparse-checkout set .cursor

  # copy the .cursor config into your project
  cp -r .cursor ../new-project/
  ```

- **After copying, typically only modify**:
  - Individual agent files in `.cursor/agents/` if you want different orchestration rules for your project.
  - Sub-agent files in `.cursor/subagents/` if you need to adjust schemas or constraints for your environment.
  - Optionally, `.cursor/global.json` if you decide to use it for agent discovery or additional configuration.

Keep the overall separation intact:

- Agents orchestrate and never directly implement specialized logic if a sub-agent exists.
- Sub-agents execute deterministically within a clearly declared scope and return structured outputs.

## Reference

- `AGENTS.md` — Full layout, roles, and how the 3-layer architecture (strategic, execution, quality) is organized.

