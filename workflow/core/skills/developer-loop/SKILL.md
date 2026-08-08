---
name: developer-loop
description: "Complete a root work item and its sub-work items by delegating each ready task to Developer. Use for autonomous implementation of a work item tree."
argument-hint: "Root work item ID (e.g. 123)"
---

## Input

A root work item ID is required. Ask for it when absent.

## Flow

### 1. Create Plan

Invoke Planner with only the root work item ID. Continue when Planner has created `/memories/session/plan-<root-ID>.md`; otherwise enter [Recovery](#recovery).

### 2. Prepare branch

Record the current commit as the review baseline. On `main` or `release/*`, create and switch to `feature/dev-<root-ID>`; use `hotfix/dev-<root-ID>` when the root work item explicitly calls for a hotfix.

### 3. Loop

Repeat:

1. Read `/memories/session/plan-<root-ID>.md`.
2. When every task is checked (`[x]`), leave the loop.
3. Select an unchecked task whose listed dependencies are checked. Use `### Reports` to account for downstream blockers or ordering changes; otherwise use list order. If no task is ready, enter [Recovery](#recovery) with the dependency deadlock.
4. Select Developer's model: `GPT 5.6 Luna` for simple or documentation tasks, `Claude Sonnet 5` for implementation, or `Claude Opus 5` for critical, complex, or error-sensitive work.
5. Invoke Developer with only the root ID and selected sub-work-item ID. When the selected plan entry is the root task, provide only the root ID.
6. Accept `SUCCESS` only when the response contains that single status keyword and the selected plan entry is now checked, then repeat from step 1. On `ALL_ISSUES_CLOSED`, invoke Planner to refresh the plan and leave the loop only when every entry is checked. For `FAILED`, an empty response, conflicting status keywords, or an unmet status condition, enter [Recovery](#recovery).

### 4. Review

Invoke code-review with GPT 5.6 Sol and compare the branch to the recorded baseline. Triage every finding, then remediate accepted findings directly:

1. Send all accepted findings to one Developer when they form one coherent task. Otherwise partition them into non-overlapping logical groups and invoke one Developer per group sequentially.
2. Provide each group as the task description, including the exact findings and the root work item ID as context for the originating spec and commit traceability. State that the root ID is context only; Developer must leave tracker and plan status unchanged.
3. Require `SUCCESS` for each group; enter [Recovery](#recovery) for any other response. After all groups succeed, rerun code-review against the same baseline. Continue until no accepted finding remains.

### 5. Completion

Complete only when the latest plan has every task checked and review has no unaddressed accepted finding. Summarize the completed work and present relevant follow-up options.

## Recovery

Present the failed step and available evidence, then follow Orchestrator's `## Handling Errors` procedure. Resume from the plan after a successful retry; preserve unchecked state when skipping or aborting.
