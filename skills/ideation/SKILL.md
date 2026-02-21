---
name: ideation
description: "loom ideation stage skill. Triggered when the user invokes /ideation. Drives the full workflow from a one-line idea to a complete requirements document. Execution chain: analyze-brief → gen-user-stories → analyze-competitors → analyze-requirements → gen-prd. Outputs (requirements.md, brief.md, research.md) are written to the project's .loom/ directory and .loom/state.md is updated."
---

# Ideation Stage

## Pre-checks

1. Check if `.loom/state.md` exists in the project root
   - Not found → create initial state.md (stage: ideation, all tasks pending)
   - Found → read current progress and resume from last checkpoint

2. Check if `.skills/stage/ideation.md` exists in the project
   - Found → use project-level file to override this orchestration logic

## Execution

Read `.loom/state.md` to find `ideation_step` (default: 1 if not set).

Load and execute ONLY the reference for the current step:

| Step | Reference |
|------|-----------|
| 1    | [analyze-brief](references/analyze-brief.md) |
| 2    | [gen-user-stories](references/gen-user-stories.md) |
| 3    | [analyze-competitors](references/analyze-competitors.md) |
| 4    | [analyze-requirements](references/analyze-requirements.md) |
| 5    | [gen-prd](references/gen-prd.md) |

After executing the step:
1. Update `ideation_step` in state.md (increment by 1)
2. Report what was done
3. Stop and wait — do not load the next reference until the user continues

## Checkpoint Rules

Pause and write to the "pending decisions" field in `.loom/state.md` when:

- User input is too vague to extract a target user or core problem
- Competitor analysis reveals a saturated market — confirm whether to proceed
- Requirements list contains conflicting features

## Completion

When all steps are done:

1. Use [infra](references/infra.md) to write outputs: `.loom/ideation/requirements.md`, `.loom/ideation/brief.md`, `.loom/ideation/research.md`
2. Use [infra](references/infra.md) to update `.loom/state.md`: stage = ideation ✅, next = design
3. Output a stage summary and wait for user to confirm "proceed to design"
