# Task Brief Template

<!-- Fill and paste as the opening message of a task.
     Delete any section that's genuinely empty — an empty heading is noise.
     Ninety seconds to fill in; if it takes longer, the task probably
     needs a conversation first, not a brief. -->

**Task:** <one sentence — the outcome, not the activity>

**Mode:** challenge | execute | explore   <!-- omit = challenge -->

**Bar:** standard | exceptional   <!-- omit = standard. Exceptional triggers
 the draft → gap-analysis → revise ratchet and licenses ambitious choices.
 Use sparingly: it costs tokens and only pays on work with headroom.
 If you have an example of the quality you mean, attach or point to it —
 taste transfers through examples, not adjectives. -->

**Why / context:**
<What this is for, what triggered it, where it fits in the larger project.
 The single highest-leverage field in the template: it lets Claude make
 a hundred small decisions correctly without asking.>

**Current state:**
<What exists now — relevant files, branch, prior attempts, related tickets.
 Point at things by path/ID; don't paste what Claude can read itself.>

**Constraints & non-goals:**
<Hard requirements, things that must not change, and explicitly what is
 OUT of scope. Non-goals prevent the expensive kind of helpfulness.>

**Definition of done:**
<Concrete and checkable. "Tests X pass", "doc covers Y and Z",
 "I can run command C and see D." If you can't write this line,
 the task isn't ready to delegate.>

**Freedom:**
<What Claude may decide alone vs. what needs sign-off.
 e.g. "free: naming, file layout, library choice among ones already
 in the project; ask first: new dependencies, schema changes,
 anything touching the wallet code.">

**Known landmines:** <optional — fragile areas, deliberate weirdness,
 things that look wrong but aren't>

---

## Worked example

**Task:** Resolve TICKET-42 — design the export-retry mechanism for the reporting queue.

**Mode:** challenge

**Why / context:** Scheduled report exports silently drop when the
downstream storage API times out. This is the last open design decision
blocking the reliability milestone. At-least-once delivery is a hard
principle: a retry must never produce a duplicate report.

**Current state:** Design notes in `docs/decisions/TICKET-42.md`, current
queue logic in `src/exporter/`. TICKET-38 (storage API rate limits) was
resolved last week — see `docs/decisions/TICKET-38.md` for the constraints
it locks in.

**Constraints & non-goals:** At-least-once delivery, works within the
existing worker process (no new services), idempotent on retry. NOT in
scope: changing the report generation logic itself.

**Definition of done:** A decision doc in `docs/decisions/` with the
chosen mechanism, two rejected alternatives with reasons, and the expected
retry/backoff behavior at 1x, 10x, and 50x normal load.

**Freedom:** Free: doc structure, which alternatives to evaluate.
Ask first: anything that would reopen TICKET-38.
