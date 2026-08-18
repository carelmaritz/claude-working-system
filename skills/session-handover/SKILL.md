---
name: session-handover
description: Preserve and restore working context across session boundaries — end of a session, approaching context limits, model transitions, or any long-horizon task that will outlive the current conversation. Use this skill whenever a session is winding down with unfinished work, whenever the user mentions handover, continuity, SESSION_HANDOVER, or resuming later, and at the START of any session where a handover document exists — reading one correctly matters as much as writing one.
---

# Session Handover

A session's context dies with the session. Everything not written down is lost — not summarized, not compressed, *gone*. The handover document is the only bridge, and it will be read by a reader with zero shared context: a future session, possibly a different model with different habits, possibly weeks later. Write for that reader.

The test of a good handover: the next session reaches productive work within its first few actions, without re-deriving anything, without repeating a failed approach, and without contradicting a decision already made.

## Part 1 — Writing a handover

### When to write

- **Proactively**, when context is filling up or the session is clearly longer than the task's remaining life. Do not wait until the last message — a handover written under pressure while context is being truncated is exactly when quality collapses.
- **At any natural checkpoint** in long-horizon work (a merged PR, a resolved design decision), update the standing handover file rather than rewriting it from scratch at the end.
- **On request**, obviously.

If the project has an established convention (e.g. `SESSION_HANDOVER.md`, `CLAUDE.md`), follow its existing structure and location exactly. Consistency of format is what lets the next session parse it fast.

### Required content

ALWAYS cover these sections, in this order:

```markdown
# Session Handover — [date] [one-line task description]

## Current state
What is true right now. Which branch, what's committed vs. uncommitted,
what's deployed, what's running. Facts only — verifiable by the reader.

## Completed this session
What was done AND verified. Mark anything done-but-unverified explicitly.

## Next steps
Ordered, concrete, each with its completion test. The first item should
be startable immediately with no further investigation.

## Open decisions
Questions awaiting the user or a design call. Include the options
considered and any leaning, so the discussion doesn't restart from zero.

## Failed approaches
What was tried and abandoned, and WHY. This is the highest-value,
most-omitted section — without it the next session cheerfully burns
hours rediscovering the same dead end.

## Landmines
Non-obvious constraints, fragile areas, things that look wrong but are
deliberate, environment quirks. Anything that would surprise a competent
newcomer.
```

Sections may be short. They may not be missing — an empty "Failed approaches" heading tells the reader something; an absent one tells them nothing.

### Writing rules

- **State facts, not narrative.** "Tests pass on branch `fix-tbd2` as of commit `a3f9c`" survives transfer; "we made great progress on the wallet stuff" does not.
- **Verified vs. believed.** Anything you did not personally confirm this session gets marked as unverified. Inherited claims stated as facts are how errors propagate across sessions.
- **Reference, don't duplicate.** Point to files, tickets, and commits by exact path/ID instead of pasting their content. The handover is an index into durable artifacts, not a replacement for them.
- **Include exact identifiers.** Task IDs, file paths, commands to re-run, env var names. The next session should never have to guess a path you knew.
- **Prune the stale.** When updating a standing handover file, delete what is no longer true. A handover that mixes current and obsolete state is worse than a short current one — the reader cannot tell which is which.

## Part 2 — Resuming from a handover

Reading discipline matters as much as writing discipline:

1. **Read the handover fully before acting.** Not skimmed — read. Acting on the first section alone is how "Next steps" gets done in a way that "Landmines" forbade.
2. **Verify the load-bearing claims cheaply.** Run the status commands, check the branch, confirm the state described still holds. Handovers age; environments drift; humans do things between sessions. Trust, but confirm what a single command can confirm.
3. **Respect settled decisions.** If the handover records a decision and its rationale, do not silently relitigate it because you would have chosen differently. If you believe a decision is genuinely wrong, raise it explicitly with the user — flag, don't override.
4. **Honor the failed-approaches list.** Before proposing a solution, check it isn't in that list. Proposing a documented dead end is the fastest way to prove you didn't read the handover.
5. **Say what you inherited.** Open the working session by briefly confirming your understanding of the state — one short paragraph. It costs little and catches misreadings before they compound.

## Anti-patterns to catch in yourself

- **The victory-lap handover:** a list of accomplishments with no next steps, no failures, no landmines. Useless to the successor; it exists to make the departing session look good.
- **Context-dump:** pasting raw transcript or entire files into the handover. Volume is not transfer; the reader's context is finite too.
- **Optimistic state:** describing where the work *should* be rather than where it verifiably is.
- **Write-only handovers:** maintaining the document but never consulting it on resume. The convention only works if both ends hold.
