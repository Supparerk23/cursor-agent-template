# Agents, Sub-Agents, and Responsibilities

This repository defines a reusable **Cursor AI Agent + Sub-Agent architecture** for software engineering projects.

- **Agents** handle large, reasoning-heavy tasks: planning, orchestration, and calling sub-agents.
- **Sub-agents** handle repetitive, deterministic, narrow-scope work and return structured outputs.

Agents must never implement specialized logic directly if a corresponding sub-agent exists. Sub-agents must not modify unrelated files.

## Directory structure

```bash
.cursor/
├── agents/
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
├── subagents/
│   ├── test-writer.md
│   ├── security-reviewer.md
│   ├── refactor-agent.md
│   ├── doc-generator.md
│   └── migration-agent.md
└── global.json
```

Each file in `.cursor/agents/` describes an **orchestrating agent**.  
Each file in `.cursor/subagents/` describes a **deterministic executor** with explicit schemas.

---

## 3-layer agent architecture

### Strategic Layer

- **architect**
  - **Role**: System-level architecture and planning.
  - **Responsibilities**: Understand initiatives, define target architecture, break work into streams, choose which execution and quality agents to involve.
  - **When to use**: New major features, large refactors, cross-cutting concerns.
  - **Subagents called (via other agents)**: Primarily orchestrates other agents, may call `doc-generator` and `migration-agent` (sub-agent) for ADRs and migration artifacts.

### Execution Layer

- **backend-engineer**
  - **Role**: Backend implementation orchestrator.
  - **Responsibilities**: Plan backend API and business-logic changes, coordinate tests, refactors, migrations, and docs.
  - **When to use**: Backend features, bug fixes, and migrations.
  - **Subagents called**: `test-writer`, `refactor-agent`, `doc-generator`, `migration-agent`, `security-reviewer`.

- **frontend-engineer**
  - **Role**: Frontend/UI implementation orchestrator.
  - **Responsibilities**: Plan component, state, and layout changes; ensure a11y and performance; coordinate tests and docs.
  - **When to use**: UI features, layout/state changes, frontend bug fixes.
  - **Subagents called**: `test-writer`, `refactor-agent`, `doc-generator`, `security-reviewer` (for sensitive flows).

- **migration-agent**
  - **Role**: Migration orchestration agent.
  - **Responsibilities**: Plan schema/config/data migrations, define phases, coordinate scripts, tests, and runbooks.
  - **When to use**: DB/schema changes, API version migrations, framework upgrades requiring migrations.
  - **Subagents called**: `migration-agent`, `test-writer`, `doc-generator`, `security-reviewer`.

- **dependency-upgrader**
  - **Role**: Dependency upgrade orchestration agent.
  - **Responsibilities**: Plan dependency upgrades, batch and order changes, coordinate migrations, tests, and security checks.
  - **When to use**: Library/runtime/tool upgrades or periodic dependency hygiene.
  - **Subagents called**: `migration-agent`, `test-writer`, `security-reviewer`, `doc-generator`.

### Quality Layer

- **test-writer** (agent)
  - **Role**: Test strategy and orchestration agent.
  - **Responsibilities**: Decide coverage strategy and test types, coordinate deterministic test generation and documentation.
  - **When to use**: After features or fixes, when raising coverage, or as a gate in reviews/migrations.
  - **Subagents called**: `test-writer`, `doc-generator`.

- **code-reviewer**
  - **Role**: Structured code review orchestrator.
  - **Responsibilities**: Analyze diffs, coordinate security checks, test suggestions, localized refactors, and doc updates.
  - **When to use**: Reviewing pull requests or large diffs before merge.
  - **Subagents called**: `test-writer`, `security-reviewer`, `refactor-agent`, `doc-generator`.

- **security-auditor**
  - **Role**: Security assessment orchestrator.
  - **Responsibilities**: Scope security reviews, coordinate checklist-based security analysis, security tests, and security docs.
  - **When to use**: Auth/PII features, dependency or infra changes with security impact.
  - **Subagents called**: `security-reviewer`, `test-writer`, `doc-generator`.

- **bug-investigator**
  - **Role**: Bug investigation orchestrator.
  - **Responsibilities**: Turn bug reports into hypotheses, coordinate regression tests, refactors, and incident documentation.
  - **When to use**: Bugs, flaky tests, production incidents.
  - **Subagents called**: `test-writer`, `refactor-agent`, `doc-generator`.

- **performance-optimizer**
  - **Role**: Performance optimization orchestrator.
  - **Responsibilities**: Analyze performance metrics, identify hotspots, coordinate performance-focused refactors, benchmarks, and docs.
  - **When to use**: Slow endpoints, heavy UIs, performance regressions.
  - **Subagents called**: `refactor-agent`, `test-writer`, `doc-generator`, `security-reviewer` (if perf work touches sensitive paths).

---

## Sub-agent overview

Each sub-agent file in `.cursor/subagents/` follows the same strict schema:

```markdown
subagent: <name>

role:
  <short description>

input_schema:
  - name: <field>
    type: <type>
    required: <true|false>
    description: <what this field means>

output_schema:
  - name: <field>
    type: <type>
    description: <what this field contains>

rules:
  - <deterministic behavior rule>

constraints:
  - <hard constraint on scope / side effects>
```

The concrete definitions live in:

- `subagents/test-writer.md`
- `subagents/security-reviewer.md`
- `subagents/refactor-agent.md`
- `subagents/doc-generator.md`
- `subagents/migration-agent.md`

At a high level:

- **test-writer**
  - **Role**: Generate deterministic unit/integration/end-to-end tests for specific files and behaviors.
  - **Key constraints**: Only outputs test code and execution instructions; never modifies production code; scoped strictly to provided targets.

- **security-reviewer**
  - **Role**: Run a deterministic security checklist over specific code/config, producing evidence-backed findings and recommendations.
  - **Key constraints**: Only analyzes provided paths/snippets; no speculative vulnerabilities; no edits to code or config.

- **refactor-agent**
  - **Role**: Apply localized, behavior-preserving refactors to a single file/snippet under explicit constraints.
  - **Key constraints**: Must preserve behavior when requested; must not touch files outside the declared file; no cross-cutting architecture changes.

- **doc-generator**
  - **Role**: Generate markdown documentation (API docs, ADRs, readme sections, test plans) from provided code or diffs.
  - **Key constraints**: Only emits markdown; does not modify code; content must reflect observable code/diff, not speculation.

- **migration-agent**
  - **Role**: Generate ordered migration steps, scripts, rollback plans, and verification checks from current/target schema or config.
  - **Key constraints**: Changes derived solely from declared schema/config differences; scope-limited to migration files; no script execution.

---

## Behavior rules

- **Agents**:
  - Orchestrate tasks, not low-level execution.
  - Call sub-agents with explicit, typed inputs.
  - Combine sub-agent outputs into a final structured result (plans, packages, reviews).

- **Sub-agents**:
  - Operate deterministically for the same inputs.
  - Return structured outputs matching their `output_schema`.
  - Avoid speculative reasoning and never touch unrelated files or concerns.

See the individual files under `.cursor/agents/` and `.cursor/subagents/` for full, implementation-ready definitions.

