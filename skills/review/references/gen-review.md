# gen-review
# Layer: 2 Executor
# Output: transient (summary presented to user; state.md and context.md updated via infra)

## Purpose

Analyse the current project state across all loom artefacts and produce a structured review report covering technical debt, architecture drift, deviations, and blocked features.

## Input

- Required: `.loom/features/*.md` — all feature files (status, Done blocks, deviations)
- Required: `.loom/architecture/architecture.md` — original architecture spec
- Required: `.loom/state.md` — current stage, blocked reasons, pending decisions
- Optional: `.loom/design/system.md` — original design system for UI drift checks

## Execution

### 1. Summarise build progress

Count features by status: done / in-progress / pending / blocked / skipped.

```
Build progress: [done]/[total] features complete
  Done       : [N]
  In-progress: [N]
  Pending    : [N]
  Blocked    : [N]
  Skipped    : [N]
```

### 2. Detect deviations from spec

For each feature with `status: done`, read its `## Done → Deviations from spec` section.
- Collect all non-"None" deviations
- Group by: scope change / tech decision / dropped requirement / added requirement
- Flag any deviation that may affect a pending feature's assumptions

```
Deviations detected: [N]
  [feature-slug]: [deviation summary]
  ...
```

If a deviation in a done feature contradicts the task definition of a pending feature, flag it explicitly:
```
⚠ Contradiction: [done-feature] changed [X], but [pending-feature] Task [Y] still assumes [old assumption]
```

### 3. Identify technical debt

From Done blocks' `## Remaining issues` sections, collect all non-"None" entries.
Categorise each:
- **Bug** — known defect
- **TODO** — deferred implementation
- **Design gap** — missing spec coverage

For each item, estimate impact: Low / Medium / High (based on whether pending features depend on the affected module).

```
Technical debt: [N] items
  [HIGH]   [feature-slug]: [issue summary]
  [MEDIUM] [feature-slug]: [issue summary]
  [LOW]    [feature-slug]: [issue summary]
```

### 4. Check architecture drift

Compare the modules and data models in `.loom/architecture/architecture.md` against what was actually built (inferred from Done blocks' "What was built" and file lists).

Flag any module that:
- Was specified but not yet started and has no pending feature
- Was added during build but not in the original architecture
- Has a significantly different interface than specified

```
Architecture drift: [N] items
  Missing from build plan : [module list]
  Added outside plan      : [module list]
  Interface drift         : [module]: [expected vs actual]
```

### 5. Blocked features summary

List all features with `status: blocked`, their `block_reason`, and how long they have been blocked (if date is available).

Identify if multiple blocked features share a root cause. If so, suggest a resolution strategy:
- Fix as a new micro-feature (use `/feature:spec`)
- Adjust architecture (re-run relevant part of `/architecture`)
- Skip and mark as out-of-scope

```
Blocked features: [N]
  [feature-slug]: [block_reason]
  ...
[Common root cause: ... → Suggested action: ...]
```

### 6. Produce action list

Prioritise and list recommended actions (HIGH first):

```
Recommended actions:
  [P1] Resolve contradiction: update [pending-feature] Task [Y] to match new assumption
  [P1] Unblock [feature-slug]: [suggested fix]
  [P2] Address technical debt: [HIGH item]
  [P3] Create new feature for undocumented module: [module]
  ...
```

### 7. Recommendation

Conclude with one of:
- **Continue building** — no critical issues; proceed with next `/build`
- **Address before continuing** — list the P1 items that must be resolved first
- **Run /architecture again** — architecture drift is significant; recommend re-scoping

## Output (transient)

Present the full review report to the user in the following structure:

```
╔══════════════════════════════════════════════════════╗
║  loom review — [YYYY-MM-DD]                          ║
╠══════════════════════════════════════════════════════╣
║  Build progress  [done]/[total] features             ║
╚══════════════════════════════════════════════════════╝

[Section 2: Deviations]
[Section 3: Technical debt]
[Section 4: Architecture drift]
[Section 5: Blocked features]
[Section 6: Action list]
[Section 7: Recommendation]
```

After presenting, ask:

```
How would you like to proceed?
1. Continue /build — start next eligible feature
2. Create a new feature to address a specific item
3. Update a pending feature's tasks based on deviations found
4. I'll handle the issues manually before continuing
```
