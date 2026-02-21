---
name: plan
description: "loom plan stage skill. Triggered when the user invokes /plan. Reads architecture.md and generates N individual feature files under .loom/features/. User reviews and confirms the feature breakdown before building begins."
---

# Plan Stage

## Pre-checks

1. Check if `.loom/state.md` exists in the project root
   - Not found → stop and tell the user to run `/ideation` first
   - Found → read current progress and resume from last checkpoint

2. Check that `.loom/architecture/architecture.md` exists
   - Not found → stop and tell the user the architecture stage must be completed first

3. Check that `.loom/state.md` shows `architecture ✅`
   - Not shown → stop and tell the user to complete and confirm `/architecture` first

4. Check if `.skills/stage/plan.md` exists in the project
   - Found → use project-level file to override this orchestration logic

## Execution

Read `.loom/state.md` to find `plan_step` (default: 1 if not set).

| Step | Reference | Pauses? |
|------|-----------|---------|
| 1    | [gen-features](references/gen-features.md) | ✅ wait for feature confirmation |

After step 1 (gen-features):
- Present the generated feature files list to the user
- **Wait for explicit confirmation** before proceeding
- User may reorder, merge, or split features before confirming

## Checkpoint Rules

Pause and write to the "pending decisions" field in `.loom/state.md` when:

- A feature has unclear scope (cannot determine What or Changes) — confirm definition
- Two features have ambiguous dependency ordering — confirm ordering

## Completion

When all steps are done and user has confirmed the feature breakdown:

1. Use [infra](references/infra.md) to update `.loom/state.md`: stage = plan ✅, next = build
2. Use [infra](references/infra.md) to update `.loom/context.md` for the build stage
3. Output a stage summary: N features generated, dependency graph, suggested build order
4. **Wait for explicit user confirmation before proceeding to `/build`**
