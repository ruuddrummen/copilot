---
name: init-workflow
description: "Initialize or update AGENTS.md and AZURE_DEVOPS.md in the consumer repository. Use when: a team has just installed the workflow packages and needs to configure their repo, or when templates have been updated and existing config files need new sections merged in."
disable-model-invocation: true
---

# Init

Scaffold or update the repository configuration files required by the workflow agents.

## When to use

- After first installing `workflow/core` or `workflow/ado` via APM.
- After upgrading the workflow packages, to merge in any new template sections.

## Behavior

- **New file**: Create from template, interviewing the user for required values.
- **Existing file**: Auto-merge missing sections from the template. Never remove or overwrite existing content — only add. The user has git, so changes are safe to revert.

## Process

### 1. Detect installed modules

Check which workflow modules are installed by looking for their skill directories:

- `workflow/core` → look for `developer-loop` or `refine` skills
- `workflow/ado` → look for `ado-tools` skill

### 2. Scaffold or update agent instructions

The workflow agents need platform context, terminology, and interaction preferences. This content can live in either:

- `AGENTS.md` (VS Code agent-level instructions)
- `.github/copilot-instructions.md` (GitHub Copilot workspace instructions)

**Detection order:**

1. If `.github/copilot-instructions.md` exists → use that file.
2. If `AGENTS.md` exists → use that file.
3. If neither exists → create `AGENTS.md`.

**If the target file does not contain agent instructions yet**:

1. Resolve the remote URL by running `git remote get-url origin`. If multiple remotes exist, use `origin`.
2. Infer the platform from the remote URL:
   - URLs containing `dev.azure.com` or `visualstudio.com` → Azure DevOps
   - URLs containing `github.com` → GitHub
   - Otherwise → ask the user which platform they use.

Then append the content from [references/agents.md](references/agents.md) to the target file, substituting the `{placeholder}` values. When appending to an existing `.github/copilot-instructions.md`, add a blank line separator before the new content.

**If the target file already contains the agent instruction sections** (detected by the presence of `## Platform Context` or `## Terminology` headings), compare its sections (by `##` headings) against the template. Add any missing sections at the end. Do not modify existing sections.

### 3. Install GitHub MCP server / `gh` CLI (GitHub platform only)

Skip this entire step if GitHub is not among the detected platforms.

1. Ask the user which GitHub transport(s) they want: **MCP**, **CLI**, or **both**. The answer drives both the Platform Skills wiring in step 2 and the installation below.
2. **MCP** chosen — ask which target(s) to install into:
   - **(a) Workspace** (`.vscode/mcp.json`)
   - **(b) Devcontainer** — only offer if `.devcontainer/devcontainer.json` or `.devcontainer.json` exists. If chosen, ask which devcontainer approach: **workspace mount** (commit `.vscode/mcp.json`, same as (a)) or **embedded** under `customizations.vscode.mcp.servers` in `devcontainer.json`. Default: workspace mount — it also works for non-devcontainer users and avoids splitting MCP config across files.
   - **(c) Both**

   For each chosen target, apply the relevant snippet from [references/github-mcp.md](references/github-mcp.md). If the target file already contains an entry for `io.github.github/github-mcp-server`, leave it alone and report "already present".
3. **CLI** chosen — detect `.devcontainer/devcontainer.json` or `.devcontainer.json`:
   - Found → ask whether to install `gh` in the container. On yes, apply the snippet from [references/github-cli-devcontainer.md](references/github-cli-devcontainer.md). If `ghcr.io/devcontainers/features/github-cli` is already in the `features` object, skip and report.
   - Not found → remind the user to install the CLI locally per the README and skip.
4. All edits must be idempotent: never duplicate an entry that already exists.

### 4. Scaffold or update AZURE_DEVOPS.md (ADO module only)

Only proceed if the `workflow/ado` module is installed.

**Constants — do not prompt the user for these:**

- **Project name** is hard-coded to `BNGBank` for this organisation. Substitute it directly into the template wherever `{project}` appears. Do not ask the user.

**If the file does not exist**, interview the user for the remaining values:

- Team name
- Area paths (at least one) - default: `BNGBank\Team <team-name>`
- Default iteration path - default: `BNGBank\Team <team-name>`
- Key epics (optional — can be added later)
- Key features (optional — can be added later)
- Stakeholders (at least the team itself)

The repository name and ID are derived from the git remote URL already obtained in step 2 — do not ask the user:

- If the remote is an ADO URL (contains `dev.azure.com` or `visualstudio.com`), extract the repository name from the URL path and resolve the repository ID by calling the Azure DevOps `get_repo_by_name_or_id` tool (pass `BNGBank` as the project name). Populate the `ADO Repository ID: {repo_id}` line in `AGENTS.md` under `## Platform Context`.
- If the remote is not an ADO URL (e.g., GitHub), omit the `ADO Repository ID` line from `AGENTS.md` entirely.

Then create `AZURE_DEVOPS.md` using the template in [references/azure-devops.md](references/azure-devops.md), substituting the `{placeholder}` values with the user's answers.

**If the file exists**, compare its sections against the template. Add any missing sections at the end. Do not modify existing sections.

### 5. Summary

Report what was created or updated. If files were merged, list the sections that were added. For step 3, list each GitHub MCP / CLI install that was applied, skipped (already present), or deferred to the user (e.g. local CLI install).
