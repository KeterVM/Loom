# exec-feature
# Layer: 2 Executor
# Output: persistent → source files + .loom/features/[name].md (status + Done block)

## Purpose

Execute a single feature from `.loom/features/`. Finds the next eligible feature, implements each task, verifies acceptance, and fills in the Done block.

## Input

- Required: `.loom/features/` directory
- Required: `.loom/architecture/architecture.md`
- Required: `.loom/design/system.md` (for UI tasks)
- Required: `.loom/design/screens/` (for screen-specific tasks)

## Execution

**1. Select the next feature**

Scan all `.loom/features/*.md` files:
- Read frontmatter: `status`, `depends-on`, `priority`
- Eligible = status is `pending` AND all features in `depends-on` have `status: done`
- If multiple eligible, pick the lowest `priority` number (lowest = highest priority)
- If no eligible feature found:
  - Check if any pending features have unsatisfied dependencies → report which features are blocked and why
  - Check if all features are done → report completion

**2. Mark feature as in-progress**

Update the selected feature file's frontmatter: `status: in-progress`.

Report:

```
Building: [NN]-[feature-name]
[What section content]

Tasks:
- [ ] Task 1 ...
- [ ] Task 2 ...
```

**3. Execute each task in order (TDD loop)**

Read the test command from the `Test` row in `.loom/architecture/architecture.md`.

For each task:

a. **Write test** — based on the task's Acceptance condition, write a test file (or add test cases to an existing file). Follow the project's test directory convention (infer from architecture.md or existing test files).

b. **Run RED** — execute the test command. Confirm the new test fails (expected — implementation doesn't exist yet). If it passes unexpectedly, investigate before proceeding.

c. **Implement** — write the source code that satisfies the Acceptance condition.

d. **Run GREEN** — execute the test command again. Confirm the test passes.

e. **Mark [x]** — update the task checkbox in the feature file: `[ ]` → `[x]`. Report the task as done with a one-line summary.

**Test runner checkpoint** — if the test command cannot be executed (missing environment, uninstalled dependencies, etc.), pause:

```
Test runner failed: [error]
Options:
1. Fix environment and retry
2. Skip test execution — verify by code inspection instead
3. Pause — I'll fix this manually
```

**4. After all tasks complete, fill in the Done block**

Replace the blank `## Done` section with:

```markdown
## Done
[date completed: YYYY-MM-DD]

### What was built
[2–4 sentences: what was implemented, key technical decisions made]

### Deviations from spec
[Any tasks that were modified from the original spec, and why. "None" if all tasks executed as written.]

### Remaining issues
[Any known bugs, TODOs, or deferred work. "None" if clean.]
```

**5. Mark feature as done**

Update frontmatter: `status: done`.

**6. Report completion**

```
Feature complete: [NN]-[feature-name]

Tasks done: [N]/[N]
Files modified:
  - [file list]

Deviations: [none | brief summary]
Remaining issues: [none | brief summary]

Next eligible feature: [NN]-[next-name] (or "all features done")

Run /build again to continue.
```

## Checkpoint

Pause mid-feature if:
- An acceptance criterion cannot be met
- Implementation reveals a task definition is incorrect or incomplete
- A required dependency (library, service, schema) is missing

```
Blocked on Task "[name]" in [feature]:

Problem: [what's wrong]
Options:
1. Skip this task and continue
2. Redefine the task: [suggested new definition]
3. Pause — I'll resolve this manually
```

## Output (persistent)

- Source files created or modified per task
- `.loom/features/[name].md` updated:
  - `status: done` in frontmatter
  - All task checkboxes marked `[x]`
  - Done block filled in
