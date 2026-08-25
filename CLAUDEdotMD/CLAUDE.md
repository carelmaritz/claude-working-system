# Global Claude Code Instructions

## Subagents (Claude Code)

- **Proactively spawn subagents** when work is parallelisable or would pollute the main context. Good candidates: exploring multiple areas of a codebase at once, researching competing options, running a long test suite while continuing other work, bulk mechanical edits.
- Keep the main thread as the *orchestrator*: it holds the plan and integrates results; agents do the context-heavy legwork and report back conclusions, not raw dumps.
- Don't spawn agents for sequential, dependent steps — coordination overhead exceeds the win.
- Before launching more than two agents at once, state the fan-out plan in one or two lines so I can veto it.
- Match the model to the task: use cheap/fast tiers (haiku, sonnet) for mechanical or exploratory legwork — bulk edits, log scanning, file searches, run-babysitting — and reserve full-strength models for work where reasoning depth pays: subtle debugging, design judgment, adversarial review. Same logic for the reasoning-effort setting. When unsure, prefer quality over cost.
