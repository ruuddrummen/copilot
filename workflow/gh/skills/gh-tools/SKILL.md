---
name: gh-tools
description: "GitHub operations via MCP (preferred) or `gh` CLI. It covers the canonical commands and the multi-line body pattern. ALWAYS Use when: creating, reading, updating, or closing issues or sub-issues; commenting on issues; applying or removing labels; searching code, issues, or repos; filing work items or bug reports."
---

# GitHub tools

Prefer the GitHub MCP tools when loaded; otherwise use the `gh` CLI for all operations. Agents should infer the appropriate MCP tool from the loaded tool list — this skill does not enumerate them.

**Do not write issue or comment bodies to temporary files.** The CLI reads bodies from stdin via `--body-file -`, which combined with a heredoc gives you multi-line input directly in the command. See the "Multi-line bodies" pattern below.

## Conventions (CLI)

- **Create an issue**: `gh issue create --title "..." --body "..."` for short bodies; for multi-line bodies, use the heredoc pattern below.
- **Create a sub-issue** (link to parent): `ISSUE_ID=$(gh issue create --title "..." --body "..." --json id --jq .id) && gh api repos/{owner}/{repo}/issues/{parent_number}/sub_issues --method POST -F sub_issue_id="$ISSUE_ID"`
- **Read an issue**: `gh issue view <number> --comments`. Filter comments / fetch labels with `--json` + `jq` as needed.
- **List issues**: `gh issue list --state open --json number,title,body,labels,comments --jq '[.[] | {number, title, body, labels: [.labels[].name], comments: [.comments[].body]}]'` with appropriate `--label` and `--state` filters.
- **Comment on an issue**: `gh issue comment <number> --body "..."`
- **Apply / remove labels**: `gh issue edit <number> --add-label "..."` / `--remove-label "..."`
- **Close**: `gh issue close <number> --comment "..."`
- **Search** (code / issues / repos): `gh search code "query"`, `gh search issues "query"`, `gh search repos "query"`

Infer the repo from `git remote -v` — `gh` does this automatically when run inside a clone.

## Multi-line bodies

Pipe a heredoc into `--body-file -` on `gh issue create`, `gh issue edit`, `gh issue comment`, `gh pr create`, etc. instead of writing to a temp file:

```bash
gh issue create --title "PRD: ..." --label prd --body-file - <<'EOF'
## Title

Markdown body.
...
EOF
```
