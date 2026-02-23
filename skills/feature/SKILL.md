---
name: feature
description: "loom feature skill. Triggered when the user invokes /feature:spec <name>. Creates a new feature spec file in .loom/features/ for a post-initial feature. User fills in or confirms the spec, then runs /build to execute it."
---

# Feature Spec

## Purpose

Add a new feature to an existing project that has already gone through `/plan` and `/build`. Creates a properly formatted feature file that `/build` can pick up and execute.

## Pre-checks

1. Check if `.loom/features/` directory exists
   - Not found → stop and tell the user to run `/plan` first

2. Check if `.loom/state.md` shows `plan ✅`
   - Not shown → stop and tell the user to complete `/plan` first

## Input

The user invokes `/feature:spec <name>` where `<name>` is the feature name (e.g. "dark-mode", "export-csv").

## Execution

**Step 1: Determine file name**

- Count existing feature files to determine next sequence number (`NN`)
- Slugify the name: lowercase, spaces → hyphens
- File path: `.loom/features/[NN]-[slug].md`

**Step 2: Ask for spec details** (if not provided in the invocation)

Ask the user:
1. **What does this feature deliver?** (one paragraph — the "What" section)
2. **Which existing features does it depend on?** (list feature slugs, or "none")
3. **What modules/files will change?** (the "Changes" section)

If the user has provided enough context in their message, draft the spec and ask for confirmation instead of asking each question separately.

**Step 3: Generate task breakdown**

Based on the What and Changes, draft 3–7 tasks with acceptance conditions. Present them to the user:

```
Draft tasks for [feature name]:

- [ ] Task 1: [description]
  - Acceptance: [condition]
- [ ] Task 2: [description]
  - Acceptance: [condition]
...

Confirm or edit these tasks before I create the file.
```

**Step 4: Write the feature file**

After user confirmation, write `.loom/features/[NN]-[slug].md`:

```markdown
---
status: pending
depends-on: [list or empty]
priority: [NN]
---

# [Feature Name]

## What
[user-provided or drafted description]

## Changes
[user-provided or drafted module list]

## Tasks
- [ ] Task 1: [description]
  - Acceptance: [condition]
- [ ] Task 2: [description]
  - Acceptance: [condition]

## Done
[Leave blank — /build fills this in upon completion]
```

**Step 5: Confirm**

```
Created: .loom/features/[NN]-[slug].md

Run /build to execute this feature.
```

## No infra calls needed

This skill writes the feature file directly (inline above). No update-state needed — `/build` will handle that when it executes the feature.
