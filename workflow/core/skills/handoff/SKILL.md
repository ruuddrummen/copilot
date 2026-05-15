---
name: handoff
description: "Write a concise handoff note so work can be resumed in a follow-up conversation. Use when the user asks for a handoff, wants to pause, or needs to summarise current state before context is lost."
---

Write a handoff note to a new `handoffs/<YYYY-MM-DD>-<topic>.md` file at the workspace root. Create the `handoffs/` directory if it does not exist and ensure it is listed in `.gitignore` (handoffs are local working notes, not committed history). Use a short kebab-case topic that mirrors the work.

The note itself is the markdown <template> below. Keep it concise, to the point, and focussed on current and next steps. Use bullets, do not use prose language.

- If applicable, suggest next skills to use.
- If applicable, refer to file paths and line numbers instead of verbally describing code.

After writing the file, summarise its location in chat (one line). Do not paste the full contents back into chat — the file is the deliverable.

<template>

# Handoff — <topic>

**Phase:** <current workflow phase>
**Next steps/skills:** <what to do next>
**Current (root) work items:** <issue refs or "none">
**Key decisions:** <brief bullets>
**Context:** <any other notes needed to pick up where we left off | concise!>

</template>
