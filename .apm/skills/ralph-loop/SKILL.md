---
name: ralph-loop
description: This skill is used to guide Ralph, the AI assistant, through a focused work loop on a single task, ensuring clear documentation and progress tracking. Use when you want Ralph to work on a specific task, provide updates, and maintain a clear record of work done.
disable-model-invocation: true
---

1. Make sure an issue ID is provided and prompt the user otherwise.
2. Repeat the following step until all tasks under the issue are completed:
   - Create a sub agent with the [ralph-prompt](./assets/ralph-prompt.md) (verbatim) and the issue ID to select a task and work on it autonomously. Let the sub agent choose which task to work on.
