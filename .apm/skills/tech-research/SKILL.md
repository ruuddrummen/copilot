---
name: tech-research
description: Researches a topic or verifies a claim using the Researcher subagent. Use when the user asks to research a topic, verify something with external resources, or fact-check a technical claim.
---

# Tech Research Skill

Delegate research and fact-checking tasks to the Tech Researcher subagent, then synthesise and present the findings clearly.

## When to Use

- User asks to research a topic ("research X", "look into X")
- User asks to verify a claim with external resources ("verify that...", "confirm whether...")
- User asks for authoritative references on a technical subject
- A previous answer needs fact-checking before acting on it

## Process

### Step 1: Formulate the Research Query

Derive a precise, self-contained research prompt from the user's request. It must include:

- The specific claim, topic, or question to investigate
- Relevant context (technology, version, product, codebase) gathered from the conversation
- The expected output format (e.g. confirm/deny the claim, list of options, pros/cons)

### Step 2: Delegate to the Tech Researcher Subagent

Invoke the Tech Researcher subagent using the #tool:agent/runSubagent tool with `agentName: "Tech Researcher"`.

Pass the formulated research prompt as the subagent's task. Instruct the Tech Researcher to:

- Cite authoritative sources (official documentation, specifications, release notes)
- Flag any conflicting or outdated information it encounters
- Distinguish clearly between confirmed facts and inferred conclusions

### Step 3: Synthesise and Present Findings

After receiving the Tech Researcher's report:

1. Summarise the key finding in 1–3 sentences.
2. State whether the original claim or question is confirmed, denied, or inconclusive.
3. List caveats or version-specific nuances if relevant.
4. Include source links from the Tech Researcher's report.

### Step 4: Propose Next Action (if applicable)

If the research reveals that a code change, fix, or decision is needed, propose a concrete next step to the user.

## Definition of Done

- [ ] Research query is specific and includes full context
- [ ] Tech Researcher subagent has been invoked with the formulated query
- [ ] Findings are summarised with a clear confirmed/denied/inconclusive verdict
- [ ] Source links are included
- [ ] Any required follow-up action is proposed

Stop once all conditions above are met. Do not implement changes unless explicitly asked.
