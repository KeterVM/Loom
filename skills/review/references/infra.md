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

Append the review summary to `.loom/state.md` under `Recent Interventions`.

Fields to update:
- `last_interventions` — append entry: `[YYYY-MM-DD] /review completed: [one-line summary]`
- `pending_decisions` — append any P1 action items the user has not yet resolved

State file format: same as other stages (see plan/references/infra.md).

## log-decision

Append an entry to `.loom/decisions.log` for each recommended action the user confirms acting on.

Entry format:

```
[YYYY-MM-DD HH:MM] Stage: review
Decision: [action chosen]
Reason: [which review finding prompted it]
Affected files: [comma-separated list, or "TBD"]
---
```

## read-decisions

Read `.loom/decisions.log` and return all entries. Filter by stage if needed.

```
stage: [optional — filter to specific stage, e.g. "build"]
```

Return entries as a compact bullet list:
- `[date] [stage] [decision]`

If the file does not exist or is empty, return nothing.

## update-context

Append the review summary to `.loom/context.md` so subsequent `/build` invocations are aware of known issues.

| Content to add |
|----------------|
| Latest review date + build progress snapshot |
| P1 action items not yet resolved |
| Any contradictions that affect pending features |
