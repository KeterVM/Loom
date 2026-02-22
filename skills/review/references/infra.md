# infra
# Layer: 3 Infrastructure
# Called by: review stage skills

## write-file

Write content to a file. Creates the file if it doesn't exist; overwrites if it does.

```
target: [file path relative to project root]
content: [full file content]
```

Always create parent directories if they don't exist.

## update-state

Append the review summary to `.loom/state.md`.

Fields to update:
- `current_stage` — keep as-is (review does not change the current stage)
- `pause_reason` — set if review finds a P1 issue that must be resolved before continuing
- `pending_decisions` — append P1 action items the user has not yet resolved

State file format:

```markdown
# Project State

## Info
- project: [name]
- current_stage: [unchanged]

## Pause Reason
[empty if none, or P1 issue from review]

## Pending Decisions
- [P1 action item from review]
```
