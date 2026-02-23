---
name: design
description: "loom design stage skill. Triggered when the user invokes /design. Drives the full workflow from a validated requirements document to a complete design system spec. Execution chain: validate-requirements → gen-design-tokens → gen-user-flows → gen-screen-specs. Outputs are written to the project's .loom/ directory and .loom/state.md is updated."
---

# Design Stage

## Pre-checks

1. Check if `.loom/state.md` exists in the project root
   - Not found → stop and tell the user to run `/ideation` first
   - Found → read current progress and resume from last checkpoint

2. Check that `.loom/ideation/requirements.md` exists
   - Not found → stop and tell the user the ideation stage must be completed first

3. Check if `.skills/stage/design.md` exists in the project
   - Found → use project-level file to override this orchestration logic

## Execution

Read `.loom/state.md` to find `design_step` (default: 1 if not set).

Load and execute ONLY the reference for the current step:

| Step | Reference |
|------|-----------|
| 1    | [validate-requirements](references/validate-requirements.md) |
| 2    | [gen-design-tokens](references/gen-design-tokens.md) |
| 3    | [gen-user-flows](references/gen-user-flows.md) |
| 4    | [gen-screen-specs](references/gen-screen-specs.md) |

After executing the step:
1. Update `design_step` in state.md (increment by 1)
2. Report what was done
3. Stop and wait — do not load the next reference until the user continues

## Checkpoint Rules

Pause and write to the "pending decisions" field in `.loom/state.md` when:

- Requirements validation fails — list problems and wait for user to resolve before continuing
- Design tokens require a style direction decision (e.g. minimal vs expressive, light vs dark)
- A user flow has ambiguous branching — confirm intended behavior before generating screen specs

## Completion

When all steps are done:

1. Use [infra](references/infra.md) to update `.loom/state.md`: stage = design ✅, next = architecture
2. Output a stage summary and wait for user to confirm "proceed to /architecture"
