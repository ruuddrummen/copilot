---
name: ralph-loop
description: This skill is used to guide Ralph, the AI assistant, through a focused work loop on a single task. Use when you want Ralph to autonomously select and work on tasks from a GitHub issue until all tasks are completed, providing updates and maintaining a clear record of work done.
disable-model-invocation: true
---

1. Make sure a GitHub issue ID is provided and prompt the user otherwise.
2. Create a session memory file at `/memories/session/issue-<ID>-notes.md` if it does not exist yet, with the issue ID, title and a placeholder task list. The sub agents will use this memory file to track progress and log notes about the tasks they work on.
3. Repeat the following step until all tasks under the GitHub issue are completed:
   - Create a sub agent with the [ralph-prompt](./assets/ralph-prompt.md) (verbatim), the GitHub issue ID and the location of the memory file.
