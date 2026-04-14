# cursor-agent-template

A simple template for building a multi-agent Cursor setup inside any software project.

This repo separates work into two layers:

- **Agents** plan and coordinate work.
- **Sub-agents** do narrow, deterministic tasks with explicit inputs and outputs.

If you want the full architecture and all rules, read `AGENTS.md`.

## What is included

```bash
.cursor/
├── agents/       # High-level agents
├── subagents/    # Specialized executors
└── global.json   # Optional Cursor config
```

### Agents

Agents are the top-level decision makers. They choose the right workflow, call sub-agents, and combine the results.

Examples:

- `architect`
- `backend-engineer`
- `frontend-engineer`
- `code-reviewer`
- `security-auditor`
- `bug-investigator`
- `performance-optimizer`

### Sub-agents

Sub-agents are smaller and more predictable. They are used for specific tasks like:

- writing tests
- reviewing security concerns
- generating docs
- planning migrations
- producing localized refactors

## How it works

The rule of this template is simple:

- Agents orchestrate.
- Sub-agents execute.

That means an agent should not directly do specialized work when a matching sub-agent already exists.

This template also expects agents to be:

- deterministic
- traceable
- explicit about assumptions
- clear about why they chose a workflow

## How to use it

Copy the `.cursor` folder into your project:

```bash
git clone --depth 1 --filter=blob:none --sparse git@github.com:Supparerk23/cursor-agent-template.git
cd cursor-agent-template
git sparse-checkout set .cursor
cp -r .cursor ../your-project/
```

Then adjust the files for your repo:

- edit `.cursor/agents/` to change orchestration behavior
- edit `.cursor/subagents/` to change execution schemas or constraints
- edit `AGENTS.md` to document your architecture

## Good starting prompts

Ask Cursor things like:

```text
Which agents are defined in this repository?
List them and explain when each one should be used.
```

```text
Use the backend-engineer agent from this repository.
Plan a new API endpoint and call the relevant sub-agents.
```

```text
Use the code-reviewer agent defined in this repository.
Review this change and explain the findings with traceable evidence.
```

## Files to read first

- `README.md` for the quick overview
- `AGENTS.md` for the full system design
- `.cursor/agents/` for agent behavior
- `.cursor/subagents/` for execution contracts

