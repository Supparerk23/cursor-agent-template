# AI Engineering Team Structure and Orchestration

This repository defines a reusable **AI engineering team** built from Cursor agents and sub-agents.

- **Agents** act like senior engineers: they plan, orchestrate, and delegate.
- **Sub-agents** act like specialized tools: they execute deterministic, narrow-scope tasks with structured inputs and outputs.
- **Debug and observability** are first-class requirements: agent workflows must be inspectable, evidence-backed, and traceable from input to output.

Agents must **only orchestrate**. When a sub-agent exists, the agent must not implement that specialized logic directly.

Sub-agents must **only execute deterministically** within a declared scope and must not modify unrelated files.

---

## 1. Team structure (who does what)

### Strategic Layer (architecture and direction)

- **architect**
  - **Role**: Principal engineer / architect.
  - **Scope**: Requirements, system design, boundaries, and workstream planning.
  - **Output**: Architecture plans, work breakdown, rollout strategies.

### Execution Layer (feature and change delivery)

- **backend-engineer**
  - **Role**: Backend feature and bugfix orchestrator.
  - **Scope**: APIs, services, jobs, data access, backend migrations.

- **frontend-engineer**
  - **Role**: Frontend/UI feature and bugfix orchestrator.
  - **Scope**: Components, state, routing, accessibility, UX flows.

- **migration-agent**
  - **Role**: Schema/config/data migration orchestrator.
  - **Scope**: DB schemas, data migrations, config changes and sequencing.

- **dependency-upgrader**
  - **Role**: Dependency and runtime upgrade orchestrator.
  - **Scope**: Libraries, frameworks, tooling versions and their impacts.

- **refactor-agent**
 - **Role**: Refactor orchestration owner.
 - **Scope**: Multi-file maintainability refactors, cleanup sequencing, and behavior-preserving restructuring.

### Quality Layer (review, safety, and non-functional requirements)

- **test-writer** (agent)
  - **Role**: Test strategy owner.
  - **Scope**: Coverage planning, test types, and test-suite organization.

- **code-reviewer**
  - **Role**: Code review lead.
  - **Scope**: Correctness, maintainability, style, and architecture adherence.

- **security-auditor**
  - **Role**: Security lead.
  - **Scope**: Threat modeling, auth/crypto review, and security posture checks.

- **bug-investigator**
  - **Role**: Incident and bug triage lead.
  - **Scope**: Reproduction, root-cause analysis, and fix recommendations.

- **performance-optimizer**
  - **Role**: Performance lead.
  - **Scope**: Profiling, hotspot analysis, and performance improvement plans.

---

## 2. Orchestration rules

These rules apply to **all agents**:

- **No direct specialized work**: if a sub-agent exists for a task, the agent:
  - Describes the task.
  - Calls the appropriate sub-agent with **explicit input_schema fields**.
  - Consumes only the sub-agent’s **structured output_schema**.
- **No cross-scope edits**: an agent must only propose changes in the scope it was given.
- **Deterministic pipelines**:
  - Agents must call sub-agents in clear, ordered steps.
  - Dependencies between steps must be explicit (for example, “generate tests after refactor output is available”).
- **Structured outputs**:
  - Final agent outputs should be structured objects (plans, packages, reviews), not free-form prose alone.
- **Evidence discipline**:
  - Agents must distinguish confirmed facts from assumptions.
  - If key inputs are missing, agents must surface the gap explicitly instead of inventing details.
- **Traceability**:
  - Agents must preserve a clear link from request inputs to execution plans, sub-agent calls, findings, and final outputs.
  - Major conclusions should be attributable to either user-provided inputs or structured sub-agent outputs.
- **Observability**:
  - Agents must expose why they were selected, what execution plan they are following, which sub-agents were called, which input schemas were passed, and which outputs are expected or received.
- **Fail-closed behavior**:
  - When critical information is missing or contradictory, agents should report uncertainty and constrain recommendations rather than hallucinating a complete answer.
- **Separation of concerns**:
  - Planning and orchestration live in agents.
  - Execution (tests, refactors, docs, migrations, security checks) live in sub-agents.

---

## 3. Agent responsibilities (summary)

| Agent                  | Layer       | Primary responsibilities                                                                                   |
|------------------------|------------|------------------------------------------------------------------------------------------------------------|
| `architect`            | Strategic   | Define target architecture, boundaries, workstreams, and rollout plans.                                   |
| `backend-engineer`     | Execution   | Plan backend changes; orchestrate tests, refactors, migrations, docs, and security checks.                |
| `frontend-engineer`    | Execution   | Plan frontend/UI changes; orchestrate component refactors, tests, docs, and security checks.             |
| `migration-agent`      | Execution   | Plan migrations; orchestrate migration scripts, rollback, validation tests, and runbooks.                 |
| `dependency-upgrader`  | Execution   | Plan dependency upgrades; orchestrate necessary migrations, tests, security review, and docs.             |
| `refactor-agent`       | Execution   | Plan maintainability refactors; orchestrate localized refactors, regression tests, and refactor docs.     |
| `test-writer` (agent)  | Quality     | Define coverage strategy and test types; orchestrate deterministic test generation and test docs.         |
| `code-reviewer`        | Quality     | Review diffs; orchestrate security review, test suggestions, localized refactors, and docs.              |
| `security-auditor`     | Quality     | Plan and scope security reviews; orchestrate security checks, security tests, and security documentation. |
| `bug-investigator`     | Quality     | Analyze bugs; orchestrate regression tests, refactors, and incident documentation.                        |
| `performance-optimizer`| Quality     | Analyze performance; orchestrate performance refactors, benchmarks, and performance docs.                 |

Each agent file in `.cursor/agents/` expands these into:

- `role`
- `responsibilities`
- `when_to_use`
- `subagents_called`
- `workflow`
- `debug_capabilities`
- `architecture_constraints`

All agent files should also encode the following runtime observability contract:

- `agent selection reason`
- `execution plan`
- `sub-agent calls`
- `input schemas passed to sub-agents`
- `expected outputs`
- `evidence vs assumptions`
- `traceability back to inputs or sub-agent outputs`

---

## 4. Sub-agent usage (how execution happens)

Sub-agents live in `.cursor/subagents/` and follow a shared shape:

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

Concrete sub-agents:

- **test-writer** (`subagents/test-writer.md`)
  - **Used by**: `backend-engineer`, `frontend-engineer`, `migration-agent`, `dependency-upgrader`, `test-writer` (agent), `code-reviewer`, `security-auditor`, `bug-investigator`, `performance-optimizer`.
  - **Typical call**: provide `language`, `framework`, `target_files`, `behavior_description`, `coverage_constraints`.
  - **Output**: `test_code`, `coverage_target`, `edge_cases`, `execution_instructions`.

- **security-reviewer** (`subagents/security-reviewer.md`)
  - **Used by**: `backend-engineer`, `frontend-engineer`, `migration-agent`, `dependency-upgrader`, `code-reviewer`, `security-auditor`, `performance-optimizer`.
  - **Typical call**: provide `code_paths` or `code_snippets`, `threat_context`, `data_sensitivity`, optional `dependency_list`, `focus_areas`.
  - **Output**: `findings`, `risk_summary`, `verification_steps`.

- **refactor-agent** (`subagents/refactor-agent.md`)
  - **Used by**: `backend-engineer`, `frontend-engineer`, `refactor-agent` (agent), `code-reviewer`, `bug-investigator`, `performance-optimizer`.
  - **Typical call**: provide `current_code`, `refactor_goal`, `file_path`, `constraints`, optional `context_snippets`.
  - **Output**: `refactored_code`, `change_summary`, `risk_notes`, `invariants`.

- **doc-generator** (`subagents/doc-generator.md`)
  - **Used by**: almost all agents when they need docs (for example, `architect`, `backend-engineer`, `frontend-engineer`, `migration-agent`, `dependency-upgrader`, `test-writer`, `code-reviewer`, `security-auditor`, `bug-investigator`, `performance-optimizer`).
  - **Typical call**: provide `code_or_diff`, `documentation_type`, `audience`, optional `constraints` and `file_path`.
  - **Output**: `documentation_markdown`, `sections`, `assumptions`.

- **migration-agent** (`subagents/migration-agent.md`)
  - **Used by**: `migration-agent` (agent), `dependency-upgrader`, `architect` (indirectly).
  - **Typical call**: provide `from_version`, `to_version`, `current_schema_or_config`, `target_state_description`, optional `environment_constraints`, `scope_files`.
  - **Output**: `migration_steps`, `migration_scripts`, `rollback_plan`, `verification_checks`.

All sub-agents:

- Must obey their own `rules` and `constraints`.
- Must never operate outside the supplied scope (for example, `scope_files`, `file_path`, `code_paths`).
- Must return fully structured outputs with no hidden behavior.

---

## 5. Routing layer (how tasks reach the right agent)

The **routing layer** decides which **agent** to invoke first, based on the task description, and that agent then routes to the correct **sub-agents**.

### High-level routing

- **Architecture / big-picture design**
  - Route to: `architect`.
- **Backend feature, bugfix, or endpoint work**
  - Route to: `backend-engineer`.
- **Frontend/UI feature or bugfix**
  - Route to: `frontend-engineer`.
- **Schema/data/config migration**
  - Route to: `migration-agent`.
- **Dependency or runtime upgrade**
  - Route to: `dependency-upgrader`.
- **Maintainability refactor or multi-file cleanup**
 - Route to: `refactor-agent`.
- **Test strategy or coverage increase**
  - Route to: `test-writer` (agent).
- **Code review of a diff or PR**
  - Route to: `code-reviewer`.
- **Security review**
  - Route to: `security-auditor`.
- **Bug / incident investigation**
  - Route to: `bug-investigator`.
- **Performance issue**
  - Route to: `performance-optimizer`.

### Intra-agent routing (agent → sub-agents)

Within an agent:

- The agent:
  - Parses the task into smaller, deterministic sub-tasks.
  - For each sub-task, chooses a sub-agent based on:
    - **Type of work** (tests, refactor, docs, migration, security).
    - **Scope** (files, functions, schemas).
  - Calls the sub-agent with a **fully-populated input_schema** object.
- The agent then:
  - Collects all `output_schema` objects from sub-agents.
  - Validates that outputs are consistent with the plan and scope.
  - Validates that conclusions remain traceable to explicit inputs or sub-agent outputs.
  - Produces a final **structured output** for the user or next step in the pipeline.

This routing pattern ensures:

- Clear ownership: strategic vs execution vs quality.
- Deterministic execution: all specialized work goes through sub-agents with explicit schemas.
- Composability: new agents or sub-agents can be added without breaking existing ones, as long as inputs and outputs stay structured.


