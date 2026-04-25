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
- `workflow/ado` → look for `create-backlog-item` skill

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

### 3. Scaffold or update AZURE_DEVOPS.md (ADO module only)

Only proceed if the `workflow/ado` module is installed.

**If the file does not exist**, interview the user for:

- Project name
- Team name
- Repository name
- Area paths (at least one)
- Default iteration path
- Key epics (optional — can be added later)
- Key features (optional — can be added later)
- Stakeholders (at least the team itself)

After gathering the project name and repository name, resolve the repository ID automatically using the Azure DevOps `get_repo_by_name_or_id` tool (pass the project name and repository name). Use the returned `id` field to populate the `{repo_id}` placeholder in the template.

Then create `AZURE_DEVOPS.md` using the template in [references/azure-devops.md](references/azure-devops.md), substituting the `{placeholder}` values with the user's answers.

**If the file exists**, compare its sections against the template. Add any missing sections at the end. Do not modify existing sections.

### 4. Summary

Report what was created or updated. If files were merged, list the sections that were added.
