---
name: ado-init
description: "Scaffold Azure DevOps configuration into AGENTS.md and AZURE_DEVOPS.md. Use when: core init-workflow detects the workflow/ado module and delegates ADO scaffolding here, or when a user wants to add ADO tooling to a repo that already has core wired up."
disable-model-invocation: true
---

# ado-init

Wire Azure DevOps access into a consumer repository:

1. Append the ADO Platform Context, Terminology, and Platform Skills sections to the agent instructions file (`AGENTS.md` or `.github/copilot-instructions.md`).
2. Scaffold or merge `AZURE_DEVOPS.md` with team-specific configuration.

This skill is normally invoked from `core/skills/init-workflow` once it detects that `workflow/ado` is installed. It can also be run on its own.

## Process

### 1. Detect the agent instructions file

Use the same detection order as `init-workflow`:

1. If `.github/copilot-instructions.md` exists → use that file.
2. If `AGENTS.md` exists → use that file.
3. If neither exists → create `AGENTS.md`.

### 2. Append the ADO AGENTS.md fragment

Resolve the remote URL by running `git remote get-url origin`. If it is an ADO URL (contains `dev.azure.com` or `visualstudio.com`), extract the repository name from the URL path and resolve the repository ID by calling the Azure DevOps `get_repo_by_name_or_id` tool. Populate the `ADO Repository ID` line in the fragment below. If the remote is not an ADO URL, omit the `ADO Repository ID` line entirely.

If the file does not yet contain `## Platform Context`, append the fragment below, substituting the `{remote_url}` and `{repo_id}` values.

If the file already contains `## Platform Context` or `## Terminology`, merge by adding any missing sections at the end. Do not modify existing sections.

```markdown
## Platform Context

This repository uses Azure DevOps for work item tracking and code reviews.

Remote URL: {remote_url}
ADO Repository ID: {repo_id}

## Terminology

| Abstract Term       | Azure DevOps         |
| ------------------- | -------------------- |
| Work item           | Product Backlog Item |
| Sub-work item       | Task                 |
| Bug                 | Bug                  |
| Work item reference | #<ID>                |
| Code review         | Pull Request         |

## Platform Skills

- **`ado-create-backlog-item`** — create Features, PBIs, and Bugs with templates and parent linking.
- **`ado-revert-work-item`** — restore an Azure DevOps work item to a previous revision.
- **`ado-tools`** — fallback reference for ADO MCP tool schemas.

Always read `AZURE_DEVOPS.md` for context before using Azure DevOps tools.

Agents should pick the right skill by description matching; do not invoke ADO MCP tools directly.
```

### 3. Scaffold or update `AZURE_DEVOPS.md`

**If the file does not exist**, interview the user for:

- Project
- Team name
- Area paths (at least one) — default: `{project}\{team_name}`
- Default iteration path — default: `{project}\{team_name}`
- Key epics (optional — can be added later)
- Key features (optional — can be added later)
- Stakeholders (at least the team itself)

Then create `AZURE_DEVOPS.md` from the template below, substituting the `{placeholder}` values with the user's answers.

**If the file exists**, compare its sections (by `##` headings) against the template. Add any missing sections at the end. Do not modify existing sections.

#### `AZURE_DEVOPS.md` template

```markdown
# Azure DevOps Team Configuration

This file contains all team-specific configuration for the backlog manager agent and `ado-` skills. Any team adopting this repo should update the values below to match their own project setup.

## Project

- **Project:** `{project}`
- **Team name:** `{team_name}`

## Area Paths

| Area Path | Description |
|---|---|
| `{project}\{team_name}` | Team-managed features |

## Default Iteration

`{project}\{team_name}` (unless a specific sprint is required)

## Key Epics

| Epic | ID | Description |
|---|---|---|
| N/A | N/A | No epics defined yet. Create epics for major initiatives or feature areas. |

## Key Features

| Feature | ID | Description |
|---|---|---|
| N/A | N/A | No features defined yet. Create features for significant deliverables or components. |

## Templates

| Work Item Type | Template Path | Description |
|---|---|---|
| Epic | `{epic_template_path}` | Template for creating Epics |
| Feature | `{feature_template_path}` | Template for creating Features |
| Product Backlog Item | `{pbi_template_path}` | Template for creating Product Backlog Items |
| Bug | `{bug_template_path}` | Template for creating Bugs |

## Stakeholders

| Stakeholder | Description |
|---|---|
| {team_name} | {team_description} |
```

### 4. Summary

Report what was created or updated. If `AGENTS.md` or `AZURE_DEVOPS.md` were merged, list the sections that were added.
