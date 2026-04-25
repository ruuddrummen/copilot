# AGENTS.md

## Platform Context

This repository uses {platform} for work item tracking and code reviews.

Remote URL: {remote_url}

## Terminology

| Abstract Term       | Azure DevOps         | GitHub            |
| ------------------- | -------------------- | ----------------- |
| Work item           | Product Backlog Item | Issue             |
| Sub-work item       | Task                 | Sub-issue         |
| Bug                 | Bug                  | Issue (bug label) |
| Work item reference | #<ID>                | #<ID>             |
| Code review         | Pull Request         | Pull Request      |

## Platform-specific Skills (ADO)

When performing these operations, prefer the following skills if available:

- Creating work items → use `create-backlog-item` skill
- Reverting / restoring a work item to a previous version → use `revert-work-item` skill

Always read `AZURE_DEVOPS.md` for context before using Azure DevOps tools.

## Agent Interaction

- Before using the Ask Questions tool, provide relevant context and reasoning so the user can make an informed choice. Do not put the context in the question or question options — put it in the chat message before invoking the tool. The question should be as concise as possible, and the options should be brief and actionable.
- Include recommended answers in question options whenever possible.
- Before asking the user, try to answer the question by exploring the codebase or consulting documentation.
- Keep chat messages brief and to the point.
- If you encounter discrepancies between these instructions and the actual codebase, notify the user and suggest how to resolve them (update the instructions, update the code, or both).
