# validate-requirements
# Layer: 2 Executor
# Output: transient

## Purpose

Check `.loom/ideation/requirements.md` for completeness and internal consistency before generating any design artifacts.

## Input

- Required: `.loom/ideation/requirements.md`

## Execution

Run the following checks in order:

**1. Coverage check** — every user story must map to at least one feature:
- List all user stories from the requirements doc
- For each story, confirm a corresponding feature exists
- Flag any story with no matching feature as: `MISSING FEATURE: [story]`

**2. Conflict check** — look for contradictions:
- Features that exclude each other (e.g. "real-time sync" + "offline-first with no conflict resolution")
- Priority conflicts (two must-have features that cannot coexist technically)
- Flag each conflict as: `CONFLICT: [feature A] ↔ [feature B] — [reason]`

**3. Completeness check** — flag missing standard sections:
- Target user defined?
- Core problem stated?
- At least one must-have feature?
- At least one non-goal or out-of-scope item?

## Checkpoint

If any `MISSING FEATURE` or `CONFLICT` items are found, pause and present them to the user:

```
Requirements issues found before design can begin:

Missing features:
- [list]

Conflicts:
- [list]

Please resolve these before continuing. You can edit .loom/ideation/requirements.md and then run /design again.
```

Do not continue to gen-design-tokens until the user confirms issues are resolved.

## Output (transient)

```
validation:
  status: pass | fail
  missing_features: [list or empty]
  conflicts: [list or empty]
  summary: "Requirements complete — ready for design" | "[N] issues found"
```
