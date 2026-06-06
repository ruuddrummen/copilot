---
name: qa
description: Interactive QA session where the user reports findings conversationally. Gather all findings first, then group related ones, then decide per group whether to file as work items or fix immediately. Use when the user wants to report bugs, do QA, file issues conversationally, or mentions "QA session".
---

# QA Session

Three phases: **Gather**, **Group**, **Resolve**. Findings can be bugs or spec changes derived from implementation insights.

## Phase 1 — Gather

The user posts findings as they encounter them. Make each one unambiguous, then get out of the way for the next.

For each finding:

1. If it is already 100% clear, acknowledge briefly. Do not restate, question, or propose fixes.
2. If anything is unclear, explore the codebase first. After exploring:
   - If now clear, acknowledge briefly.
   - If still ambiguous, prefer **stating assumptions for the user to confirm** over asking open questions.
3. Do not propose fixes or diagnose root causes. That's for later phases.

Keep findings in the conversation by default. If the session grows long enough that context is at risk, switch to appending each clarified finding to a session-memory scratch file (append-only). Acknowledge with "added to memory" — do not echo the finding back.

Continue until the user signals they are done adding findings.

## Phase 2 — Group

Triggered when the user signals done.

### Draft groups

A **group** is a set of closely related findings to address together. Groups may mix bugs and spec changes.

Findings belong in the same group when they:

- Hit the **same vertical slice** — the same end-to-end behavioral path.
- Would modify the **same surface in the same way** (same data type, module, component, writer, dialog, etc.). This is the file-level expression of the slice rule and a strong signal on its own.

Keep findings in **separate groups** when they touch genuinely different slices, or carry a meaningfully different *kind* of risk (layout vs. data-shape vs. rename, etc.), even with incidental file overlap.

Prefer fewer, well-scoped groups over many thin ones that fight each other for the same surface. Groups do **not** need vertical-slice treatment (no HITL/AFK tagging, no acceptance criteria, no user-story mapping).

### Review with the user

Present the grouping as a numbered list. For each group:

- **Title**
- **Findings included** — one-liners referencing the originals
- **Blocked by** — another group, only if there's a genuine ordering dependency

Ask whether granularity, splits/merges, and blocking relationships are right. Iterate until approved.

## Phase 3 — Resolve

Ask once: **What should we do?** (e.g., `file 1–3, implement 4, drop 5`)

Options: `file` (work item), `implement` (fix now), `drop` (discard).

Parse reply, then execute in order: file (dependency order), implement, drop.

### File

Create work item from template below — standalone or as sub-item. File in dependency order so "Blocked by" can reference real IDs. Print URLs after.

<work-item-template>
## Parent

Parent work item reference (only if the QA session was running under one).

## What's wrong or needs to change

Concise description covering all findings in this group.

## Expected behavior or desired change

What it should be instead.

## How to reproduce or observe

1. Concrete numbered steps using domain terms.

Omit for pure spec changes with no reproducible symptom.

## Blocked by

- Reference to a blocking work item, if any.

Omit when none.

## Additional context

Extra observations that frame the group. Domain language only; no file paths or line numbers.
</work-item-template>

Rules:

- No file paths or line numbers — they go stale.
- Use the project's domain language (check `CONTEXT.md` or equivalent).
- Describe behaviors, not code.
- A developer should grasp the work item in 30 seconds.

### Fix immediately

Proceed to implement the fix directly, using the group description as the spec. No work item is created.

### Drop

Discard the group. Note it as dropped in the session summary.

## Re-entry

The user may add findings at any time. Return to Phase 1 for the new finding, then re-run Phase 2 over the combined set before continuing Phase 3.
