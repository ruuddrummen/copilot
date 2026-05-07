---
name: init-workflow
description: "Initialize or update agent configuration files in the consumer repository. Use when: a team has just installed the workflow packages and needs to configure their repo, or when templates have been updated and existing config files need new sections merged in. Delegates platform-specific scaffolding to whichever platform module(s) are installed (e.g. workflow/gh, workflow/ado)."
disable-model-invocation: true
---

# Init

Scaffold or update the repository configuration files required by the workflow agents. Core scaffolds an agnostic skeleton; per-module `*-init` skills (e.g. `gh-init`, `ado-init`) handle platform-specific content.

## When to use

- After first installing `workflow/core` (and optionally one or more platform modules) via APM.
- After upgrading the workflow packages, to merge in any new template sections.

## Behavior

- **New file**: Create from template.
- **Existing file**: Auto-merge missing sections from the template. Never remove or overwrite existing content — only add. The user has git, so changes are safe to revert.

## Process

### 1. Detect installed modules

Check which workflow modules are installed by looking for their skill directories:

- `workflow/core` → `core/skills/developer-loop` or `core/skills/refine`.
- `workflow/gh` → `gh/skills/gh-init` (and `gh/skills/gh-tools`).
- `workflow/ado` → `ado/skills/ado-init` (and `ado/skills/ado-tools`).
- Any other sibling module under `workflow/` exposing a `*-init` skill should be treated the same way.

### 2. Scaffold the agnostic agent instructions skeleton

The workflow agents need an `## Agent Interaction` policy and a `## Platform Skills` section that the user (or a platform `*-init` skill) populates.

Pick the target file using this detection order:

1. If `.github/copilot-instructions.md` exists → use that file.
2. If `AGENTS.md` exists → use that file.
3. If neither exists → create `AGENTS.md`.

If the target file does not yet contain agent instruction sections (detected by the absence of `## Platform Skills` and `## Agent Interaction` headings), append the content from [references/agents.md](references/agents.md). When appending to an existing `.github/copilot-instructions.md`, add a blank line separator before the new content.

If the target file already contains the agent instruction sections, compare its sections (by `##` headings) against the template. Add any missing sections at the end. Do not modify existing sections.

The agnostic skeleton intentionally leaves `## Platform Skills` empty (or with a placeholder) — it is filled in by the next step or by the user.

### 3. Delegate platform-specific scaffolding

For each platform module detected in step 1, invoke its `*-init` skill and let it handle the platform-specific work (Platform Context, Terminology, Platform Skills, and any platform-specific config files such as `AZURE_DEVOPS.md`):

- `workflow/gh` installed → invoke `gh-init`.
- `workflow/ado` installed → invoke `ado-init`.

If no platform module is installed, skip this step. The user is expected to fill in the empty `## Platform Skills` section themselves — either with custom skills they have installed or with a brief description of how agents should interact with their tracker (e.g. raw CLI invocations, manual steps).

### 4. Summary

Report what core created or updated, then summarise what each delegated `*-init` skill reported.
