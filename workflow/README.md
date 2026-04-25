# workflow

A multi-agent development workflow that takes you from idea to shipped code. An **Orchestrator** coordinates the conversation while specialised agents handle planning, implementation, research, and exploration autonomously.

## What's included

The workflow ships as two independently installable **capability modules**:

| Module | Path | Description |
|--------|------|-------------|
| **core** | `workflow/core` | Orchestrator, Developer, Planner, Explore, and Researcher agents plus all workflow skills (refine, to-prd, to-issues, developer-loop, researcher-loop, QA, architecture improvement) |
| **ado** | `workflow/ado` | Azure DevOps integration — backlog item creation, ADO MCP tool reference, and templates |

Install **core** for the base workflow. Add **ado** when your team tracks work in Azure DevOps.

## Quick start

### 1. Install the APM CLI

#### Linux / macOS

```bash
curl -sSL https://aka.ms/apm-unix | sh
```

#### Windows

```powershell
irm https://aka.ms/apm-windows | iex
```

### 2. Install the modules

#### Option A — Install locally (per-repo)

```shell
# Core workflow (required)
apm install BNGBank/agentic/workflow/core

# Azure DevOps integration (optional)
apm install BNGBank/agentic/workflow/ado
```

#### Option B — Install globally (available in every repo)

> Note you may have to install the MCP servers manually if you use global installation. See the `.mcp.json` files in `workflow/core/` and `workflow/ado/` for the server declarations to add to your global MCP config.

```shell
# Core workflow (required)
apm install BNGBank/agentic/workflow/core -g

# Azure DevOps integration (optional)
apm install BNGBank/agentic/workflow/ado -g
```

### 3. MCP servers

Each module ships a `.mcp.json` declaring the MCP servers it needs. APM handles installation. If your environment requires manual setup, see the `.mcp.json` files in `workflow/core/` and `workflow/ado/` for the server declarations to add to your `.vscode/mcp.json`.

| Module | Servers |
|--------|--------|
| **core** | GitHub MCP, Context7, Microsoft Learn |
| **ado** | Azure DevOps MCP |

### 4. Configure your repository

Run the **/init-workflow** skill to set up the required configuration files. It auto-detects your git remote and platform, interviews you for team-specific values, and scaffolds the files automatically:

- `AGENTS.md` (or appends to `.github/copilot-instructions.md` if it exists) — platform context, terminology, agent interaction rules
- `AZURE_DEVOPS.md` (ADO teams only) — project, team, area paths, iterations, epics, features, stakeholders

On subsequent runs (e.g., after a package upgrade), the **/init-workflow** skill merges in any new template sections without overwriting your existing content.

<details>
<summary>Manual setup</summary>

#### AGENTS.md

Every repo using the workflow needs an `AGENTS.md` at the root (or equivalent content in `.github/copilot-instructions.md`). This tells agents about your platform, terminology, and interaction preferences. Example:

```markdown
# AGENTS.md

## Platform Context

This repository uses Azure DevOps for work item tracking and code reviews.

Remote URL: https://dev.azure.com/<org>/<project>/_git/<repo>

## Terminology

| Abstract Term       | Azure DevOps         | GitHub            |
| ------------------- | -------------------- | ----------------- |
| Work item           | Product Backlog Item | Issue             |
| Sub-work item       | Task                 | Sub-issue         |
| Bug                 | Bug                  | Issue (bug label) |
| Work item reference | #<ID>                | #<ID>             |
| Code review         | Pull Request         | Pull Request      |

## Platform-specific Skills (ADO)

When performing these operations, prefer the following skills if available:

- Creating work items → use `create-backlog-item` skill

Always read `AZURE_DEVOPS.md` for context before using Azure DevOps tools.

## Agent Interaction

- Before using the Ask Questions tool, provide relevant context and reasoning so the user can make an informed choice.
- Include recommended answers in question options whenever possible.
- Before asking the user, try to answer the question by exploring the codebase or consulting documentation.
- Keep chat messages brief and to the point.
```

#### AZURE_DEVOPS.md (ADO teams only)

If you installed the **ado** module, create an `AZURE_DEVOPS.md` at the repo root with your team-specific configuration:

```markdown
# Azure DevOps Team Configuration

## Project

- **Project:** `YourProject`
- **Team name:** `Your Team`

## Repository

- **Name:** `your-repo`
- **ID:** `00000000-0000-0000-0000-000000000000`

## Area Paths

| Area Path | Description |
|---|---|
| `YourProject\Your Team` | Team-managed features |

## Default Iteration

`YourProject\Your Team`

## Key Epics

| Epic | ID | Description |
|---|---|---|
| My Epic | 12345 | Description of the epic |

## Key Features

| Feature | ID | Description |
|---|---|---|
| My Feature | 67890 | Description of the feature |

## Stakeholders

| Stakeholder | Description |
|---|---|
| Your Team | Description of your team |
```

</details>

## Agents

| Agent | Role |
|-------|------|
| **Orchestrator** | Primary coordinator — guides the workflow, interviews the user, delegates to other agents |
| **Developer** | Autonomous implementation — picks up a task, writes code, validates, commits |
| **Planner** | Reads work items and produces an ordered plan with dependencies |
| **Explore** | Read-only codebase explorer — maps files, symbols, and patterns relevant to a task |
| **Researcher** | Technical research specialist — gathers information from docs and web sources |

## Workflow

The standard development lifecycle flows through these phases:

```
1. refine             →  Interview about an idea until shared understanding
2. to-prd             →  Synthesize a PRD from the conversation context
3. to-issues          →  Break the PRD into vertical-slice work items
4. developer-loop     →  Hand work items to Developer for autonomous coding
   — or researcher-loop  →  Hand research items to Researcher
5. qa                 →  Interactive QA session — file new bugs as work items
   — or improve-codebase-architecture  →  Find architectural improvements
6. developer-loop     →  Implement new work items from QA / architecture review
7. Repeat 5–6 as needed
```

Start by opening the **dev** prompt (or **dev-max** / **dev-sonnet** for model variants) and describing what you want to build. The Orchestrator will guide you from there.

## Skills reference

### Core skills (`workflow/core`)

| Skill | Description |
|-------|-------------|
| **refine** | Relentless interview to reach shared understanding on an idea or design |
| **to-prd** | Synthesize a PRD from conversation context and submit as a work item |
| **to-issues** | Break a PRD into independently-grabbable vertical-slice work items |
| **create-plan** | Read work items and produce an ordered implementation plan with dependencies |
| **developer-loop** | Run Developer in a loop to complete all tasks under a work item |
| **researcher-loop** | Run Researcher in a loop to complete all research subtasks |
| **qa** | Interactive QA session — user reports bugs, agent files work items |
| **improve-codebase-architecture** | Explore for architectural friction and propose module-deepening refactors |
| **init** | Initialize or update `AGENTS.md` and `AZURE_DEVOPS.md` configuration files |

### ADO skills (`workflow/ado`)

| Skill | Description |
|-------|-------------|
| **create-backlog-item** | Create Features, PBIs, and Bugs in Azure DevOps with templates and parent linking |
| **azure-devops-tools** | Fallback reference for all ADO MCP tool schemas (used only when dedicated skills are insufficient) |

## Prompts

Three prompt entry points are provided, differing only in model selection:

| Prompt | Use when |
|--------|----------|
| **dev** | Default — best balance of capability and speed |
| **dev-max** | Maximum capability for complex tasks |
| **dev-sonnet** | Faster, lighter tasks |

All three launch the Orchestrator agent.
