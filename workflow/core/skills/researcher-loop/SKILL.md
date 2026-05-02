---
name: researcher-loop
description: "Run Researcher in a loop to autonomously complete all research subtasks under a research work item. Use when: you want to work through every research sub-task on a work item until done; autonomous research loop; run all research tasks."
argument-hint: "Root work item ID (e.g. 123)"
---

## Prerequisites

A work item ID **must** be provided. If missing, prompt the user for it using the Ask Questions tool.

## Flow

### 1. Determine Work Item Structure

Read the root work item to check whether it has sub-work items.

- **No sub-work items**: Skip to the [Single Task Path](#single-task-path).
- **Has sub-work items**: Continue to step 2.

### 2. Create Plan

**Always invoke the Planner agent** with the root work item ID. The Planner will read all sub-work items, analyse the work, and create the plan file at `/memories/session/plan-<work-item-ID>.md`. There are no exceptions — do not skip or substitute the Planner agent regardless of how the work items were created or what context is already available.

### 3. Loop

Repeat until all tasks are complete:

1. **Read the plan file** at `/memories/session/plan-<work-item-ID>.md`.
2. **Identify the next parallel group**: collect all unchecked tasks (`[ ]`) whose dependencies are all checked (`[x]`). These tasks have no unmet dependencies and can run simultaneously.
3. **Invoke Researcher in parallel** for each task in the group — pass both the **root work item ID** and the **selected sub-work item ID** to each Researcher instance. Do not provide extra context or guidance beyond the work item IDs — the Researcher reads the plan file and work items itself.
4. **Wait for all parallel Researcher calls to complete**, then check each response for a status keyword:
   - **`SUCCESS`**: Task completed successfully.
   - **`FAILED`** or no recognized keyword: Enter the [Error Recovery](#error-recovery) flow for that task.
5. **Commit written files** after each parallel group in which at least one task succeeded. Use `execute` with these git commands:
   - `git add .`
   - `git commit -m "research(<root-ID>): complete subtask(s) #<id1> [#<id2> ...]"` — list all IDs of tasks that returned `SUCCESS` in this group.
6. After handling all responses (and retries), re-read the plan file and check whether all checkboxes are marked `[x]`. If so, exit the loop.
7. **Surface proposed learnings.** If any Researcher response includes a `### Proposed Learning`, or if a loop-level pattern emerges that could improve the workflow's agents, skills, or instructions, follow the Orchestrator's `## Upstream learnings` flow: summarise, recommend, consult the user, file an upstream issue only on approval. Only surface learnings that are generally applicable — not specific to the current codebase.

### 4. Completion

When all tasks are done (all checkboxes marked `[x]`):

1. Summarize what was researched and which output files were written.
2. Use the Ask Questions tool to present follow-up options to the user.

---

## Single Task Path

When the root work item has no sub-work items:

1. Invoke **Researcher** with the root work item ID. No Planner, no plan file.
2. Check the response for a status keyword and handle as in step 3.4 above.
3. If successful, commit with: `git add .` and `git commit -m "research(<root-ID>): complete #<root-ID>"`.
4. On completion, summarize and present follow-up options.

---

## Error Recovery

When Researcher fails (returns `FAILED`, no keyword, or an empty response):

1. **First failure**: Invoke Researcher again on the **same task** with this additional context:
   > "Previous attempt did not complete. Review the plan file and work item, then retry the research task."
2. **Second failure on the same task**: **Stop.** Report the failure to the user using the Ask Questions tool with these options:
   - **Skip this task** and continue with the next available group.
   - **Abort** the loop entirely and discuss the problem.
   - **Retry once more** (user's discretion).

Do not retry more than once automatically. After 2 total attempts on the same task, human judgment is required.

A failed task does not block other tasks unless they explicitly depend on it. If a failed task is listed as a dependency of a later task, skip the dependent task as well and report both as blocked.
