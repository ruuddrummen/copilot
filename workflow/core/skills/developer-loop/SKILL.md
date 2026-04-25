---
name: developer-loop
description: "Run Developer in a loop to autonomously complete all tasks under a work item. Use when: you want Developer to work through every sub-task on a work item until done; implement work items; autonomous coding loop; run all tasks."
argument-hint: "Root work item ID (e.g. 123)"
---

## Prerequisites

A work item ID **must** be provided. If missing, prompt the user for it using `#tool:vscode/askQuestions`.

## Flow

### 1. Determine Work Item Structure

Read the root work item to check whether it has sub-work items.

- **No sub-work items**: Skip to the [Single Task Path](#single-task-path).
- **Has sub-work items**: Continue to step 2.

### 2. Create Plan

**Always invoke the Planner agent** with the root work item ID. The Planner will read all sub-work items, analyse the work, and create the plan file. There are no exceptions — do not skip or substitute the Planner agent regardless of how the work items were created or what context is already available.

### 3. Loop

Repeat until all tasks are complete:

1. **Read the plan file** at `/memories/session/plan-<work-item-ID>.md`.
2. **Pick the next task**: select an unchecked task (`[ ]`) whose dependencies are all checked (`[x]`). Use your judgment to pick the best candidate — consider reports from completed tasks (in the `### Reports` section of the plan file) that may indicate blockers, risks, or ordering recommendations. Default to list order when there is no reason to deviate.
3. **Invoke Developer** with both the **root work item ID** and the **selected sub-work item ID**. Do not tell Developer which approach to take or provide additional context beyond the work item IDs — Developer reads the plan file and work items itself.
4. **Check Developer's response** for a status keyword:
   - **`SUCCESS`**: Continue to the next iteration (step 1).
   - **`ALL_ISSUES_CLOSED`**: Exit the loop — all work is done.
   - **`FAILED`**: Enter the [Error Recovery](#error-recovery) flow.
   - **No recognized keyword**: Treat as a failure. Enter the [Error Recovery](#error-recovery) flow.
5. After completing a full loop, evaluate whether an Orchestrator-level learning is warranted and append to `LEARNINGS.md` if so.

### 4. Completion

When all tasks are done (all checkboxes marked `[x]` or Developer returns `ALL_ISSUES_CLOSED`):

1. Summarize what was accomplished.
2. Use `#tool:vscode/askQuestions` to present follow-up options to the user.

---

## Single Task Path

When the root work item has no sub-work items:

1. Invoke **Developer** with the root work item ID. No Planner, no plan file.
2. Check Developer's response for a status keyword and handle as in step 3.4 above.
3. On completion, summarize and present follow-up options.

---

## Error Recovery

When Developer fails (returns `FAILED`, no keyword, or an empty response):

1. **First failure**: Invoke Developer again on the **same task** with this additional context:
   > "Previous attempt did not complete. There may be uncommitted changes from the previous attempt — inspect git status and decide whether to continue from the partial changes or start fresh."
2. **Second failure on the same task**: **Stop.** Report the failure to the user using `#tool:vscode/askQuestions` with these options:
   - **Skip this task** and continue with the next one in the plan.
   - **Abort** the loop entirely and discuss the problem.
   - **Retry once more** (user's discretion).

Do not retry more than once automatically. After 2 total attempts on the same task, human judgment is required.
