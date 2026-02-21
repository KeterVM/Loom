# gen-features
# Layer: 2 Executor
# Output: persistent → .loom/features/[NN]-[name].md (one file per feature)

## Purpose

Break the architecture document into independently deliverable feature slices. Each feature becomes a standalone `.md` file with spec, tasks, and a blank Done section. Files are numbered to suggest build order; users may reorder before confirming.

## Input

- Required: `.loom/architecture/architecture.md`
- Optional: `.loom/ideation/requirements.md` (for priority context)
- Optional: `.loom/design/system.md` (for UI feature scope)

## Execution

**1. Identify features** — read `architecture.md` and extract logical feature slices:

Rules:
- Each feature must be independently deliverable (runnable / testable in isolation)
- A feature may span multiple modules but must have a single clear "what"
- Prefer smaller features (3–7 tasks) over large ones; split if larger
- The first feature should always be `foundation` (project scaffolding, DB schema, CI setup, etc.)
- Features should be ordered so each one builds on the previous

**2. Identify dependencies** — for each feature, determine which other features it requires to be done first. Express as `depends-on: [feature-slug]`.

**3. Build dependency graph** (text summary for user review):

```
01-foundation
  └─ 02-auth
       ├─ 03-user-profile
       └─ 04-core-api
            └─ 05-dashboard
```

**4. Write one file per feature** using the format below.

## Feature File Format

```markdown
---
status: pending
depends-on: []
priority: [N]
---

# [Feature Name]

## What
[One paragraph: what this feature delivers to the user or system. Be specific about scope boundaries — what is included and what is not.]

## Changes
[Delta only — which modules, API routes, DB tables, or components will be created or modified by this feature. Reference architecture.md module names.]

## Tasks
- [ ] Task 1: [imperative description]
  - Acceptance: [specific, observable condition that proves it's done]
- [ ] Task 2: [imperative description]
  - Acceptance: [specific, observable condition that proves it's done]

## Done
[Leave blank — /build fills this in upon completion]
```

Rules for tasks:
- Each task must have exactly one Acceptance condition (concrete and verifiable)
- Tasks must be in implementation order within the feature
- No task should span more than 2 unrelated modules
- L-complexity work should be split into multiple tasks

## Checkpoint

If a feature's scope is ambiguous (cannot write a clear "What" in one paragraph), pause:

```
Feature "[name]" has unclear scope.

Draft What: [attempt]

Options:
1. Split into: [feature A] + [feature B]
2. Narrow scope to: [reduced description]
3. Keep as-is with this What: [draft]
```

## Output (persistent)

Write `.loom/features/[NN]-[slug].md` for each feature:
- `NN` is a zero-padded sequence number (01, 02, …)
- `slug` is the feature name lowercased, spaces replaced with hyphens

After all files are written, output a summary:

```
Generated [N] features in .loom/features/:

  01-foundation.md      — [one-line What]
  02-auth.md            — [one-line What]
  ...

Dependency graph:
[text graph]

Review the feature files and confirm to begin /build.
```
