---
name: revert-work-item
description: 'Restore a previous version of an Azure DevOps work item by reading its revision history and applying chosen field values. Use when: reverting, undoing, or restoring a work item to a previous state; accidental edit; wrong field values saved.'
argument-hint: 'Work item ID to revert (e.g. 415356)'
---

# Revert Work Item Skill

Restores an Azure DevOps work item to a previous revision by reading its full revision history, presenting a discovery summary to the user, and applying the chosen version's fields.

## When to Use

- A work item was accidentally edited and needs to be rolled back
- Fields like Title, Description, or Acceptance Criteria were overwritten
- The user wants to compare revisions and pick one to restore

## Prerequisites

Read `AZURE_DEVOPS.md` for the project name before using any Azure DevOps tools.

## Process

### Step 1: Fetch Revision History

Use `list_work_item_revisions` with the work item ID and project from `AZURE_DEVOPS.md`.

> **Important:** The revision history API truncates large text fields at 8,192 characters. Do **not** use revision data as the restore source for Description or Acceptance Criteria — use `get_work_item` with `asOf` instead (Step 3). See [adо-truncation-workaround](./references/ado-truncation-workaround.md) for details.

### Step 2: Present Revision Summary and Ask the User

Build a summary table. For each revision include:

| Rev | Date (UTC) | Changed By | Key changes |
|-----|------------|------------|-------------|
| N   | ...        | ...        | State, Title, or field changes vs. previous rev |

To identify key changes, compare `System.State`, `System.Title`, `System.IterationPath`, and `System.Description` (by **length only** — not full content) between consecutive revisions.

After presenting the table, ask the user:

> "Which revision would you like to restore to?"

### Step 3: Fetch Full Field Values for the Target Revision

Once the user selects a revision, use `get_work_item` with `asOf` to retrieve the **complete, untruncated** field values. See [ado-truncation-workaround](./references/ado-truncation-workaround.md) for the exact `asOf` calculation.

Fields to fetch: `System.Title`, `System.Description`, `Microsoft.VSTS.Common.AcceptanceCriteria`, `System.State`, `System.IterationPath`.

### Step 4: Confirm Before Writing

Present what will be restored and ask the user to confirm:

> "Ready to restore #\<ID\> to rev \<N\>. This will update: Title, Description (\<N\> chars), Acceptance Criteria (\<N\> chars). Confirm?"

### Step 5: Apply the Restore

Use `update_work_item` with `op: "Replace"` for each field. Always include `format: "Markdown"` for `System.Description` and `Microsoft.VSTS.Common.AcceptanceCriteria`.

Always restore at minimum:
- `System.Title`
- `System.Description`
- `Microsoft.VSTS.Common.AcceptanceCriteria`

Only restore `System.State` and `System.IterationPath` if they changed between target and current, **and** the user explicitly confirms.

### Step 6: Verify and Report

Re-fetch the work item and confirm the description length matches what was written. Report:

- Work item ID and direct URL
- Revision restored to
- Fields updated
- Description length after restore (confirms no silent truncation)

## Constraints

- **Never guess field values.** Always source large text fields from `get_work_item` with `asOf`, never from revision history.
- **Always confirm before writing.** Do not apply changes without explicit user confirmation in Step 4.
- **Preserve unrelated fields.** Do not reset `System.AssignedTo`, comments, or relations.
