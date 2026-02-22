# infra
# Layer: 3 Infrastructure
# Called by: build stage skills

## write-file

Write content to a file. Creates the file if it doesn't exist; overwrites if it does.

```
target: [file path relative to project root]
content: [full file content]
```

Always create parent directories if they don't exist.

## update-state

Update `.loom/state.md` after each feature completes or when pausing.

Fields to update:
- `current_stage` — "build" or "build ✅" when all features are done
- `pause_reason` — empty if not paused; fill in when stopping for user input
- `pending_decisions` — list of open questions for the user; clear when resolved

State file format:

```markdown
# Project State

## Info
- project: [name]
- current_stage: build

## Pause Reason
[empty if none]

## Pending Decisions
- [question for user]
```

## mark-blocked

Mark a feature as blocked when the user chooses to stop and resolve manually.

```
feature: [feature file path, e.g. .loom/features/03-auth.md]
reason: [one-line description of what is blocking]
task: [task name that is blocked, optional]
```

Update the feature file's frontmatter:
- `status: blocked`
- `block_reason: [reason]`
- `blocked_task: [task name]` (if provided)

Also call `update-state` to set `pause_reason` to the same reason.
