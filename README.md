# Claude Working-System Bundle

Everything from the 2026-07-02 session with Fable 5: three Claude Code skills,
a collaboration contract, a per-task brief template, and the upgraded project
task-tracking templates. Written to be model-agnostic — built for Opus 4.8,
valid for whatever comes after.

## What goes where

### 1. `skills/` → `~/.claude/skills/`

Copy the three folders as-is (folder + SKILL.md inside):

```
cp -r skills/task-planning skills/code-quality-review skills/session-handover ~/.claude/skills/
```

Claude Code picks them up automatically. To scope a skill to a single project
instead, place it in `<project>/.claude/skills/`.

- `task-planning` — plan/decompose before executing; front-load risk; scope control
- `code-quality-review` — producing code that survives review + reviewing code
- `session-handover` — writing AND resuming handovers; defers to your
  SESSION_HANDOVER.md / CLAUDE.md conventions

### 2. `prompting/collaboration-contract.md` → standing context

Two destinations, same content:

- **Claude Code**: paste into `~/.claude/CLAUDE.md` (global, all projects) or a
  specific project's `CLAUDE.md` (that project only). If you already have a
  CLAUDE.md, append it as a section.
- **Claude.ai**: paste into a Project's custom instructions.

Defines the three-stage shape (Align → Execute → Close out), challenge-first
default, `Mode:` and `Bar:` dials, honesty norms, and subagent policy.

### 3. `prompting/task-brief-template.md` → wherever you draft messages

Not installed anywhere — it's a fill-in skeleton you paste as the opening
message of ad-hoc tasks. Keep it somewhere handy (notes app, snippets tool,
or the project repo root). For work tracked in the task system below, the
task file replaces this brief entirely.

### 4. `project-templates/` → root of each new project repo

```
cp project-templates/TASKS.md project-templates/TASK_TEMPLATE.md <project-root>/
mkdir <project-root>/TASKS
```

Then fill in the project overview placeholders in TASKS.md and delete the
example rows. Upgrades in this version:

- TASK_TEMPLATE.md: added **Out of Scope**, **Freedom** (agent decision
  authority), and **Failed Approaches** (never delete entries) sections;
  usage guide updated with the never-prune rule.
- TASKS.md: added **Source of Truth** rule — task file is authority, tables
  are index, both updated in the same commit.

## How the pieces fit

- **Skills** = the discipline (how Claude plans, reviews, hands over)
- **Collaboration contract** = the relationship (staged collaboration,
  challenge-first, honesty, agents)
- **Task brief / task files** = the work itself (goal, done, boundaries,
  freedom, dead ends)

A task file + the contract + the skills is a complete delegation package:
an agent can pick up `TASKS/TASK-XXX.md` and work it safely without
supervision.

## Maintenance notes

- After a week or two on Opus, prune anything from the skills that the model
  already does reliably unprompted — a skill restating default behaviour is
  context tax.
- `Bar: exceptional` is a dial you spend, not a default. If everything is
  exceptional, nothing is.
- If you find yourself only filling in "Task" and "Done" in the brief
  template, trim the template to what you actually use rather than
  abandoning it.
