# workflow

A multi-agent development workflow that takes you from idea to shipped code. An **Orchestrator** coordinates the conversation while specialised agents handle planning, implementation, research, and exploration autonomously.

Some skills are heavily inspired by https://github.com/mattpocock/skills.

## What's included

The workflow ships as two independently installable **capability modules**:

| Module | Path | Description |
| --- | --- | --- |
| **core** | `workflow/core` | Orchestrator, Developer, Planner, Explore, and Researcher agents plus all workflow skills (init-workflow, refine, to-prd, to-issues, developer-loop, researcher-loop, QA, architecture improvement, gh-tools) |
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

### 2. Install the workflow modules

```shell
# Core workflow (required)
apm install BNGBank/agentic/workflow/core

# Azure DevOps integration (optional)
apm install BNGBank/agentic/workflow/ado
```

> You can update the modules at any time by running `apm deps update`. Add `-g` to update global installations.

> Add `-g` to install globally for all repositories (e.g. `apm install -g BNGBank/agentic/workflow/core`). Global installs do not include prompts and MCP servers — those remain repo-scoped.

### 3. Run `init-workflow`

> After installing the modules, run the `/init-workflow` skill. It interviews you for team-specific values, scaffolds `AGENTS.md` and (for ADO teams) `AZURE_DEVOPS.md`, and offers to install the GitHub MCP server and/or `gh` CLI in your workspace and devcontainer.
>
> On subsequent runs (e.g., after a package upgrade) it merges in any new template sections without overwriting existing content.

<details>
<summary>Manual GitHub MCP / CLI setup</summary>

If `init-workflow` cannot manage your environment (e.g. no devcontainer, restricted shell), you can install GitHub access manually.

GitHub users may use either the **GitHub MCP server** (`io.github.github/github-mcp-server`, added by the consumer to `.vscode/mcp.json`) or the `gh` **CLI** for GitHub operations — at least one is required. ADO-only teams can skip this step entirely.

#### Windows

```powershell
winget install --id GitHub.cli
```

#### Linux / devcontainer

Add the GitHub CLI as a [devcontainer feature](https://containers.dev/features) in your `devcontainer.json`:

```jsonc
{
  "features": {
    "ghcr.io/devcontainers/features/github-cli:1": {},
  },
}
```

This works on any base image. For non-devcontainer Linux environments, see [CLI install instructions](https://github.com/cli/cli/blob/trunk/docs/install_linux.md).

After installation, authenticate with `gh auth login`.

</details>

<details>
<summary>Manual MCP server configuration</summary>

Each module ships a `.mcp.json` declaring the MCP servers it needs. APM handles installation. If your environment requires manual setup, see the `.mcp.json` files in `workflow/core/` and `workflow/ado/` for the server declarations to add to your `.vscode/mcp.json`.

| Module   | Servers                   |
| -------- | ------------------------- |
| **core** | Context7, Microsoft Learn |
| **ado**  | Azure DevOps MCP          |

> **Note:** `core` does **not** bundle the GitHub MCP server — it would force-load on ADO-only consumers. GitHub users who want MCP transport must add `io.github.github/github-mcp-server` to their own `.vscode/mcp.json`. Otherwise the `gh-tools` skill falls back to the `gh` CLI.

</details>

<details>
<summary>Manual AGENTS.md / AZURE_DEVOPS.md setup</summary>

#### AGENTS.md

Every repo using the workflow needs an `AGENTS.md` at the root (or equivalent content in `.github/copilot-instructions.md`). This tells agents about your platform, terminology, and interaction preferences. Example:

```markdown
# AGENTS.md

## Platform Context

This repository uses Azure DevOps for work item tracking and code reviews.

Remote URL: https://dev.azure.com/<org>/<project>/\_git/<repo>

## Terminology

| Abstract Term       | Azure DevOps         | GitHub            |
| ------------------- | -------------------- | ----------------- |
| Work item           | Product Backlog Item | Issue             |
| Sub-work item       | Task                 | Sub-issue         |
| Bug                 | Bug                  | Issue (bug label) |
| Work item reference | #<ID>                | #<ID>             |
| Code review         | Pull Request         | Pull Request      |

## Platform Skills

Prefer skills prefixed with the platform name in use:

- **ADO** — `ado-create-backlog-item` (create work items), `ado-revert-work-item` (restore previous version), `ado-tools` (general MCP tool reference).
- **GitHub** — `gh-tools` (issues, sub-issues, comments, search; prefers MCP, falls back to gh CLI).

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

| Area Path               | Description           |
| ----------------------- | --------------------- |
| `YourProject\Your Team` | Team-managed features |

## Default Iteration

`YourProject\Your Team`

## Key Epics

| Epic    | ID    | Description             |
| ------- | ----- | ----------------------- |
| My Epic | 12345 | Description of the epic |

## Key Features

| Feature    | ID    | Description                |
| ---------- | ----- | -------------------------- |
| My Feature | 67890 | Description of the feature |

## Stakeholders

| Stakeholder | Description              |
| ----------- | ------------------------ |
| Your Team   | Description of your team |
```

</details>

## Agents

| Agent | Role |
| --- | --- |
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
| --- | --- |
| **refine** | Relentless interview to reach shared understanding on an idea or design |
| **to-prd** | Synthesize a PRD from conversation context and submit as a work item |
| **to-issues** | Break a PRD into independently-grabbable vertical-slice work items |
| **create-plan** | Read work items and produce an ordered implementation plan with dependencies |
| **developer-loop** | Run Developer in a loop to complete all tasks under a work item |
| **researcher-loop** | Run Researcher in a loop to complete all research subtasks |
| **qa** | Interactive QA session — user reports bugs, agent files work items |
| **improve-codebase-architecture** | Explore for architectural friction and propose module-deepening refactors |
| **init-workflow** | Initialize or update `AGENTS.md` and `AZURE_DEVOPS.md`, and optionally wire up the GitHub MCP server / `gh` CLI in your workspace and devcontainer |
| **gh-tools** | Lean GitHub operations (issues, sub-issues, view, list, search) — ships with **core** for general GH use, prefers MCP and falls back to `gh` CLI |

### ADO skills (`workflow/ado`)

| Skill | Description |
| --- | --- |
| **ado-create-backlog-item** | Create Features, PBIs, and Bugs in Azure DevOps with templates and parent linking |
| **ado-revert-work-item** | Restore an Azure DevOps work item to a previous revision from its history |
| **ado-tools** | Fallback reference for all ADO MCP tool schemas (used only when dedicated skills are insufficient) |

## Prompts

Three prompt entry points are provided, differing only in model selection:

| Prompt         | Use when                                       |
| -------------- | ---------------------------------------------- |
| **dev**        | Default — best balance of capability and speed |
| **dev-max**    | Maximum capability for complex tasks           |
| **dev-sonnet** | Faster, lighter tasks                          |

All three launch the Orchestrator agent.
