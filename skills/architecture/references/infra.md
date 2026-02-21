# infra
# Layer: 3 Infrastructure
# Called by: any atomic skill with persistent output

## write-file

Write content to a file. Creates the file if it doesn't exist; overwrites if it does.

```
target: [file path relative to project root]
content: [full file content]
```

Always create parent directories if they don't exist.

## update-state

Update `.loom/state.md` after each task or stage transition.

Fields to update:
- `current_stage` — current stage name
- `stage_progress` — e.g. "3/5 tasks complete"
- `pause_reason` — empty if not paused
- `pending_decisions` — list of open questions for the user
- `last_interventions` — append latest user instruction with timestamp and affected files

State file format:

```markdown
# Project State

## Info
- project: [name]
- current_stage: [stage]
- stage_progress: [x/y tasks complete]

## Current Tasks
- [x] task one
- [ ] task two

## Pause Reason
[empty if none]

## Pending Decisions
- [question for user]

## Recent Interventions
- [YYYY-MM-DD] You said: [instruction]
  Modified: [file list]
```

## log-decision

Append an entry to `.loom/decisions.log` when a decision is made or a user intervenes.

Entry format:

```
[YYYY-MM-DD HH:MM] Stage: [stage]
Decision: [what was decided]
Reason: [user instruction or checkpoint resolution]
Affected files: [comma-separated list]
---
```

## update-context

Regenerate `.loom/context.md` at stage transitions. Extract the minimum useful context for the next stage:

| Next stage | What to include |
|------------|-----------------|
| design     | Full `.loom/ideation/requirements.md` + brief summary |
| architecture | `.loom/design/system.md` + key design decisions |
| plan       | `.loom/architecture/architecture.md` + module list + key stack decisions |
