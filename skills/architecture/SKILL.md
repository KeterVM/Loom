---
name: architecture
description: "loom architecture stage skill. Triggered when the user invokes /architecture. Drives the full workflow from a completed design system to a confirmed technical architecture. Execution chain: analyze-risks → gen-architecture. Output is .loom/architecture/architecture.md. User must confirm stack and architecture before proceeding to /dev."
---

# Architecture Stage

## Pre-checks

1. Check if `.loom/state.md` exists in the project root
   - Not found → stop and tell the user to run `/ideation` first
   - Found → read current progress and resume from last checkpoint

2. Check that `.loom/design/system.md` exists
   - Not found → stop and tell the user the design stage must be completed first

3. Use [infra](references/infra.md) `read-decisions` (filter: `ideation`, `design`) to load a summary of prior decisions — surface these before risk analysis so no already-resolved trade-off is re-opened

4. Check if `.skills/stage/architecture.md` exists in the project
   - Found → use project-level file to override this orchestration logic

## Execution

Read `.loom/state.md` to find `architecture_step` (default: 1 if not set).

Load and execute ONLY the reference for the current step:

| Step | Reference |
|------|-----------|
| 1    | [analyze-risks](references/analyze-risks.md) |
| 2    | [gen-architecture](references/gen-architecture.md) |

After executing the step:
1. Update `architecture_step` in state.md (increment by 1)
2. Report what was done
3. Stop and wait — do not load the next reference until the user continues

## Checkpoint Rules

Pause and write to the "pending decisions" field in `.loom/state.md` when:

- Risk analysis surfaces a high-severity risk — confirm mitigation strategy before proceeding
- Architecture requires a stack decision (e.g. which database, which framework) — confirm choice
## Completion

When all steps are done:

1. Use [infra](references/infra.md) to update `.loom/state.md`: stage = architecture ✅, next = plan
2. Use [infra](references/infra.md) to update `.loom/context.md` for the plan stage
3. Output a stage summary: stack decisions, module count, key risks mitigated
4. **Wait for explicit user confirmation before proceeding to `/plan`**
