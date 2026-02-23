# Install loom

Read this file and execute the steps below to install loom into a target project.

**CRITICAL: Execute every step in order. Do NOT skip any step, even if the target file or directory already exists. All files must be written (or overwritten) on every install.**

---

## What you are doing

loom is a workflow system for product development. It lives in a separate library directory. Installing it means:
1. Writing a few state files into the target project's `.loom/` directory
2. Writing a `CLAUDE.md` routing table into the target project so future Claude Code sessions know how to run loom stages

No files are copied from the loom library. The target project references the library by absolute path.

---

## Step 1: Gather inputs

Ask the user for:

1. **Target project path** — absolute path to the project to install loom into
2. **loom library path** — absolute path to the directory containing this file
   - If the user does not know it, use the directory where this INSTALL.md lives
3. **Agent in use** — determine which agent is running this install:

   | Agent       | Project skills dir        |
   | ----------- | ------------------------- |
   | Claude Code | `.claude/skills`          |
   | Codex       | `.codex/skills`           |
   | Other       | ask user for the dir name |

   If you can identify yourself from the table above, use the corresponding skills dir. Otherwise ask the user which agent they are using.

Validate before proceeding:
- Target project directory must exist
- loom library directory must contain a `skills/` subdirectory

If either check fails, stop and report the problem. Do not proceed.

---

## Step 2: Create `.loom/` structure in the target project

Write the following three files. Create the `.loom/` directory if it does not exist. **Always write all three files, even if they already exist — overwrite them.**

### `.loom/loom-path`

Content: the absolute path to the loom library, with no trailing newline.

```
[absolute path to loom library]
```

### `.loom/state.md`

```markdown
# Project State

## Info
- project: [target project directory name]
- current_stage: not started
- stage_progress: 0/6 stages complete

## Current Tasks
(none)

## Pause Reason
(none)

## Pending Decisions
(none)

## Recent Interventions
- [today's date] loom installed
```

### `.loom/context.md`

```markdown
# Project Context

(populated at first stage transition)
```

---

## Step 3: Symlink loom skills into the target project

Skills are symlinked into the target project root, scoped to this project only.

Determine `SKILLS_DIR` based on which agent is running this install:

| Agent        | Project skills dir (relative to target project) |
| ------------ | ----------------------------------------------- |
| Claude Code  | `.claude/skills`                                |
| Codex        | `.codex/skills`                                 |
| Other        | ask user for the dir name                       |

### Create symlinks

Run:

```
mkdir -p [SKILLS_DIR]
ln -sf [loom-library-path]/skills/ideation      [SKILLS_DIR]/ideation
ln -sf [loom-library-path]/skills/design        [SKILLS_DIR]/design
ln -sf [loom-library-path]/skills/architecture  [SKILLS_DIR]/architecture
ln -sf [loom-library-path]/skills/plan          [SKILLS_DIR]/plan
ln -sf [loom-library-path]/skills/build         [SKILLS_DIR]/build
ln -sf [loom-library-path]/skills/feature       [SKILLS_DIR]/feature
```

Use `-sf` so the symlink is replaced if it already exists (e.g. from a previous install or path change).

If any `ln` command fails, stop and report the error. Do not proceed.

---

## Step 4: Confirm

Report to the user using the following template. Fill in each field exactly — do not omit any row. Use `✓` for success, `✗` for failure, and include a short note if anything failed or was skipped.

```
loom install report
═══════════════════════════════════════════════════

Target project : [absolute path]
loom library   : [absolute path]

── Step 2: .loom/ files ────────────────────────────
  [✓/✗]  .loom/loom-path       [created / overwritten / FAILED: reason]
  [✓/✗]  .loom/state.md        [created / overwritten / FAILED: reason]
  [✓/✗]  .loom/context.md      [created / overwritten / FAILED: reason]

── Step 3: skill symlinks ──────────────────────────
  Skills dir(s): [path(s) detected]

  [✓/✗]  ideation        → [SKILLS_DIR]/ideation
  [✓/✗]  design          → [SKILLS_DIR]/design
  [✓/✗]  architecture    → [SKILLS_DIR]/architecture
  [✓/✗]  plan            → [SKILLS_DIR]/plan
  [✓/✗]  build           → [SKILLS_DIR]/build
  [✓/✗]  feature         → [SKILLS_DIR]/feature

═══════════════════════════════════════════════════
Status: [ALL OK / X FAILURE(S) — see above]

Next step: open [target project path] in your agent and run /ideation
```

If any row shows `✗`, do not silently continue — surface the failure clearly so the user can fix it before proceeding.
