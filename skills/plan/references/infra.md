# infra
# Layer: 3 Infrastructure
# Called by: plan stage skills

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
- `stage_progress` — e.g. "plan ✅"
- `pause_reason` — empty if not paused
- `pending_decisions` — list of open questions for the user
- `last_interventions` — append latest user instruction with timestamp and affected files

State file format:

```markdown
# Project State

## Info
- project: [name]
- current_stage: [stage]
- stage_progress: [x/y stages complete]

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
[YYYY-MM-DD HH:MM] Stage: plan
Decision: [what was decided]
Reason: [user instruction or checkpoint resolution]
Affected files: [comma-separated list]
---
```

## read-decisions

Read `.loom/decisions.log` and return entries relevant to specified stages.

```
stages: [comma-separated list, e.g. "architecture"]
```

For each matching entry, return:
- Date, Stage, Decision, Reason

Return as a compact bullet list. If the file does not exist or is empty, return nothing (do not error).

Use this when determining feature scope boundaries and dependency ordering.

## update-context

Regenerate `.loom/context.md` at stage transitions. Extract the minimum useful context for the next stage:

| Next stage   | What to include |
|--------------|-----------------|
| build        | Feature list from `.loom/features/` (titles + status) + architecture module summary + key stack decisions |
