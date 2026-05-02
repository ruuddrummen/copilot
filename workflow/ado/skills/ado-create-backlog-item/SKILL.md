---
name: ado-create-backlog-item
description: "Create Azure DevOps backlog items including Features, Product Backlog Items (PBI), and Bugs. Use when: creating a feature, adding a backlog item, filing a bug, creating a work item in Azure DevOps, or adding items to the backlog."
argument-hint: "Work item type and description"
---

# Create Backlog Item Skill

This skill guides the creation of Features and Backlog Items in Azure DevOps.

## Project Configuration

Read `AZURE_DEVOPS.md` for all organization, team and project-specific configuration: project name, team name, repository name and ID, area paths, default iteration, key epics, stakeholders and backlog item templates.

## Work Item Hierarchy

- Epic → Feature → Backlog Item
- Features MUST be linked to existing Epics
- Backlog Items MUST be linked to existing Features
- Bugs do NOT require a parent item

## Creation Process

### Step 1: Create the Work Item

Create the work item using the Azure DevOps **work item creation** tool with:

- `project`: Use the project name from `AZURE_DEVOPS.md`
- `workItemType`: "Feature", "Product Backlog Item", or "Bug"
- `fields`: array of `{ "name": "<field name>", "value": "<field value>" }` objects — see example below
- For Bugs: use the current sprint iteration path by default
- For large text fields (description, acceptance criteria): write the content in Markdown and add `"format": "Markdown"` to the field object

### Step 2: Link to Parent

Immediately after creation, use the Azure DevOps **work item linking** tool to establish the parent-child relationship:

- Link the newly created work item ID to the parent work item ID
- Use `type: "parent"` for the relationship
- Skip this step for Bugs — they do not require a parent item

### Step 3: Verify

Always verify the relationship was established using the Azure DevOps **work item retrieval** tool with expanded relations

### Step 4: Report

After creation and linking, provide a summary of the created work item including a direct link to the Azure DevOps work item.

## Acceptance Criteria

### For Features (3-5 High-Level Criteria)

Focus on business outcomes:

- Business outcome 1
- Business outcome 2
- ...

### For Backlog Items (5-7 Detailed Criteria)

Focus on functional outcomes — what should be true for the user/team, not how it is implemented:

- Functional outcome 1
- Functional outcome 2
- ...

## Information to Gather

### When Creating Features

- Which Epic should this Feature be linked to?
- What is the primary business objective?
- Who are the key stakeholders?
- What are the critical dependencies?
- Which area path?

### When Creating Backlog Items

- Which Feature should this be linked to?
- What specific task needs to be done?
- What is the acceptance criteria focus?
- Are there specific edge cases or scenarios?
- What testing approach should be used?
- What documentation needs to be updated?

## Success Criteria

### For Features

- ✅ Linked to existing Epic
- ✅ Template fully populated
- ✅ Clear stakeholders and dependencies identified
- ✅ High-level outcome-focussed acceptance criteria defined (3-5 business-focused outcomes)

### For Backlog Items

- ✅ Linked to existing Feature (unless user specifies otherwise)
- ✅ Template fully populated
- ✅ Functional outcome-focussed acceptance criteria defined (5-7 criteria)
- ✅ Solution realization describes the direction, not implementation specifics
- ✅ Comprehensive test plan

## Best Practices

1. Always verify parent work item exists before creating children
2. Populate ALL template sections comprehensively
3. Keep descriptions and acceptance criteria concise (less is more)
4. Use default iteration unless specific sprint is specified; for Bugs use the current sprint
5. Verify parent-child relationships after creation (not applicable for Bugs)
6. Use markdown formatting in description and acceptance criteria fields
7. Never create Epics — suggest the content in the conversation instead

---

## Tool Reference

Quick reference for the ADO MCP tools used by this skill.

### wit_create_work_item

Create a new work item.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `project` | string | **Yes** | Project name |
| `workItemType` | string | **Yes** | `Epic`, `Feature`, `Product Backlog Item`, `Task`, `Bug` |
| `fields` | array | **Yes** | Array of `{ name, value, format? }` objects |

**Fields format** — array of objects (NOT JSON Patch):

```json
{
  "project": "MyProject",
  "workItemType": "Product Backlog Item",
  "fields": [
    { "name": "System.Title", "value": "My title" },
    { "name": "System.AreaPath", "value": "MyProject\\MyTeam\\MyArea" },
    { "name": "System.IterationPath", "value": "MyProject\\MyTeam\\Sprint 1" },
    { "name": "System.Description", "value": "**Markdown** content", "format": "Markdown" },
    { "name": "Microsoft.VSTS.Common.AcceptanceCriteria", "value": "- Criterion", "format": "Markdown" },
    { "name": "Microsoft.VSTS.Common.Priority", "value": "2" },
    { "name": "Microsoft.VSTS.Scheduling.StoryPoints", "value": "5" },
    { "name": "System.Tags", "value": "tag1; tag2" }
  ]
}
```

> **Gotcha**: Fields use `{ name, value }` format. Do NOT use JSON Patch `{ op, path, value }` — that format is for `wit_update_work_item` only.

> **Gotcha**: Add `"format": "Markdown"` for fields containing Markdown (Description, Acceptance Criteria). Without it, Markdown may render as plain text.

#### Common field reference names

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

### wit_update_work_item

Update a work item by ID.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `id` | number | **Yes** | Work item ID |
| `updates` | array | **Yes** | Array of JSON Patch operations |

**Updates format** — JSON Patch (different from create):

```json
{
  "id": 12345,
  "updates": [
    { "op": "Replace", "path": "/fields/System.State", "value": "Active" },
    { "op": "Replace", "path": "/fields/System.Title", "value": "Updated title" }
  ]
}
```

> **Gotcha**: Uses `{ op, path, value }` (JSON Patch), NOT `{ name, value }`. Do not confuse with `wit_create_work_item`.

### wit_work_items_link

Link work items together.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `project` | string | **Yes** | Project name |
| `updates` | array | **Yes** | Array of link operations |

**Link type values**:
- `"parent"` — Creates parent-child hierarchy link (sets `System.Parent`)
- `"child"` — Inverse of parent
- `"related"` — General related link
- `"predecessor"` / `"successor"` — Dependency links

> **Gotcha**: Use `"parent"` (not `"System.LinkTypes.Hierarchy-Reverse"`). The raw URN string is rejected with "Unknown link type".

### wit_get_work_item

Get a single work item by ID.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `id` | number | **Yes** | Work item ID |
| `project` | string | **Yes** | Project name |
| `expand` | string | No | `None`, `Relations`, `Fields`, `Links`, `All` |
| `fields` | array | No | Specific fields to return |
| `asOf` | string | No | Date-time to get work item as of |

> Cannot use `expand` and `fields` together.

Use `expand: "Relations"` after creation to verify parent-child links.

### work_list_team_iterations

List iterations assigned to a team.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `project` | string | **Yes** | Project name |
| `team` | string | **Yes** | Team name |
| `timeframe` | string | No | `past`, `current`, `future` |

Use `timeframe: "current"` to find the current sprint for Bug assignments.
