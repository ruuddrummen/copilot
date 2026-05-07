---
name: gh-init
description: "Scaffold GitHub-specific configuration into AGENTS.md and install the GitHub MCP server and/or `gh` CLI in the workspace and devcontainer. Use when: core init-workflow detects the workflow/gh module and delegates GitHub scaffolding here, or when a user wants to add GitHub tooling to a repo that already has core wired up."
disable-model-invocation: true
---

# gh-init

Wire GitHub access into a consumer repository:

1. Append the GitHub Platform Context, Terminology, and Platform Skills sections to the agent instructions file (`AGENTS.md` or `.github/copilot-instructions.md`).
2. Install the GitHub MCP server into `.vscode/mcp.json` and/or the devcontainer.
3. Install the `gh` CLI as a devcontainer feature.

This skill is normally invoked from `core/skills/init-workflow` once it detects that `workflow/gh` is installed. It can also be run on its own.

## Process

### 1. Detect the agent instructions file

Use the same detection order as `init-workflow`:

1. If `.github/copilot-instructions.md` exists → use that file.
2. If `AGENTS.md` exists → use that file.
3. If neither exists → create `AGENTS.md`.

### 2. Append the GitHub AGENTS.md fragment

If the file does not yet contain `## Platform Context`, append the fragment below, substituting the `{remote_url}` value (resolve via `git remote get-url origin`).

If the file already contains `## Platform Context` or `## Terminology`, merge by adding any missing sections at the end. Do not modify existing sections.

```markdown
## Platform Context

This repository uses GitHub for work item tracking and code reviews.

Remote URL: {remote_url}

## Terminology

| Abstract Term       | GitHub            |
| ------------------- | ----------------- |
| Work item           | Issue             |
| Sub-work item       | Sub-issue         |
| Bug                 | Issue (bug label) |
| Work item reference | #<ID>             |
| Code review         | Pull Request      |

## Platform Skills

- **`gh-tools`** — issues, sub-issues, comments, labels, search. Prefers the GitHub MCP server; falls back to the `gh` CLI.

Agents should pick the right skill by description matching; do not invoke `gh` or GitHub MCP tools directly.
```

### 3. Install the GitHub MCP server / `gh` CLI

1. Ask the user which GitHub transport(s) they want: **MCP**, **CLI**, or **both**.
2. **MCP** chosen — ask which target(s) to install into:
   - **(a) Workspace** (`.vscode/mcp.json`)
   - **(b) Devcontainer** — only offer if `.devcontainer/devcontainer.json` or `.devcontainer.json` exists. If chosen, ask which devcontainer approach: **workspace mount** (commit `.vscode/mcp.json`, same as (a)) or **embedded** under `customizations.vscode.mcp.servers` in `devcontainer.json`. Default: workspace mount — it also works for non-devcontainer users and avoids splitting MCP config across files.
   - **(c) Both**

   For each chosen target, apply the relevant snippet from the **GitHub MCP snippets** section below. If the target file already contains an entry for `io.github.github/github-mcp-server`, leave it alone and report "already present".
3. **CLI** chosen — detect `.devcontainer/devcontainer.json` or `.devcontainer.json`:
   - Found → ask whether to install `gh` in the container. On yes, apply the snippet from the **`gh` CLI devcontainer feature** section below. If `ghcr.io/devcontainers/features/github-cli` is already in the `features` object, skip and report.
   - Not found → remind the user to install the CLI locally (see the workflow README's manual setup section) and skip.
4. All edits must be idempotent: never duplicate an entry that already exists.

### 4. Summary

Report what was created or updated. List each MCP / CLI install that was applied, skipped (already present), or deferred to the user (e.g. local CLI install).

## GitHub MCP snippets

Server entry to inject:

```json
"io.github.github/github-mcp-server": {
  "type": "http",
  "url": "https://api.githubcopilot.com/mcp/",
  "gallery": "https://api.mcp.github.com",
  "version": "1.0.0"
}
```

### Workspace `.vscode/mcp.json`

VS Code reads `.vscode/mcp.json` at the workspace root. The same file works inside a devcontainer because the workspace is mounted.

If `.vscode/mcp.json` already declares `io.github.github/github-mcp-server` under `servers`, leave it alone.

#### a) File does not exist — create from scratch

```json
{
  "servers": {
    "io.github.github/github-mcp-server": {
      "type": "http",
      "url": "https://api.githubcopilot.com/mcp/",
      "gallery": "https://api.mcp.github.com",
      "version": "1.0.0"
    }
  }
}
```

#### b) File exists — merge into the existing `servers` object

Add the entry as an additional key inside `servers`. Do not replace other entries. Example before / after:

```json
// before
{
  "servers": {
    "io.github.upstash/context7": { "type": "stdio", "command": "npx", "args": ["@upstash/context7-mcp@1.0.31"] }
  }
}

// after
{
  "servers": {
    "io.github.upstash/context7": { "type": "stdio", "command": "npx", "args": ["@upstash/context7-mcp@1.0.31"] },
    "io.github.github/github-mcp-server": {
      "type": "http",
      "url": "https://api.githubcopilot.com/mcp/",
      "gallery": "https://api.mcp.github.com",
      "version": "1.0.0"
    }
  }
}
```

### Devcontainer

Two valid approaches (see [VS Code MCP docs — other options to add an MCP server](https://code.visualstudio.com/docs/copilot/customization/mcp-servers#_other-options-to-add-an-mcp-server)):

#### (a) Workspace `.vscode/mcp.json` — preferred

Commit the workspace snippet above; it is picked up inside the devcontainer because the workspace folder is mounted. Ensure `.vscode/mcp.json` is **not** in `.gitignore`.

#### (b) Embedded under `customizations.vscode.mcp.servers`

Useful when the team wants the MCP config bundled with the dev environment definition rather than the workspace settings.

##### File does not declare `customizations.vscode` yet — create from scratch

```json
{
  "image": "mcr.microsoft.com/devcontainers/typescript-node:latest",
  "customizations": {
    "vscode": {
      "mcp": {
        "servers": {
          "io.github.github/github-mcp-server": {
            "type": "http",
            "url": "https://api.githubcopilot.com/mcp/",
            "gallery": "https://api.mcp.github.com",
            "version": "1.0.0"
          }
        }
      }
    }
  }
}
```

##### `customizations.vscode` already exists — merge

Add the `mcp.servers` entry without replacing other `customizations.vscode` keys (e.g. `extensions`, `settings`). Example before / after:

```json
// before
{
  "customizations": {
    "vscode": {
      "extensions": ["github.copilot"]
    }
  }
}

// after
{
  "customizations": {
    "vscode": {
      "extensions": ["github.copilot"],
      "mcp": {
        "servers": {
          "io.github.github/github-mcp-server": {
            "type": "http",
            "url": "https://api.githubcopilot.com/mcp/",
            "gallery": "https://api.mcp.github.com",
            "version": "1.0.0"
          }
        }
      }
    }
  }
}
```

## `gh` CLI devcontainer feature

`gh` is installed by adding a devcontainer feature entry to your `devcontainer.json`:

```jsonc
"ghcr.io/devcontainers/features/github-cli:1": {}
```

Idempotency check before editing: if `ghcr.io/devcontainers/features/github-cli` is already in the `features` object, skip and report "already present".

### Variants

#### a) `features` object already exists

Add the entry as a new key inside the existing `features` object.

**Before:**

```jsonc
{
  "features": {
    "ghcr.io/devcontainers/features/common-utils:2": {}
  }
}
```

**After:**

```jsonc
{
  "features": {
    "ghcr.io/devcontainers/features/common-utils:2": {},
    "ghcr.io/devcontainers/features/github-cli:1": {}
  }
}
```

#### b) No `features` object

Create the `features` object with the entry.

**Before:**

```jsonc
{
  "image": "mcr.microsoft.com/devcontainers/base:ubuntu"
}
```

**After:**

```jsonc
{
  "image": "mcr.microsoft.com/devcontainers/base:ubuntu",
  "features": {
    "ghcr.io/devcontainers/features/github-cli:1": {}
  }
}
```

### Notes

- `gh auth login` must be run interactively after the container starts — it cannot be automated. Remind the user.
