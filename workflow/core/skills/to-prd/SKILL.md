---
name: to-prd
description: "Turn the current conversation context into a PRD and submit it as a work item. Use when user wants to create a PRD, write a product requirements document, document a feature spec, or submit a design as a work item."
argument-hint: "Optional: additional context for the PRD"
---

This skill takes the current conversation context and codebase understanding and produces a PRD. Do NOT interview the user — just synthesize what you already know.

## Process

1. Explore the repo to understand the current state of the codebase, if you haven't already.

2. Sketch out the major modules you will need to build or modify to complete the implementation. Actively look for opportunities to extract deep modules that can be tested in isolation.

A deep module (as opposed to a shallow module) is one which encapsulates a lot of functionality in a simple, testable interface which rarely changes.

Check with the user that these modules match their expectations. Check with the user which modules they want tests written for.

3. Resolve the PRD template using this hybrid lookup:
   - If `<repo-root>/templates/prd.md` exists, use it. This lets a consumer override or localise the template without forking core.
   - Otherwise, use the bundled default at `templates/prd.md` next to this skill.

4. Write the PRD using the resolved template. Then submit the PRD as a work item via the platform skills listed in `AGENTS.md` under "Platform Skills". Prefix the title with `PRD:` and include tag `prd`.
