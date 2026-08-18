---
name: code-quality-review
description: Discipline for producing and reviewing high-quality code. Use this skill whenever writing, modifying, or reviewing code — new features, bug fixes, refactors, PR reviews, or "does this look right" checks. Use it especially before presenting any diff as finished, and whenever asked to review someone else's (or a previous session's) code, even if the user doesn't say "review".
---

# Code Quality & Review Workflow

Two workflows live here: **producing** code that survives review, and **reviewing** code (yours or others'). The common principle: the author's perspective is blind to the author's mistakes, so quality comes from deliberately switching perspectives before calling anything done.

## Part 1 — Producing code

### Before writing

- **Read the neighborhood first.** Look at how adjacent code handles errors, naming, logging, and structure. Match the codebase's existing conventions even where you would personally choose differently — consistency beats local elegance.
- **Search for prior art.** Before writing a helper, check whether the codebase (or its dependencies) already provides it. Reinvented utilities are a maintenance tax and a review red flag.
- **Separate refactoring from behavior change.** If a change needs both, do them as distinct steps (ideally distinct commits). A diff that mixes them is unreviewable: the reader cannot tell which changes are supposed to alter behavior.

### While writing

- **Keep the diff minimal.** Touch only lines the task requires. No drive-by reformatting, no speculative generality, no "while I'm here" changes — note those for the user instead.
- **Handle the failure paths.** For every external interaction (I/O, network, parsing, user input), decide explicitly what happens on failure. Silent failure and swallowed exceptions are the most common class of bug in generated code.
- **Comments explain *why*, not *what*.** Add a comment only where the code cannot speak for itself: a non-obvious constraint, a workaround with a reason, a deliberate deviation from convention.
- **When changing a signature or contract, find every caller.** Grep for usages before and after. A compiling change with an un-updated caller is a bug you shipped.

### Before presenting — the self-review pass

This pass is mandatory for any non-trivial diff. Re-read the **complete diff** (not the code in the editor — the diff) in a different mode: you are now a skeptical reviewer who did not write this and does not trust it.

Checklist:

1. **Correctness:** edge cases (empty, zero, None/null, unicode, concurrent access), off-by-one boundaries, error paths actually reachable and handled.
2. **Evidence:** *never claim code works without evidence.* Run the tests, run the script, exercise the path. If you cannot run it, say exactly that: "written but not executed; the risk areas are X and Y." A confident false "done" destroys more trust than any bug.
3. **Security basics:** no secrets in code or logs, inputs validated at trust boundaries, no injection vectors (SQL, shell, path traversal), least privilege on anything touching keys or funds.
4. **Leftovers:** debug prints, commented-out code, TODO-without-owner, unused imports, dead branches.
5. **Tests:** does the change deserve a test? If a bug fix — write the test that would have caught the bug, and confirm it fails before the fix and passes after.
6. **Honest summary:** when presenting, state what was verified, what was not, and any decisions the reviewer should weigh in on. Surface your own doubts; hiding them just relocates the cost to production.

## Part 2 — Reviewing code

When asked to review a diff, PR, or file:

### Order of operations

1. **Understand intent before reading lines.** Read the description/ticket, then skim the whole diff for shape. Line-by-line reading without knowing the goal produces nitpicks and misses design flaws.
2. **Big issues first.** Does the approach solve the actual problem? Is there a simpler design? Any correctness or security flaw? Only after these, descend to style.
3. **Check what's absent.** Missing error handling, missing tests, un-updated callers, docs that now lie. The bugs in a diff are usually visible; the bugs *around* a diff are the ones reviews exist to catch.

### Writing the review

- **Categorize every comment:** `[blocking]` must fix before merge, `[suggestion]` worth considering, `[nit]` take or leave. This lets the author triage instead of treating twelve comments as twelve demands.
- **Be specific and constructive:** point to the exact line, explain the failure scenario, and where practical propose the fix. "This is wrong" without a scenario is noise.
- **Verify before asserting.** If you claim code has a bug, trace the actual failure path first. A review containing one false claim gets the whole review discounted.
- **Say what's good.** Noting solid choices isn't politeness — it tells the author which patterns to keep.

### Verdict

End every review with an explicit verdict: **approve**, **approve with nits**, or **request changes**, plus the one-line reason. A review without a verdict forces the reader to infer one.

## Anti-patterns to catch in yourself

- **Author's blindness:** reviewing your own diff by re-reading your intentions instead of the text. Review the diff, coldly.
- **Green-checkmark theater:** "all tests pass" when you didn't run them, or ran a subset.
- **Nit-storming:** ten style comments and zero engagement with the design.
- **Scope-creep review:** demanding improvements unrelated to the diff's purpose. File them separately.
