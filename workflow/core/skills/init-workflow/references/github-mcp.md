# GitHub MCP server snippets

Server entry to inject:

```json
"io.github.github/github-mcp-server": {
  "type": "http",
  "url": "https://api.githubcopilot.com/mcp/",
  "gallery": "https://api.mcp.github.com",
  "version": "1.0.0"
}
```

## Workspace `.vscode/mcp.json`

- VS Code reads `.vscode/mcp.json` at the workspace root. Same file works inside a devcontainer because the workspace is mounted.
- If `.vscode/mcp.json` already declares `io.github.github/github-mcp-server` under `servers`, leave it alone.

### a) File does not exist — create from scratch

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

### b) File exists — merge into the existing `servers` object

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

## Devcontainer

Two valid approaches (see [VS Code MCP docs — other options to add an MCP server](https://code.visualstudio.com/docs/copilot/customization/mcp-servers#_other-options-to-add-an-mcp-server)):

### (a) Workspace `.vscode/mcp.json` — preferred

Commit the workspace snippet above; it is picked up inside the devcontainer because the workspace folder is mounted. Ensure `.vscode/mcp.json` is **not** in `.gitignore`.

### (b) Embedded under `customizations.vscode.mcp.servers`

Useful when the team wants the MCP config bundled with the dev environment definition rather than the workspace settings.

#### File does not declare `customizations.vscode` yet — create from scratch

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

#### `customizations.vscode` already exists — merge

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
