---
name: task-planning
description: Plan and decompose non-trivial work before executing it. Use this skill whenever a task involves more than one file, more than one step, any ambiguity about the goal, or any risk of wasted work — including feature implementation, refactors, migrations, debugging sessions, infrastructure changes, and research tasks. Use it even if the user doesn't say "plan"; if you are about to start a multi-step task without a plan, that is the trigger.
---

# Task Planning & Decomposition

The purpose of planning is not ceremony. It is to spend a small number of tokens up front to avoid spending a large number of tokens on work that gets thrown away. Most wasted effort in agentic sessions comes from three failure modes: building on an unverified assumption, discovering a blocker late that should have been probed first, and scope drift. This skill exists to prevent those three things.

## Step 0: Classify before you plan

Not every task deserves a plan. Overhead must be proportional.

- **Trivial** (single file, obvious change, reversible): just do it. Planning a one-line fix wastes the tokens planning is meant to save.
- **Medium** (2–5 steps, low ambiguity): write a 3–6 line plan inline before acting. No document needed.
- **Complex** (multi-file, ambiguous, risky, or long-horizon): follow the full process below, and track it in a todo list or plan file so state survives context pressure.

If unsure which bucket a task is in, spend one minute investigating and then classify. Misclassifying complex work as trivial is the expensive mistake; the reverse merely costs a paragraph.

## Step 1: Restate the goal and the definition of done

Before touching anything, state in one or two sentences: what the user actually wants, and how you will know it is finished. If you cannot write a definition of done, you do not understand the task yet — ask one precise question rather than guessing. A wrong guess executed confidently costs far more than a clarifying question.

Distinguish the *goal* from the *requested method*. If the user asks for a method that won't achieve their stated goal, say so before executing.

## Step 2: Investigate before planning

Plans written from assumptions are fiction. Before decomposing:

- Read the code/config that the task touches. Search first (grep, glob), then read narrowly. Do not read entire directories when a targeted search answers the question.
- Identify existing conventions, utilities, and patterns. The plan should extend the codebase's existing shape, not fight it.
- List the assumptions you are making. Mark each one **verified** (you read the evidence) or **unverified**. Any unverified assumption that the whole plan rests on must either be verified now or explicitly flagged to the user.

## Step 3: Decompose into verifiable steps

Break the work into steps where each step:

1. Has a **completion test** — a concrete way to know it worked (a command that passes, a file that exists, an output that matches). "Implement X" is not a step; "implement X and confirm test Y passes" is.
2. Is **as independent as possible** — a failure in step 4 should not silently invalidate steps 1–3.
3. Leaves the system in a **working or recoverable state**. Prefer sequences where you could stop after any step without leaving things broken.

## Step 4: Front-load the risk

Identify the step most likely to fail or to invalidate the plan — the unfamiliar API, the permission you may not have, the library that may not support the feature. **Do that step, or a minimal spike of it, first.** Discovering a fatal blocker at step 1 costs a probe; discovering it at step 7 costs the whole session.

## Step 5: Declare scope boundaries

Write down what is explicitly out of scope — the tempting adjacent refactor, the unrelated bug you noticed. Note discoveries for the user instead of fixing them silently. Scope drift is the quiet killer of long sessions: each individual expansion looks reasonable, and the sum blows the budget and muddies the diff.

## Step 6: Execute with checkpoints, and know when to re-plan

- Update the plan/todo state as steps complete, in the moment — not reconstructed later from memory.
- **Re-plan trigger:** if two consecutive steps fail, or reality contradicts a load-bearing assumption, stop executing. Do not push harder on a broken plan. Say what changed, revise the plan, and continue.
- If the session may end before the work does, write a handover: current state, what's done, what's next, open questions. Assume the reader (a future session, possibly a different model) has zero context beyond what you write.

## Economy rules (apply throughout)

- One thorough brief beats twenty micro-iterations. When asking the user something, batch the questions.
- Never re-read files that have not changed. Trust your earlier reading unless you edited the file or something else did.
- Prefer targeted searches over broad reads; prefer reading the function over reading the file; prefer reading the file over reading the directory.
- Do not narrate what you are about to do and then do it — the plan is the narration. Execute.

## Anti-patterns to catch in yourself

- **Plan-as-theater:** writing a plan and then improvising anyway. The plan is only useful if execution consults it.
- **Assumption laundering:** stating an assumption once, then treating it as fact three steps later.
- **Completion bias:** declaring a step done because the code was written, not because the completion test passed.
- **Sunk-cost execution:** continuing a failing approach because of the tokens already spent on it. Spent tokens are gone either way; only the remaining path matters.
