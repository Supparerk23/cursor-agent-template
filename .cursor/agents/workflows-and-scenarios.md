# Agent Workflows and Usage Scenarios

This file documents example execution workflows and a concrete usage scenario that demonstrate how agents orchestrate deterministic sub-agents.

## Architect-driven strategic workflow

1. The `architect` agent receives a high-level feature request.
2. The `architect` agent analyzes the existing architecture and identifies impacted domains.
3. The `architect` agent delegates:
   - Backend work to the `backend-engineer` agent with backend-specific scope and constraints.
   - Frontend work to the `frontend-engineer` agent with UI-specific scope and constraints.
   - Migration planning to the `migration-agent` agent, if schemas or configs must change.
   - Dependency considerations to the `dependency-upgrader` agent, if upgrades are required.
4. Each execution agent:
   - Plans its changes.
   - Calls relevant sub-agents (`test-writer`, `refactor-agent`, `doc-generator`, `migration-agent`, `security-reviewer`) for deterministic tasks.
5. The `architect` agent collects the resulting backend, frontend, migration, and dependency plans and consolidates them into a single architecture and rollout plan.

## Execution-layer implementation workflow (backend feature)

1. The `backend-engineer` agent receives a backend feature description and scope files.
2. The `backend-engineer` agent:
   - Analyzes existing backend design and data flows.
   - Produces an ordered backend implementation plan.
3. The `backend-engineer` agent orchestrates sub-agents:
   - Calls the `test-writer` sub-agent with language, framework, target_files, and behavior_description to generate unit and integration tests.
   - Calls the `doc-generator` sub-agent with code_or_diff and documentation_type to create API documentation.
   - Optionally calls the `security-reviewer` sub-agent for sensitive endpoints.
   - Optionally calls the `refactor-agent` sub-agent to simplify or prepare code for the change.
4. The `backend-engineer` agent aggregates:
   - Suggested patches from refactor and migration sub-agents.
   - Generated tests and execution instructions from the test-writer sub-agent.
   - Documentation from the doc-generator sub-agent.
5. The `backend-engineer` agent outputs a backend change package that includes the implementation plan, suggested patches, tests, and documentation updates.

## Quality-layer verification workflow (code review and security)

1. The `code-reviewer` agent receives a diff or pull request description.
2. The `code-reviewer` agent:
   - Classifies changes by area (backend, frontend, tests, docs, config).
   - Identifies risky or complex regions.
3. The `code-reviewer` agent orchestrates sub-agents:
   - Calls the `security-reviewer` sub-agent with code_paths or code_snippets for security-sensitive sections.
   - Calls the `test-writer` sub-agent to generate or suggest missing tests.
   - Calls the `refactor-agent` sub-agent where local cleanup or simplification is beneficial.
   - Calls the `doc-generator` sub-agent for missing or outdated docs.
4. The `code-reviewer` agent aggregates:
   - Security findings and recommendations.
   - New or updated tests.
   - Local refactor proposals.
   - Documentation updates.
5. The `code-reviewer` agent produces a structured review report with blocking issues, non-blocking suggestions, and links to concrete sub-agent outputs.

## Example usage scenario: full-stack feature

1. A team adds this template to a repository under `.cursor/`.
2. A new feature touches both backend APIs and frontend UI:
   - The user first invokes the `architect` agent with the high-level feature description and constraints.
   - The `architect` agent designs a high-level plan and delegates:
     - Backend tasks to the `backend-engineer` agent.
     - Frontend tasks to the `frontend-engineer` agent.
3. The `backend-engineer` agent:
   - Plans backend changes.
   - Calls the `test-writer` sub-agent to create backend unit and integration tests.
   - Calls the `doc-generator` sub-agent for API documentation.
   - Optionally calls the `security-reviewer` sub-agent and the `refactor-agent` sub-agent.
4. The `frontend-engineer` agent:
   - Plans component and state updates.
   - Calls the `test-writer` sub-agent to create component and end-to-end tests.
   - Calls the `doc-generator` sub-agent to produce UI usage notes or story documentation.
5. After implementation, the team runs quality agents:
   - The `test-writer` agent to ensure coverage is sufficient and call the test-writer sub-agent if more tests are needed.
   - The `security-auditor` agent to coordinate a focused security review for the new feature.
   - The `performance-optimizer` agent if the feature introduces potential performance risks.
6. Finally, the `code-reviewer` agent is invoked on the combined diff to:
   - Call sub-agents for security, tests, refactors, and docs.
   - Produce a structured review report to gate the merge.

