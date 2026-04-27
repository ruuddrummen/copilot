---
name: create-plan
description: "Read work items and produce an ordered implementation plan with dependencies. Use when: creating a plan file for a root work item and its sub-work items."
user-invocable: false
---

## Input

You receive a **root work item ID** and need to produce an ordered plan.

## Process

0. If you are working with an Azure DevOps repository, read `AZURE_DEVOPS.md` for context.
1. If the sub-work item content is already available in your context (e.g., you just created them), use that directly — do not re-read from the platform.
2. Otherwise, read the root work item and all its sub-work items to understand individual tasks.
3. Analyze dependencies between tasks — which tasks must complete before others can start.
4. Order the tasks, placing independent tasks first and dependent tasks after their prerequisites.
5. Create the plan file at `/memories/session/plan-<work-item-ID>.md`.

## Output

Create a plan file using the format below. Use the exact format for tasks and notes as shown, replacing placeholders with actual work item IDs and titles.

```markdown
## Plan for #<ID> — <Title>

### Tasks
1. [ ] **#<sub-ID> — <Title>** (no dependencies)
2. [ ] **#<sub-ID> — <Title>** (depends on #<sub-ID>)
3. [ ] **#<sub-ID> — <Title>** (depends on #<sub-ID>, #<sub-ID>)

### Notes
- Crucial cross-task dependencies, blockers or notes, which can not be easily inferred when only reading the PRD and working on one of the sub-issues.

### Reports
<!-- This is where implementers of the issues append their planning-focused reports after they complete each task -->
```

## Constraints

- **Read-only on the codebase.** Do not edit code, run commands, or search the codebase.
- **One-shot.** Produce the plan and return. Do not loop or wait for further instructions.
- **Do not select a task.** The caller decides what to work on next based on the plan.
- **Keep it concise.** Task descriptions should be one line. Dependency annotations should reference work item IDs only.
