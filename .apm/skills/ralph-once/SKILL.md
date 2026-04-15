---
name: ralph-once
description: This skill is used to instruct Ralph, the AI assistant, to work on a single task until completion. Use when you want Ralph to focus on a specific task, provide updates, and maintain a clear record of work done.
disable-model-invocation: true
---

1. Make sure an issue ID is provided and prompt the user otherwise.
2. Use GitHub tools to review the issue and all child tasks, and select the item you will work on. This should be the one YOU decide has the highest priority. This is not necessarily the first item in the list.
3. Check any feedback loops, such as types and tests.
4. Make a git commit of the task after completing it, with a clear commit message describing the changes made. Reference the issue ID in the commit message for traceability.
5. Complete the task in GitHub by setting its status to "Done".

When you are done with your task, respond with a small confirmation message. If this was the final issue that needed work, or you did not find any task to work on, include "ALL_ISSUES_CLOSED" in your response.

**CRITICAL: ONLY WORK ON A SINGLE TASK.**
