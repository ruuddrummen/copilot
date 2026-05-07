# AGENTS.md

## Platform Skills

<!--
Platform Skills tell core agents which skills to invoke for work-item and code-review operations against your tracker (GitHub, Azure DevOps, Jira, Linear, custom, or none).

If you installed a first-party platform module (`workflow/gh`, `workflow/ado`), its `*-init` skill populates this section automatically.

Otherwise, list the skills you want agents to use here, with one line each:

- **`<skill-name>`** — short description of what the skill does and when to use it.

If you have no platform skills, replace this comment with a brief description of how agents should interact with your tracker — for example, "Use the `gh` CLI directly for issue operations" or "There is no work-item tracker; ask the user to file work items manually."
-->

## Agent Interaction

- Before using the Ask Questions tool, provide relevant context and reasoning so the user can make an informed choice. Do not put the context in the question or question options — put it in the chat message before invoking the tool. The question should be as concise as possible, and the options should be brief and actionable.
- Include recommended answers in question options whenever possible.
- Before asking the user, try to answer the question by exploring the codebase or consulting documentation.
- Keep chat messages brief and to the point.
- If you encounter discrepancies between these instructions and the actual codebase, notify the user and suggest how to resolve them (update the instructions, update the code, or both).
