# Claude Working-System Bundle

A set of Claude Code skills, a global CLAUDE.md, and project task-tracking
templates. Written to be model-agnostic — built for Opus 4.8, valid for
whatever comes after.

## What's in here

```
CLAUDEdotMD/CLAUDE.md      → global Claude Code instructions
skills/                    → three Claude Code skills
project-templates/         → task-tracking templates for a project repo
```

### 1. `CLAUDEdotMD/CLAUDE.md` → `~/.claude/CLAUDE.md`

Global Claude Code instructions — currently just the subagent policy
(when to spawn agents, keep the main thread as orchestrator, model
routing between cheap and full-strength tiers). Copy or merge into
`~/.claude/CLAUDE.md` for instructions that apply to every project, or
into a specific project's `CLAUDE.md` to scope it there.

### 2. `skills/` → `~/.claude/skills/`

Copy the three folders as-is (folder + SKILL.md inside):

```
cp -r skills/task-planning skills/code-quality-review skills/session-handover ~/.claude/skills/
```

Claude Code picks them up automatically. To scope a skill to a single project
instead, place it in `<project>/.claude/skills/`.

- `task-planning` — classify before planning, restate goal + definition of
  done, investigate before decomposing, front-load risk, declare scope
  boundaries, know when to re-plan
- `code-quality-review` — producing code that survives review (read the
  neighborhood, keep diffs minimal, self-review pass before presenting) and
  reviewing code (understand intent, big issues first, explicit verdict)
- `session-handover` — writing AND resuming handovers: required sections
  (current state, completed, next steps, open decisions, failed approaches,
  landmines), reading discipline on resume

### 3. `project-templates/` → root of each new project repo

```
cp project-templates/TASKS.md project-templates/TASK_TEMPLATE.md <project-root>/
mkdir <project-root>/TASKS
```

Then fill in the project overview placeholders in TASKS.md and delete the
example rows.

- `TASKS.md` — project-level task index: ID format, priority/effort/status
  definitions, branch naming, and the **Source of Truth** rule (task file is
  authority, the index table is a convenience view — both updated in the
  same commit)
- `TASK_TEMPLATE.md` — per-task file with Objective, Technical Requirements,
  **Out of Scope**, **Freedom** (what the assigned agent may decide alone vs.
  what needs sign-off), Implementation Plan, **Failed Approaches** (never
  delete entries), and a usage guide by task size

## How the pieces fit

- **CLAUDE.md** = standing instructions Claude Code loads every session
- **Skills** = the discipline (how Claude plans, reviews, hands over) — loaded
  on demand when the task matches
- **Task files** = the work itself (goal, done, boundaries, freedom, dead
  ends), tracked per project

A task file plus the skills is a complete delegation package: an agent can
pick up `TASKS/TASK-XXX.md` and work it safely without supervision.

## Maintenance notes

- After a stretch on a given model, prune anything from the skills that it
  already does reliably unprompted — a skill restating default behaviour is
  context tax.
- If a task file only ever gets "Objective" and "Success Criteria" filled in
  for your work, that's fine — the template's later sections are marked N/A
  or removed per the usage guide, not mandatory ceremony.
