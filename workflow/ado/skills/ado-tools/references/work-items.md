# Work Items Tools

> Last verified against [TOOLSET.md](https://github.com/microsoft/azure-devops-mcp/blob/main/docs/TOOLSET.md): 2026-04-23

## wit_get_work_item

Get a single work item by ID.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `id` | number | **Yes** | Work item ID |
| `project` | string | **Yes** | Project name |
| `expand` | string | No | `None`, `Relations`, `Fields`, `Links`, `All` |
| `fields` | array | No | List of field names to return |
| `asOf` | string | No | Date-time to retrieve the work item as of |

> Cannot use `expand` and `fields` together.

## wit_get_work_items_batch_by_ids

Retrieve multiple work items by IDs in batch.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `project` | string | **Yes** | Project name |
| `ids` | array | **Yes** | Array of work item IDs |
| `fields` | array | No | Fields to include |

## wit_create_work_item

Create a new work item.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `project` | string | **Yes** | Project name |
| `workItemType` | string | **Yes** | Type: `Epic`, `Feature`, `Product Backlog Item`, `Task`, `Bug` |
| `fields` | array | **Yes** | Array of `{ name, value, format? }` objects |

**Fields format** — array of objects (NOT JSON Patch):

```json
{
  "project": "MyProject",
  "workItemType": "Product Backlog Item",
  "fields": [
    { "name": "System.Title", "value": "My title" },
    { "name": "System.Description", "value": "**Markdown** content", "format": "Markdown" },
    { "name": "System.AreaPath", "value": "MyProject\\MyTeam\\MyArea" },
    { "name": "System.IterationPath", "value": "MyProject\\MyTeam\\Sprint 1" },
    { "name": "Microsoft.VSTS.Common.AcceptanceCriteria", "value": "- Criterion", "format": "Markdown" },
    { "name": "Microsoft.VSTS.Common.Priority", "value": "2" }
  ]
}
```

### Common field reference names

| Field | Reference Name |
|-------|---------------|
| Title | `System.Title` |
| Description | `System.Description` |
| State | `System.State` |
| Area Path | `System.AreaPath` |
| Iteration Path | `System.IterationPath` |
| Assigned To | `System.AssignedTo` |
| Tags | `System.Tags` |
| Priority | `Microsoft.VSTS.Common.Priority` |
| Acceptance Criteria | `Microsoft.VSTS.Common.AcceptanceCriteria` |
| Story Points | `Microsoft.VSTS.Scheduling.StoryPoints` |
| Effort | `Microsoft.VSTS.Scheduling.Effort` |
| Remaining Work | `Microsoft.VSTS.Scheduling.RemainingWork` |
| Original Estimate | `Microsoft.VSTS.Scheduling.OriginalEstimate` |
| Value Area | `Microsoft.VSTS.Common.ValueArea` |

## wit_update_work_item

Update a work item by ID with specified fields.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `id` | number | **Yes** | Work item ID to update |
| `updates` | array | **Yes** | Array of JSON Patch operations |

**Updates format** — JSON Patch operations (different from `wit_create_work_item`):

```json
{
  "id": 12345,
  "updates": [
    { "op": "Replace", "path": "/fields/System.State", "value": "Active" },
    { "op": "Replace", "path": "/fields/System.Title", "value": "Updated title" }
  ]
}
```

> **Important**: `wit_update_work_item` uses `{ op, path, value }` (JSON Patch), NOT `{ name, value }`. This is different from `wit_create_work_item`.

## wit_update_work_items_batch

Update multiple work items in batch.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `updates` | array | **Yes** | Array of batch update operations |

## wit_add_child_work_items

Create child work items under a parent.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `parentId` | number | **Yes** | Parent work item ID |
| `project` | string | **Yes** | Project name |
| `workItemType` | string | **Yes** | Type for the child items |
| `items` | array | **Yes** | Array of child items to create |

## wit_work_items_link

Link work items together in batch.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `project` | string | **Yes** | Project name |
| `updates` | array | **Yes** | Array of link operations |

**Link type values**:
- `"parent"` — Creates a parent-child hierarchy link (sets `System.Parent`)
- `"child"` — Inverse of parent
- `"related"` — General related link
- `"predecessor"` — Predecessor dependency
- `"successor"` — Successor dependency

> **Important**: Use `"parent"` (not `"System.LinkTypes.Hierarchy-Reverse"`). The raw URN string is rejected with "Unknown link type".

## wit_work_item_unlink

Remove links from a work item.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `project` | string | **Yes** | Project name |
| `id` | number | **Yes** | Work item ID |
| `type` | string | No | Link type to remove |
| `url` | string | No | URL of the link to remove |

## wit_add_artifact_link

Link artifacts (commits, builds, PRs) to work items.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `workItemId` | number | **Yes** | Work item ID |
| `project` | string | **Yes** | Project name |
| `repositoryId` | string | No | Repository ID |
| `commitId` | string | No | Commit SHA |
| `buildId` | number | No | Build ID |
| `pullRequestId` | number | No | PR ID |
| `branchName` | string | No | Branch name |
| `projectId` | string | No | Project ID (GUID) |
| `linkType` | string | No | Type of artifact link |
| `artifactUri` | string | No | Direct artifact URI |
| `comment` | string | No | Link comment |

## wit_link_work_item_to_pull_request

Link a single work item to a pull request.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `projectId` | string | **Yes** | Project ID (GUID, not name) |
| `repositoryId` | string | **Yes** | Repository ID (GUID) |
| `pullRequestId` | number | **Yes** | Pull request ID |
| `workItemId` | number | **Yes** | Work item ID |
| `pullRequestProjectId` | string | No | Project ID of the PR if different |

> **Note**: This tool uses `projectId` (GUID), not `project` (name).

## wit_list_work_item_comments

List comments on a work item.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `project` | string | **Yes** | Project name |
| `workItemId` | number | **Yes** | Work item ID |
| `top` | number | No | Max comments to return |

## wit_add_work_item_comment

Add a comment to a work item.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `project` | string | **Yes** | Project name |
| `workItemId` | number | **Yes** | Work item ID |
| `comment` | string | **Yes** | Comment text |
| `format` | string | No | Comment format (e.g., `Markdown`) |

## wit_update_work_item_comment

Update an existing comment on a work item.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `project` | string | **Yes** | Project name |
| `workItemId` | number | **Yes** | Work item ID |
| `commentId` | number | **Yes** | Comment ID to update |
| `text` | string | **Yes** | New comment text |
| `format` | string | No | Comment format |

## wit_list_work_item_revisions

Get revision history of a work item.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `project` | string | **Yes** | Project name |
| `workItemId` | number | **Yes** | Work item ID |
| `top` | number | No | Max revisions |
| `skip` | number | No | Revisions to skip |
| `expand` | string | No | Expand options |

## wit_get_work_item_type

Get details of a specific work item type.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `project` | string | **Yes** | Project name |
| `workItemType` | string | **Yes** | Work item type name |

## wit_my_work_items

List work items relevant to the authenticated user.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `project` | string | **Yes** | Project name |
| `type` | string | No | Work item type filter |
| `top` | number | No | Max results |
| `includeCompleted` | boolean | No | Include completed items |

## wit_get_work_items_for_iteration

Get work items in a specific iteration.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `project` | string | **Yes** | Project name |
| `iterationId` | string | **Yes** | Iteration ID |
| `team` | string | No | Team name |

## wit_list_backlogs

List backlogs for a team.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `project` | string | **Yes** | Project name |
| `team` | string | **Yes** | Team name |

## wit_list_backlog_work_items

Get work items in a backlog.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `project` | string | **Yes** | Project name |
| `team` | string | **Yes** | Team name |
| `backlogId` | string | **Yes** | Backlog category ID |

## wit_get_query

Get a work item query by ID or path.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `project` | string | **Yes** | Project name |
| `query` | string | **Yes** | Query ID or path |
| `depth` | number | No | Query folder depth |
| `expand` | string | No | Expand options |
| `includeDeleted` | boolean | No | Include deleted queries |
| `useIsoDateFormat` | boolean | No | Use ISO date format |

## wit_get_query_results_by_id

Execute a saved query and get results.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `id` | string | **Yes** | Query ID |
| `project` | string | No | Project name |
| `team` | string | No | Team name |
| `top` | number | No | Max results |
| `timePrecision` | boolean | No | Include time in dates |
| `responseType` | string | No | Response format |

## wit_query_by_wiql

Execute a WIQL query and return matching work items.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `wiql` | string | **Yes** | WIQL query string |
| `project` | string | No | Project name |
| `team` | string | No | Team name |
| `top` | number | No | Max results |
| `timePrecision` | boolean | No | Include time in date comparisons |

## wit_get_work_item_attachment

Download a work item attachment as base64.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `project` | string | **Yes** | Project name |
| `attachmentId` | string | **Yes** | Attachment ID |
| `fileName` | string | No | File name hint |

### Gotchas

- `wit_create_work_item` uses `{ name, value }` fields format; `wit_update_work_item` uses JSON Patch `{ op, path, value }` — do not mix them up.
- `wit_link_work_item_to_pull_request` uses `projectId` (GUID), not `project` (name).
- `wit_add_work_item_comment` uses `workItemId` and `comment` parameters, not `id` and `text`.
- `wit_query_by_wiql` uses `wiql` parameter, not `query`.
- There is no `wit_manage_work_item_tags` tool — use `wit_update_work_item` to modify `System.Tags`.
