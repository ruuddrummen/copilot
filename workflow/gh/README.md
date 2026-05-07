# workflow-gh

GitHub integration for the [workflow](../README.md) agentic system. Sibling of `workflow/ado`.

Install alongside `workflow/core` when your team tracks work in GitHub Issues and reviews code via GitHub Pull Requests.

## What's included

| Skill | Description |
| --- | --- |
| **gh-tools** | GitHub operations — issues, sub-issues, comments, labels, search. Prefers the GitHub MCP server; falls back to the `gh` CLI. |
| **gh-init** | Scaffolds the GitHub-flavoured `AGENTS.md` Platform Context, Terminology, and Platform Skills sections, and offers to install the GitHub MCP server and/or `gh` CLI in your workspace and devcontainer. Invoked from `core/skills/init-workflow`. |

The module also ships a `.mcp.json` declaring the GitHub MCP server so APM can wire it up automatically.

## Install

```shell
apm install ruuddrummen/copilot/workflow/core
apm install ruuddrummen/copilot/workflow/gh
```

Then run `/init-workflow` once. Core's `init-workflow` detects this module and delegates GitHub-specific scaffolding to `gh-init`.

## Manual setup

If `init-workflow` cannot manage your environment (e.g. no devcontainer, restricted shell), see the snippets inlined in [`skills/gh-init/SKILL.md`](skills/gh-init/SKILL.md). At least one of the GitHub MCP server or the `gh` CLI must be available for `gh-tools` to function.
