---
name: review
description: "loom review stage skill. Triggered when the user invokes /review. Evaluates the current build state: identifies technical debt, architecture drift, feature deviations, and blocked features. Produces a prioritised action list and recommends whether to continue building or address issues first."
---

# Review Stage

## Purpose

Review is an optional, repeatable stage that can be run any time during the build loop — especially useful after every 3–5 features or when the user senses drift from the original design. It does not block other stages.

## Pre-checks

1. Check if `.loom/state.md` exists in the project root
   - Not found → stop and tell the user to run `/ideation` first

2. Check that `.loom/features/` directory exists and contains at least one `.md` file
   - Not found → stop and tell the user to run `/plan` first

3. Check that `.loom/state.md` shows `plan ✅`
   - Not shown → stop and tell the user to complete `/plan` first

4. Check if `.skills/stage/review.md` exists in the project
   - Found → use project-level file to override this orchestration logic

## Execution

Load and execute [gen-review](references/gen-review.md).

## Checkpoint Rules

Pause and write to the "pending decisions" field in `.loom/state.md` when:

- Review finds a critical architecture drift that invalidates pending features
- A blocking technical debt item must be resolved before the next feature can start
- Multiple blocked features share a common root cause that warrants a new feature or task

## Completion

When gen-review is done:

1. Use [infra](references/infra.md) to update `.loom/state.md`: append review summary to `Recent Interventions`
2. Use [infra](references/infra.md) to `log-decision` for each recommended action the user confirms
3. Use [infra](references/infra.md) to update `.loom/context.md` with the review summary
4. **Do not change any feature statuses** — the user decides what to act on
5. Output the review report and wait for the user to decide next step (continue `/build`, fix debt, or run `/feature:spec`)
