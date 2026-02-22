---
name: build
description: "loom build stage skill. Triggered when the user invokes /build. Finds the next pending feature whose dependencies are satisfied, executes it task by task, fills in the Done block, and marks it done. One feature per invocation."
---

# Build Stage

## Pre-checks

1. Check if `.loom/state.md` exists in the project root
   - Not found → stop and tell the user to run `/ideation` first
   - Found → read current progress and resume from last checkpoint

2. Check that `.loom/features/` directory exists and contains at least one `.md` file
   - Not found → stop and tell the user to run `/plan` first

3. Check that `.loom/state.md` shows `plan ✅`
   - Not shown → stop and tell the user to complete and confirm `/plan` first

4. Check if any features have `status: blocked`
   - If yes, surface them before proceeding: list each blocked feature and its `block_reason`
   - The user may resolve them or choose to skip them in exec-feature

5. Use [infra](references/infra.md) `read-decisions` to load a summary of past decisions — use this to avoid re-asking resolved questions during this session

6. Check if `.skills/stage/build.md` exists in the project
   - Found → use project-level file to override this orchestration logic

## Execution

Load and execute [exec-feature](references/exec-feature.md).

## Checkpoint Rules

Pause and write to the "pending decisions" field in `.loom/state.md` when:

- A task's acceptance criterion cannot be met — confirm how to proceed
- Implementation reveals a task definition is incorrect or incomplete
- A dependency on an external service or environment is missing

```
Blocked on Task "[name]" in [feature]:

Problem: [what's wrong]
Options:
1. Skip this task and continue
2. Redefine the task: [suggested new definition]
3. Block this feature — I'll resolve this manually
```

When option 3 is chosen: use [infra](references/infra.md) `mark-blocked` to set `status: blocked` on the feature with the reason, then stop. The next `/build` invocation will surface blocked features first.

## Completion

When the current feature is done:

1. Use [infra](references/infra.md) to update the feature file: status = done, fill Done block
2. Use [infra](references/infra.md) to update `.loom/state.md` with feature progress
3. Use [infra](references/infra.md) to update `.loom/context.md`
4. Output a feature summary (see exec-feature)
5. **Stop and wait** — do not start the next feature automatically

If all features are done:
1. Update `.loom/state.md`: stage = build ✅
2. Tell the user all features are complete
