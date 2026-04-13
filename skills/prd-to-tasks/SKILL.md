---
name: prd-to-tasks
description: Break a PRD into independently-grabbable Azure DevOps tasks using tracer-bullet vertical slices. Use when user wants to convert a PRD to tasks, create implementation tickets, or break down a PRD into work items.
---

# PRD to Tasks

Break a PRD backlog item into independently-grabbable child tasks using vertical slices (tracer bullets).

## Process

### 1. Locate the PRD

Ask the user for the PRD Azure DevOps backlog item ID.

If the PRD is not already in your context window, fetch it using #tool:microsoft/azure-devops-mcp/wit_get_work_item with the given ID.

### 2. Explore the codebase (optional)

If you have not already explored the codebase, do so to understand the current state of the code.

### 3. Draft vertical slices

Break the PRD into **tracer bullet** tasks. Each task is a thin vertical slice that cuts through ALL integration layers end-to-end, NOT a horizontal slice of one layer.

Slices may be 'HITL' or 'AFK'. HITL slices require human interaction, such as an architectural decision or a design review. AFK slices can be implemented and merged without human interaction. Prefer AFK over HITL where possible.

<vertical-slice-rules>
- Each slice delivers a narrow but COMPLETE path through every layer (schema, API, UI, tests)
- A completed slice is demoable or verifiable on its own
- Prefer many thin slices over few thick ones
</vertical-slice-rules>

### 4. Quiz the user

Present the proposed breakdown as a numbered list. For each slice, show:

- **Title**: short descriptive name (in Dutch)
- **Type**: HITL / AFK
- **Blocked by**: which other slices (if any) must complete first
- **User stories covered**: which user stories from the PRD this addresses

Ask the user:

- Does the granularity feel right? (too coarse / too fine)
- Are the dependency relationships correct?
- Should any slices be merged or split further?
- Are the correct slices marked as HITL and AFK?

Iterate until the user approves the breakdown.

### 5. Create the child tasks

For each approved slice, create an Azure DevOps Task using #tool:microsoft/azure-devops-mcp/wit_create_work_item with `workItemType: "Task"` and `project: "BNGBank"`. Use the task body template below. Write all titles and descriptions in Dutch.

Create tasks in dependency order (blockers first) so you can reference real work item IDs in the "Geblokkeerd door" field.

After creating each task, immediately link it as a child of the parent PRD backlog item using #tool:microsoft/azure-devops-mcp/wit_work_items_link with `type: "parent"`.

<task-template>
## Wat te bouwen

Een beknopte beschrijving van dit verticale segment. Beschrijf het end-to-end gedrag, niet de laag-voor-laag implementatie. Verwijs naar specifieke secties van de bovenliggende PRD in plaats van inhoud te dupliceren.

## Acceptatiecriteria

- [ ] Criterium 1
- [ ] Criterium 2
- [ ] Criterium 3

## Geblokkeerd door

- Geblokkeerd door #<work-item-id> (indien van toepassing)

Of "Geen - kan direct worden gestart" als er geen blokkades zijn.

## User stories afgedekt

Referentie per nummer uit de bovenliggende PRD:

- User story 3
- User story 7

</task-template>

Do NOT modify the parent PRD backlog item.
