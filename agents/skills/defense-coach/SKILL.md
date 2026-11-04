---
name: defense-coach
description: Generate mock follow-up questions from the actual implementation, let the user answer first, then check the answers against code, tests, and logs. Use after core features are done or before submission; read-only.
---

# Defense Coach

## Goal
Help the user explain **their own submission** with evidence. Follow-up questions are likely to be generated from the submitted work, so questions must come from the real code, not generic theory. This skill is read-only: do not edit code, tests, or docs, and do not run tests unless the user asks.

## 1. Gather facts
1. Read the task statement, `docs/PLAN.md`, `docs/PROGRESS.md`, `docs/DEFENSE.md`, and the actual source layout.
2. Note what is implemented, what is assumed, and what is not implemented. Do not invent files, functions, or behavior. If something cannot be confirmed in the code, say so.

## 2. Ask (3-6 questions, one at a time or as a short list)
Choose from these types, always anchored to a real file, function, or decision in this project:
1. **Consistency under concurrent change:** what if input data or settings change while a long operation is running or between steps? How is a stale result prevented from looking current? Compare with alternatives and the cost accepted.
2. **Module and data flow:** walk through the whole flow from input to final output by module. Where is each piece of state stored and who owns it?
3. **Rule verification:** how can one confirm the output obeys the task's rules? Which verification path exists, and which rules are only assumed or not checked?
4. **Failure behavior:** invalid input, a check that fails after validation, a failure while saving. What happens to existing data and what the user sees?
5. **Optimization limits:** where the solution uses heuristics or search limits, what breaks with much larger data or extreme parameters? What would be checked next, and on what evidence rather than AI explanation?
6. **Incident timeline:** pick one real failure from PROGRESS. In order: what the AI first reported, what actually happened, which AI diagnosis or fix was accepted, corrected, or discarded, and the evidence for each judgment.

Do not reveal suggested answers before the user answers.

## 3. Evaluate the answers
After the user answers, compare against code, tests, and PROGRESS records. Mark each point as:
- **Supported** (name the file, command, or test),
- **Contradicted** (what the code actually does),
- **Unverifiable** (not in code or logs; do not accept it as fact).

Flag: answers that rely on "the AI said so", generic claims not tied to this project, invented files or behavior, and padding with unrelated achievements. Suggest one concrete thing to check or improve per weak answer.

## 4. Record
Only if the user asks, propose rows for the DEFENSE "understanding check" table. Never mark a question as understood before the user has answered it. Keep at most 3 important questions open.
