---
name: init-workflow
description: "Initialize or update agent configuration files in the consumer repository. Use when: a team has just installed the workflow packages and needs to configure their repo, or when templates have been updated and existing config files need new sections merged in. Delegates platform-specific scaffolding to any installed `*-init` skill (e.g. `gh-init`, `ado-init`)."
disable-model-invocation: true
---

# Init

Scaffold or update the repository configuration files required by the workflow agents. Core scaffolds an agnostic skeleton; platform `*-init` skills handle platform-specific content.

## When to use

- After first installing the workflow (core plus any platform modules) via APM.
- After upgrading the workflow, to merge in any new template sections.

## Behavior

- **New file**: Create from template.
- **Existing file**: Auto-merge missing sections. Never remove or overwrite existing content — only add.

## Process

### 1. Detect installed `*-init` skills

List the skills directory and look for any skill whose name ends in `-init` (e.g. `gh-init`, `ado-init`). APM installs all skills into a single flat `skills/` folder regardless of which module they came from, so detect by skill name, not by path.

### 2. Scaffold the agnostic agent instructions skeleton

The workflow agents need an `## Agent Interaction` policy and a `## Platform Skills` section.

Pick the target file:

1. `.github/copilot-instructions.md` if it exists.
2. Else `AGENTS.md` if it exists.
3. Else create `AGENTS.md`.

If the file lacks `## Platform Skills` and `## Agent Interaction`, append the content from [references/agents.md](references/agents.md). When appending to `.github/copilot-instructions.md`, separate with a blank line.

If the file already has those sections, add any missing sections at the end. Do not modify existing sections.

`## Platform Skills` is intentionally left empty for the next step or the user to fill in.

### 3. Delegate platform-specific scaffolding

Invoke every `*-init` skill detected in step 1. Each handles its own Platform Context, Terminology, Platform Skills entries, and any platform-specific config files (e.g. `AZURE_DEVOPS.md`).

If none are installed, skip. The user fills in `## Platform Skills` themselves with whatever tooling they have — custom skills, raw CLI invocations, or manual steps.

### 4. Summary

Report what core created or updated, then summarise each delegated `*-init` skill's report.
