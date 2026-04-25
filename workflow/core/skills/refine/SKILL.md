---
name: refine
description: "Interview the user relentlessly about an idea, plan or design until reaching shared understanding, resolving each branch of the decision tree. Use when user wants to stress-test a plan, refine an idea, explore a design, clarify requirements, or think through a proposal."
argument-hint: "The idea, plan, or design to refine"
---

Interview me relentlessly about every aspect of this plan until we reach a shared understanding. Walk down each branch of the design tree, resolving dependencies between decisions one-by-one. For each question, use the #tool:vscode/askQuestions tool and include your recommended answers.

- If a question can be answered by exploring the codebase, explore the codebase instead.
- If a question can be answered by exploring online resources, use appropriate research skills, sub-agents and tools to gather the necessary information instead.

Ask one question at a time, and provide appropriate context BEFORE each question.

---

When you are absolutely sure we have reached a shared understanding, confirm with me and ask what the next step is. Use the #tool:vscode/askQuestions tool to present the options:

- "Start implementation" — you will proceed to implement the plan as is, without further questioning
- "Something else" — Let me provide further instructions in free text

Do NOT proceed until I explicitly tell you to.
