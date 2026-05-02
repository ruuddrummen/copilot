---
name: to-prd
description: "Turn the current conversation context into a PRD and submit it as a work item. Use when user wants to create a PRD, write a product requirements document, document a feature spec, or submit a design as a work item."
argument-hint: "Optional: work item platform (github/ado) or additional context"
---

This skill takes the current conversation context and codebase understanding and produces a PRD. Do NOT interview the user — just synthesize what you already know.

## Process

1. Explore the repo to understand the current state of the codebase, if you haven't already.

2. Sketch out the major modules you will need to build or modify to complete the implementation. Actively look for opportunities to extract deep modules that can be tested in isolation.

A deep module (as opposed to a shallow module) is one which encapsulates a lot of functionality in a simple, testable interface which rarely changes.

Check with the user that these modules match their expectations. Check with the user which modules they want tests written for.

3. Write the PRD using the appropriate platform template from the `templates/` directory. Then submit the PRD as a work item. Prefix the title with `PRD:` and include tag `prd`.
