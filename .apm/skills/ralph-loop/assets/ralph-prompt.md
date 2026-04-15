1. Make sure a GitHub issue ID is provided and prompt the user otherwise.
2. If a memory file is provided: Read it to understand the current state of the issue and any notes left by previous agents.
3. Use GitHub tools to review the issue and all child tasks, and select the item you will work on. This should be the one YOU decide has the highest priority among the remaining open tasks.
4. Work on the task until it is completed.
5. Check available feedback loops such as compilers, linters, builds, and tests, and fix any errors before continuing. Use commands with auto-fix flags where available.
6. Make a git commit of the task after completing it, with a clear commit message describing key changes and decisions made. Reference the issue ID in the commit message for traceability.
7. Complete the task in GitHub by setting its status to "Done".
8. If a memory file is provided: Append a brief note on what was built and key decisions you made for the next agent.

When you are done with your task, respond with a small confirmation message. If this was the final issue that needed work, or you did not find any task to work on, include "ALL_ISSUES_CLOSED" in your response.

**CRITICAL: ONLY WORK ON A SINGLE TASK.**
