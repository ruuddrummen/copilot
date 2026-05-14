---
name: handoff
description: "Write a concise handoff note so work can be resumed in a follow-up conversation. Use when the user asks for a handoff, wants to pause, or needs to summarise current state before context is lost."
---

Write a summary of the current state, in a markdown code block, that the user can copy. Keep it concise, to the point, and focussed on current and next steps. Use bullets, do not use prose language.

- If applicable, suggest next skills to use
- If applicable, refer to file paths and line numbers instead of verbally describing code.

<template>

```markdown
## Handoff — <topic>

**Phase:** <current workflow phase>
**Next steps/skills:** <what to do next>
**Current (root) work items:** <issue refs or "none">
**Key decisions:** <brief bullets>
**Context:** <any other notes needed to pick up where we left off | concise!>
```

</template>
