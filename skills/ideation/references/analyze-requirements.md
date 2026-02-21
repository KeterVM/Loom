# analyze-requirements
# Layer: 2 Executor
# Output: transient

## Purpose

Evaluate the requirements list for feasibility, completeness, and conflicts. Assign final priorities.

## Input

transient: user_stories (from gen-user-stories), market_gap (from analyze-competitors)

## Execution

Review the user story list and for each item assess:

1. **Feasibility** — is this buildable by a solo developer in a reasonable timeframe?
2. **Coverage** — does the story map to the core problem? Flag any story that doesn't connect to `core_problem`
3. **Conflicts** — does this story contradict another? (e.g. "offline-first" vs "real-time sync")
4. **Missing** — is there a critical path not covered by any story?

Output a refined requirements list with:
- Confirmed P0/P1/P2 priorities
- Conflict notes inline
- A "missing items" section for gaps

## Checkpoint

Pause if conflicts are found between P0 stories:

```
I found conflicting requirements that both have P0 priority:
- "[story A]"
- "[story B]"

These can't both be true at the same time. How do you want to resolve this?
1. Keep A, drop B
2. Keep B, drop A
3. Redefine scope — explain what you meant
```

## Output (transient)

```
requirements:
  - story: "..."
    priority: P0
    conflict_with: null | "story ref"
    feasibility: ok | risky
  ...
missing_items:
  - "..."
```
