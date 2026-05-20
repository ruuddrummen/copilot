---
name: ado-tools
description: "FALLBACK ONLY — Last-resort reference for Azure DevOps MCP tool schemas. Use only when the agent fails to operate MCP tools without it. When used, report usage to the user and recommend creating dedicated skills."
---

# Azure DevOps MCP Tools Reference

> **⚠ Fallback only.** This skill should ONLY be used when the agent fails to operate ADO MCP tools without consulting it. Dedicated skills (e.g., `ado-create-backlog-item`) should be the primary source of tool guidance.
>
> **When this skill is used, the agent MUST report to the user:**
> 1. That this fallback skill was used
> 2. What specific operation or tool it was needed for
> 3. A recommendation to update the relevant existing skill or create a new dedicated skill for that operation — so this fallback is not needed again

Comprehensive reference for all tools provided by the [Azure DevOps MCP server](https://github.com/microsoft/azure-devops-mcp).

## Authoritative Source

The official tool documentation is maintained at:
**[TOOLSET.md](https://github.com/microsoft/azure-devops-mcp/blob/main/docs/TOOLSET.md)**

The reference files in this skill were verified against TOOLSET.md on 2026-04-23. When in doubt, consult the official source.

## Tool Naming Convention

Tool names in this reference use the **bare name** (e.g., `wit_create_work_item`). In VS Code, tools are prefixed with `mcp_<server-name>_` based on the server name configured in `.vscode/mcp.json`. For example, if the server is named `ado-remote-mcp`, the full tool name becomes `mcp_ado-remote-mcp_wit_create_work_item`.

## Domains

Each domain has its own reference file with tool names, parameter tables, and known gotchas:

| Domain | File | Tools |
|--------|------|-------|
| Core | [core.md](references/core.md) | Projects, teams, identities |
| Work Items | [work-items.md](references/work-items.md) | CRUD, linking, comments, queries |
| Work | [work.md](references/work.md) | Iterations, capacity, team settings |
| Repositories | [repositories.md](references/repositories.md) | Repos, branches, commits, PRs, threads |
| Pipelines | [pipelines.md](references/pipelines.md) | Builds, runs, logs, artifacts |
| Search | [search.md](references/search.md) | Code, work item, wiki search |
| Wiki | [wiki.md](references/wiki.md) | Wiki pages CRUD |
| Test Plans | [test-plans.md](references/test-plans.md) | Plans, suites, test cases, results |
| Advanced Security | [advanced-security.md](references/advanced-security.md) | Security alerts |

## Common Gotchas

1. **`wit_create_work_item` fields format**: Use `{ name, value }` objects in an array — NOT JSON Patch `{ op, path, value }` format. The `wit_update_work_item` tool uses a different format (`{ op, path, value }`).
2. **Link types**: Use simple strings like `"parent"`, not URN strings like `"System.LinkTypes.Hierarchy-Reverse"`. The URN format is rejected.
3. **Markdown fields**: Add `"format": "Markdown"` for fields containing Markdown (Description, Acceptance Criteria).
4. **Branch refs**: PR-related tools require full ref paths (`refs/heads/main`), while other tools accept just the branch name.
5. **Project parameter**: Nearly all tools require `project` — always use the project name, not the ID.
6. **Tool availability**: The toolset is the same for both remote (`mcp.dev.azure.com`) and local (`npx @azure-devops/mcp`) servers.
