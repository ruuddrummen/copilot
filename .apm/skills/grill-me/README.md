# grill-me

Interviews the user relentlessly about a plan or design until reaching shared understanding.

This skill is inspired by the `grill-me` skill developed by Matt Pocock, and is further optimized for the pay-per-request model of GitHub Copilot. You can find the original and other skills [here](https://github.com/mattpocock/skills). To see this and other skills in action, check out Matt's demo video [Building a REAL feature with Claude Code: every step explained](https://www.youtube.com/watch?v=hX7yG1KVYhI).

## Install

```
apm install BNGBank/agentic/skills/grill-me
```

> **Tip:** This skill can be used on its own, but works best when paired with research extensions such as [`BNGBank/agentic/tech-research`](https://github.com/BNGBank/agentic/blob/main/tech-research), which enables the agent to look up facts, verify claims, and ground questions in up-to-date technical context during the session.

## Usage

The primary way to start a grill session is with the `/grill-me` slash command in chat. You can kick off a session with any idea, plan, or design — no need to have it fully formed.

```
/grill-me I want to build a rate-limiting service for our API gateway
```

To keep a session shorter, just say so in your prompt:

```
/grill-me I want to validate my caching strategy. Keep the grilling session short.
```

The skill will walk through every branch of the decision tree, asking targeted questions (with recommended answers) until a shared understanding is reached. It can also be triggered when the user mentions "grill me" or asks to stress-test a plan.
