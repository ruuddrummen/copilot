---
name: create-plan
description: "Read work items and produce an ordered implementation plan with dependencies. Use when: creating a plan file for a root work item and its sub-work items."
user-invocable: false
---

## Input

You receive a **root work item ID** and need to produce an ordered plan.

## Process

1. If the sub-work item content is already available in your context (e.g., you just created them), use that directly — do not re-read from the platform.
2. Otherwise, read the root work item and all its sub-work items to understand individual tasks.
3. Analyze dependencies between tasks — which tasks must complete before others can start.
4. Order the tasks, placing independent tasks first and dependent tasks after their prerequisites.
5. Create the plan file at `/memories/session/plan-<work-item-ID>.md`.

## Output

Create a plan file using the format below. Use the exact format for tasks and notes as shown, replacing placeholders with actual work item IDs and titles.

```markdown
# Plan for #<ID> — <Title>

## Tasks
1. [ ] **#<sub-ID> — <Title>** (no dependencies)
2. [ ] **#<sub-ID> — <Title>** (depends on #<sub-ID>)
3. [ ] **#<sub-ID> — <Title>** (depends on #<sub-ID>, #<sub-ID>)

## Notes
_Bullet list of crucial cross-task dependencies and edit points, which can not be inferred when only reading the root issue and working on one of the sub-issues. Do not repeat information that is already present in the work items. Focus on insights that emerge from analyzing them together. Implementers will describe their findings and decisions below by themselves. Use ultra concise language._

## Reports
<!-- This is where implementers of the issues append their reports after they complete each task, focussing on their findings, decisions, and any deviations from the original plan that affect later tasks. Use ultra concise language. -->
```

## Constraints

- **Read-only on the codebase.** Do not edit code, run commands, or search the codebase.
- **One-shot.** Produce the plan and return. Do not loop or wait for further instructions.
- **Do not select a task.** The caller decides what to work on next based on the plan.
- **Short response.** The plan file is the output. Do not include any other text in your response unless necessary.
