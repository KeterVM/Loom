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

Update `.loom/state.md` after each feature is completed.

Fields to update:
- `current_stage` — "build"
- `stage_progress` — e.g. "3/7 features done"
- `pause_reason` — empty if not paused
- `pending_decisions` — list of open questions for the user
- `last_interventions` — append latest user instruction with timestamp and affected files

State file format:

```markdown
# Project State

## Info
- project: [name]
- current_stage: build
- stage_progress: [N]/[total] features done

## Current Tasks
- [x] 01-foundation
- [x] 02-auth
- [ ] 03-user-profile
- [ ] ...

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
[YYYY-MM-DD HH:MM] Stage: build
Feature: [feature-slug]
Decision: [what was decided]
Reason: [user instruction or checkpoint resolution]
Affected files: [comma-separated list]
---
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
Also call `log-decision` with `Decision: Feature blocked` and the reason.

## read-decisions

Read `.loom/decisions.log` and return a condensed summary of past decisions relevant to the current stage.

```
stage: build
```

Filter entries where `Stage: build` or earlier stages. For each entry extract:
- Date, Stage, Feature (if present), Decision, Reason

Return as a compact bullet list. If the file does not exist or is empty, return nothing (do not error).

Use this at the start of `/build` to avoid re-asking questions already resolved in prior sessions.

## update-context

Regenerate `.loom/context.md` after each feature is done. Extract the minimum useful context for the next stage:

| Next stage   | What to include |
|--------------|-----------------|
| build        | Done features list + pending features list (include `blocked` ones with their `block_reason`) + last completed feature's Done block |
