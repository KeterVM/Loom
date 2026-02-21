# gen-user-stories
# Layer: 2 Executor
# Output: transient

## Purpose

Generate a user story list from the brief, covering the core usage paths.

## Input

transient: brief (from analyze-brief)

## Execution

Generate 5–10 user stories in this format:

```
As a [target user], I want to [action], so that [outcome].
Priority: P0 | P1 | P2
```

Priority rules:
- **P0**: Product is unusable without this
- **P1**: Significantly better with it, but barely usable without
- **P2**: Nice to have — can be cut from MVP

## Checkpoint

Pause and ask the user to narrow the scope if `target_user` is too broad (e.g. "everyone"), making it impossible to write focused stories.

## Output (transient)

```
user_stories:
  - story: "As a ..., I want to ..., so that ..."
    priority: P0
  - story: "..."
    priority: P1
  ...
```
