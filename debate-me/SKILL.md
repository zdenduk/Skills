---
name: debate-me
description: Debate a user's proposed answer, position, plan, or interpretation by inspecting both sides, steelmanning the strongest arguments, identifying weak assumptions, and taking a clear reasoned stance. Use when the user asks Codex to debate them, challenge their answer, argue both sides, inspect pros and cons, stress-test a response, or decide whether their answer is defensible.
---

# Debate Me

## Overview

Use this skill to turn a one-sided answer into a rigorous argument map. The goal is not to be contrarian; it is to test whether the user's answer survives serious opposition and to state the most defensible position.

## Debate Workflow

1. Restate the user's answer or position in neutral, precise terms.
2. Identify the core question being answered and the decision standard that matters, such as truth, usefulness, risk, ethics, cost, feasibility, or explanatory power.
3. Steelman the user's side with the strongest arguments, not just the arguments they already gave.
4. Steelman the opposing side with the strongest credible counterarguments.
5. Inspect evidence quality, hidden assumptions, definitions, incentives, edge cases, and likely failure modes on both sides.
6. Compare the sides directly instead of listing disconnected pros and cons.
7. Take a clear stance and explain what would change that stance.

## Response Shape

Prefer this structure unless the user asks for another format:

- `Your Position`: concise restatement of the user's answer.
- `Best Case For It`: strongest supporting arguments.
- `Best Case Against It`: strongest opposing arguments.
- `Key Pressure Points`: assumptions, missing evidence, tradeoffs, and edge cases that decide the debate.
- `My Stance`: clear conclusion with confidence level and conditions that could reverse it.

## Debate Standards

- Be direct and fair. Do not flatter the user's answer or attack it performatively.
- Separate empirical claims from value judgments.
- Flag uncertainty explicitly when the answer depends on missing facts.
- Avoid false balance. If one side is materially stronger, say so.
- Do not use a debate format when the user needs a factual lookup, calculation, legal/medical/financial safety guidance, or implementation work first; handle that prerequisite before debating.
- If the user's answer is ambiguous, state the interpretation being debated and proceed unless the ambiguity changes the conclusion.
