# workflow

A multi-agent development workflow that takes you from idea to shipped code. An **Orchestrator** coordinates the conversation while specialised agents handle planning, implementation, research, and exploration autonomously.

Some skills are heavily inspired by https://github.com/mattpocock/skills.

## What's included

The workflow ships as three independently installable **capability modules**:

| Module | Path | Description |
| --- | --- | --- |
| **core** | `workflow/core` | Platform-agnostic foundation — Orchestrator, Developer, Planner, Explore, and Researcher agents plus the agnostic skills (init-workflow, refine, to-prd, to-issues, developer-loop, researcher-loop, QA, architecture improvement). Functions on its own; users wire their own tooling through `AGENTS.md`'s Platform Skills section. |
| **gh** | `workflow/gh` | GitHub integration — `gh-tools` (issues, sub-issues, comments, search; prefers MCP, falls back to `gh` CLI) and `gh-init` (scaffolds GitHub `AGENTS.md` content; offers to install the GitHub MCP server / `gh` CLI). |
| **ado** | `workflow/ado` | Azure DevOps integration — backlog item creation, ADO MCP tool reference, work item revert, and `ado-init` (scaffolds `AZURE_DEVOPS.md` and ADO `AGENTS.md` content). |

Install **core** for the base workflow. Add **gh** and/or **ado** for first-party platform integrations. Consumers using a different tracker (Jira, Linear, GitLab, etc.) — or none at all — install only **core** and fill in the `## Platform Skills` section of `AGENTS.md` with their own skills, raw CLI invocations, or manual instructions.

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
apm install ruuddrummen/copilot/workflow/core

# GitHub integration (optional)
apm install ruuddrummen/copilot/workflow/gh

# Azure DevOps integration (optional)
apm install ruuddrummen/copilot/workflow/ado
```

> You can update the modules at any time by running `apm deps update`. Add `-g` to update global installations.

> Add `-g` to install globally for all repositories (e.g. `apm install -g ruuddrummen/copilot/workflow/core`). Global installs do not include prompts and MCP servers — those remain repo-scoped.

### 3. Run `init-workflow`

> After installing the modules, run the `/init-workflow` skill. Core scaffolds an agnostic `AGENTS.md` skeleton (`## Platform Skills` + `## Agent Interaction`) and then delegates to whichever platform module(s) you installed. `gh-init` adds GitHub Platform Context, Terminology, and Platform Skills to `AGENTS.md` and offers to install the GitHub MCP server / `gh` CLI. `ado-init` adds the ADO equivalents and scaffolds `AZURE_DEVOPS.md`.
>
> If you installed only `core`, `init-workflow` leaves the `## Platform Skills` section as a placeholder for you to fill in.
>
> On subsequent runs (e.g., after a package upgrade) it merges in any new template sections without overwriting existing content.

### Bring your own platform tooling

If you use a tracker without a first-party module, install only `workflow/core` and edit the `## Platform Skills` section in `AGENTS.md` to describe how agents should interact with your tracker. You can:

- Reference custom skills you have installed (any skill whose description matches the relevant operation works — agents pick by description).
- Describe raw CLI invocations to use (e.g. `jira issue create ...`).
- State that there is no tracker and that work items should be discussed in chat.

Core agents never name a specific platform skill in their behaviour; they always defer to whatever you list here.

<details>
<summary>Manual GitHub MCP / CLI setup</summary>

If `gh-init` cannot manage your environment (e.g. no devcontainer, restricted shell), you can install GitHub access manually. The full snippets are inlined in `workflow/gh/skills/gh-init/SKILL.md`.

GitHub users may use either the **GitHub MCP server** (`io.github.github/github-mcp-server`, added by the consumer to `.vscode/mcp.json`) or the `gh` **CLI** for GitHub operations — at least one is required.

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

Each module ships a `.mcp.json` declaring the MCP servers it needs. APM handles installation. If your environment requires manual setup, see the `.mcp.json` files in each module for the server declarations to add to your `.vscode/mcp.json`.

| Module   | Servers                          |
| -------- | -------------------------------- |
| **core** | Context7, Microsoft Learn        |
| **gh**   | GitHub MCP server                |
| **ado**  | Azure DevOps MCP                 |

> **Note:** `core` deliberately does **not** bundle the GitHub MCP server — it would force-load on consumers who do not use GitHub. Install `workflow/gh` (which ships the server in its `.mcp.json`) or add `io.github.github/github-mcp-server` to your own `.vscode/mcp.json` manually.

</details>

<details>
<summary>Manual AGENTS.md / AZURE_DEVOPS.md setup</summary>

#### AGENTS.md

Every repo using the workflow needs an `AGENTS.md` at the root (or equivalent content in `.github/copilot-instructions.md`). At minimum it needs an `## Agent Interaction` section and a `## Platform Skills` section. Platform modules add `## Platform Context` and `## Terminology` on top.

The agnostic skeleton scaffolded by core looks like this:

```markdown
# AGENTS.md

## Platform Skills

<!-- Filled in by gh-init / ado-init, or by the user for bring-your-own setups. -->

## Agent Interaction

- Before using the Ask Questions tool, provide relevant context and reasoning so the user can make an informed choice.
- Include recommended answers in question options whenever possible.
- Before asking the user, try to answer the question by exploring the codebase or consulting documentation.
- Keep chat messages brief and to the point.
```

A GitHub-flavoured `AGENTS.md` after `gh-init` runs adds, on top of the skeleton:

```markdown
## Platform Context

This repository uses GitHub for work item tracking and code reviews.

Remote URL: https://github.com/<owner>/<repo>

## Terminology

| Abstract Term       | GitHub            |
| ------------------- | ----------------- |
| Work item           | Issue             |
| Sub-work item       | Sub-issue         |
| Bug                 | Issue (bug label) |
| Work item reference | #<ID>             |
| Code review         | Pull Request      |

## Platform Skills

- **`gh-tools`** — issues, sub-issues, comments, labels, search. Prefers the GitHub MCP server; falls back to the `gh` CLI.
```

An ADO-flavoured `AGENTS.md` after `ado-init` runs adds the ADO equivalents (Platform Context with `Remote URL` and `ADO Repository ID`, an ADO Terminology table, and Platform Skills naming `ado-create-backlog-item`, `ado-revert-work-item`, and `ado-tools`).

#### AZURE_DEVOPS.md (ADO teams only)

If you installed the **ado** module, `ado-init` scaffolds `AZURE_DEVOPS.md` from the template inlined in `workflow/ado/skills/ado-init/SKILL.md`. The minimal manual equivalent:

```markdown
# Azure DevOps Team Configuration

## Project

- **Project:** `YourProject`
- **Team name:** `Your Team`

## Area Paths

| Area Path               | Description           |
| ----------------------- | --------------------- |
| `YourProject\Your Team` | Team-managed features |

## Default Iteration

`YourProject\Your Team`

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
| **refine-with-docs** | Refinement that challenges plans against the existing domain model and updates docs inline |
| **to-prd** | Synthesize a PRD from conversation context and submit as a work item (uses a single canonical template; override via `<repo>/templates/prd.md`) |
| **to-issues** | Break a PRD into independently-grabbable vertical-slice work items |
| **create-plan** | Read work items and produce an ordered implementation plan with dependencies |
| **developer-loop** | Run Developer in a loop to complete all tasks under a work item |
| **researcher-loop** | Run Researcher in a loop to complete all research subtasks |
| **qa** | Interactive QA session — user reports bugs, agent files work items |
| **improve-codebase-architecture** | Explore for architectural friction and propose module-deepening refactors |
| **init-workflow** | Scaffold the agnostic `AGENTS.md` skeleton and delegate platform-specific scaffolding to whichever platform module(s) are installed |

### GitHub skills (`workflow/gh`)

| Skill | Description |
| --- | --- |
| **gh-tools** | Lean GitHub operations (issues, sub-issues, comments, labels, search) — prefers MCP, falls back to `gh` CLI |
| **gh-init** | Scaffold GitHub `AGENTS.md` content; install the GitHub MCP server and/or `gh` CLI in workspace and devcontainer |

### ADO skills (`workflow/ado`)

| Skill | Description |
| --- | --- |
| **ado-create-backlog-item** | Create Features, PBIs, and Bugs in Azure DevOps with templates and parent linking |
| **ado-revert-work-item** | Restore an Azure DevOps work item to a previous revision from its history |
| **ado-tools** | Fallback reference for all ADO MCP tool schemas (used only when dedicated skills are insufficient) |
| **ado-init** | Scaffold ADO `AGENTS.md` content and `AZURE_DEVOPS.md` |

## Prompts

Three prompt entry points are provided, differing only in model selection:

| Prompt         | Use when                                       |
| -------------- | ---------------------------------------------- |
| **dev**        | Default — best balance of capability and speed |
| **dev-max**    | Maximum capability for complex tasks           |
| **dev-sonnet** | Faster, lighter tasks                          |

All three launch the Orchestrator agent.
