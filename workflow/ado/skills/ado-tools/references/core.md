# Core Tools

> Last verified against [TOOLSET.md](https://github.com/microsoft/azure-devops-mcp/blob/main/docs/TOOLSET.md): 2026-04-23

## core_list_projects

List all projects in the Azure DevOps organization.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `projectNameFilter` | string | No | Filter projects by name |
| `stateFilter` | string | No | Filter by project state |
| `top` | number | No | Max results to return |
| `skip` | number | No | Number of results to skip |
| `continuationToken` | string | No | Continuation token for pagination |

## core_list_project_teams

List teams within a project.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `project` | string | **Yes** | Project name or ID |
| `mine` | boolean | No | If true, only return teams the user belongs to |
| `top` | number | No | Max results |
| `skip` | number | No | Results to skip |

## core_get_identity_ids

Retrieve Azure DevOps identity IDs by search filter.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `searchFilter` | string | **Yes** | Search filter for identities |

### Gotchas

- There is no `core_get_project` tool — use `core_list_projects` with `projectNameFilter` instead.
- There is no `core_list_teams` tool — the correct name is `core_list_project_teams`.
