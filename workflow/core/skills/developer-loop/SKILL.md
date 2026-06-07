---
name: developer-loop
description: "Run Developer in a loop to autonomously complete all tasks under a work item. Use when: you want Developer to work through every sub-task on a work item until done; implement work items; autonomous coding loop; run all tasks."
argument-hint: "Root work item ID (e.g. 123)"
---

## Prerequisites

A work item ID **must** be provided. If missing, ask the user for it.

## Flow

### 1. Create Plan

If you have all work items in your context, use the `create-plan` skill. In all other cases, **always invoke the Planner agent** to create the plan file, and ONLY provide the root work item ID. Do not read or analyze work items yourself, do not create the plan file yourself, and do not invoke the `create-plan` skill directly. The Planner agent handles all of this for you — it reads the work items, analyzes dependencies, creates the plan file, and returns control to you when done.

### 2. Prepare branch

If you are on the `main` or a `release/*` branch, create and switch to a new branch named `(feature|hotfix)/dev-<root-ID>` before starting the loop.

### 2. Loop

Repeat until all tasks are complete:

1. **Read the plan file** at `/memories/session/plan-<work-item-ID>.md`.
2. **Pick the next task**: select an unchecked task (`[ ]`) whose dependencies are all checked (`[x]`). Use your judgment to pick the best candidate — consider reports from completed tasks (in the `### Reports` section of the plan file) that may indicate blockers, risks, or ordering recommendations. Default to list order when there is no reason to deviate.
3. **Select a model** for Developer to use on this task, based on the task description and your knowledge of the models available to you. Consider the complexity of the task. Available models: `Claude Haiku 4.5` / `GPT 5.4-mini` (simple or docs only), `Claude Sonnet 4.6` / `GPT 5.4` (simple implementation work), `Claude Opus 4.8` / `GPT 5.5`  (for critical, complex or error-sensitive tasks where quality is paramount).
3. **Invoke Developer** with both the **root work item ID** and the **selected sub-work item ID**. Do not tell Developer which approach to take or provide additional context beyond the work item IDs — Developer reads the plan file and work items themselves.
4. **Check Developer's response** for a status keyword:
   - **`SUCCESS`**: Continue to the next iteration (step 1).
   - **`ALL_ISSUES_CLOSED`**: Exit the loop — all work is done.
   - **`FAILED`**: Enter the [Error Recovery](#error-recovery) flow.
   - **No recognized keyword**: Report this to the user, but determine success or failure based on the response and continue accordingly.
5. **Surface proposed learnings.** If Developer's response includes a `### Proposed Learning`, or a loop-level pattern emerges, follow the Orchestrator's `## Upstream learnings` flow: summarise, recommend, consult the user, file only on approval. Apply the full bar — **stack-agnostic** and **workflow scope** (would adoption edit a file under `workflow/` and change agent behaviour?). Coding tips belong in repo conventions.

### 3. Completion

When all tasks are done (all checkboxes marked `[x]` or Developer returns `ALL_ISSUES_CLOSED`):

1. Summarize what was accomplished.
2. Present follow-up options to the user.

## Single Task Path

When the root work item has no sub-work items:

1. Invoke **Developer** with the root work item ID. No Planner, no plan file.
2. Check Developer's response for a status keyword and handle as in step 2.4 above.
3. On completion, summarize and present follow-up options.

## Error Recovery

When Developer fails (returns `FAILED`, no keyword, or an empty response):

- If the developer provided a response, triage the failure. If you can't fix it, report the error to the user.
- If the response is empty or does not contain a recognized status keyword, report to the user. Probable cause: `chat.agent.maxRequests` has been exceeded.
