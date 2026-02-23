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
- `ideation_step` — current step number within this stage (increment after each step)
- `pause_reason` — empty if not paused; fill in when stopping for user input
- `pending_decisions` — list of open questions for the user; clear when resolved

State file format:

```markdown
# Project State

## Info
- project: [name]
- current_stage: ideation
- ideation_step: [N]

## Pause Reason
[empty if none]

## Pending Decisions
- [question for user]
```
