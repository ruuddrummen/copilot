---
name: create-plan
description: "Plan a root work item and its sub-work items in dependency order. Use when an implementation loop needs its canonical plan file."
user-invocable: false
---

## Input

You receive one root work item ID.

## Process

1. Read the current root work item, its comments, and every direct sub-work item with comments. When a plan already exists, read its `### Reports` section for still-relevant downstream notes.
2. Derive dependencies from explicit relationships and task content. Include only dependencies among tasks in this plan; reject cycles.
3. Order ready tasks first and dependents after their prerequisites. Mark tasks already complete in the tracker as `[x]` and all others as `[ ]`.
4. When sub-work items exist, include each exactly once. When none exist, include the root itself as the sole task.
5. Create or replace `/memories/session/plan-<root-ID>.md` using the format below. Preserve still-relevant reports for retained tasks.
6. Return only after every included task, status, and dependency agrees with the work items read in step 1.

## Output

Create a plan file using the format below. Use the exact format for tasks and notes as shown, replacing placeholders with actual work item IDs and titles.

```markdown
# Plan for #<ID> — <Title>

## Tasks
1. [ ] **#<sub-ID> — <Title>** (no dependencies)
2. [ ] **#<sub-ID> — <Title>** (depends on #<sub-ID>)
3. [ ] **#<sub-ID> — <Title>** (depends on #<sub-ID>, #<sub-ID>)

<!-- With no sub-work items, use: 1. [ ] **#<ID> — <Title>** (root task; no sub-work items) -->

## Notes
_Ultra-concise bullets for cross-task dependencies, shared edit points, or ordering risks visible only from reading the tasks together. Use `None.` when there are none._

## Reports
<!-- Implementers append only findings, decisions, and deviations that affect later tasks. -->
```

## Constraints

- Derive tasks, status, and dependencies only from current work-item content; leave the codebase untouched.
- Produce the plan in one pass and return the plan file as the only output.
- Leave task selection to the caller.
