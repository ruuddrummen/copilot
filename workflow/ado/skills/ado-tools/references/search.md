# Search Tools

> Last verified against [TOOLSET.md](https://github.com/microsoft/azure-devops-mcp/blob/main/docs/TOOLSET.md): 2026-04-23

## search_code

Search for code across repositories.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `searchText` | string | **Yes** | Search query text |
| `project` | string | No | Project name |
| `repository` | string | No | Filter to specific repo |
| `path` | string | No | Filter by path |
| `branch` | string | No | Filter by branch |
| `top` | number | No | Max results |
| `skip` | number | No | Results to skip |
| `includeFacets` | boolean | No | Include facets in results |

## search_wiki

Search wiki pages by keywords.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `searchText` | string | **Yes** | Search query text |
| `project` | string | No | Project name |
| `wiki` | string | No | Filter to specific wiki |
| `top` | number | No | Max results |
| `skip` | number | No | Results to skip |
| `includeFacets` | boolean | No | Include facets in results |

## search_workitem

Search work items by text and filters.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `searchText` | string | **Yes** | Search query text |
| `project` | string | No | Project name |
| `top` | number | No | Max results |
| `skip` | number | No | Results to skip |
| `areaPath` | string | No | Filter by area path |
| `assignedTo` | string | No | Filter by assignee |
| `state` | string | No | Filter by state |
| `workItemType` | string | No | Filter by work item type |
| `includeFacets` | boolean | No | Include facets in results |

### Gotchas

- The work item search tool is `search_workitem` (singular), not `search_work_items`.
- `search_code` does not require `project` — it can search across all projects in the organization.
