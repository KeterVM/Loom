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

## Step 4: Write loom routing into the target project's agent instruction file

Determine which agent instruction file the target project uses:

1. Check for `CLAUDE.md` — used by Claude Code
2. Check for `AGENTS.md` — used by Codex and other agents
3. Check for both, or neither

Decision rules:
- If one exists: check whether it already contains a `# loom` section
  - If the section exists: **replace it entirely** with the content below
  - If the section does not exist: append the content below
- If both exist, apply the above rule to both files
- If neither exists, create `CLAUDE.md` with the content below

**Do NOT skip this step because a loom section already exists. Always update it to ensure it reflects the current loom library path.**

Content to write:

Content to write:

```markdown
# loom

This project uses loom. Skill definitions live in a separate library directory.

## How to run a loom stage

When the user invokes a loom command (`/ideation`, `/design`, `/architecture`, `/plan`, `/build`, `/feature:spec`):

1. Read `.loom/loom-path` to get the absolute path to the loom library
2. Load `[loom-library-path]/skills/[stage]/SKILL.md`
3. Follow the instructions in that file exactly

Command-to-file mapping:
- `/ideation`      → `skills/ideation/SKILL.md`
- `/design`        → `skills/design/SKILL.md`
- `/architecture`  → `skills/architecture/SKILL.md`
- `/plan`          → `skills/plan/SKILL.md`
- `/build`         → `skills/build/SKILL.md`
- `/feature:spec`  → `skills/feature/SKILL.md`

## State

Stage progress is tracked in `.loom/state.md`. All stage skills read and write this file automatically. Do not edit it manually.

## Overrides

To override a stage skill for this project only, create `.skills/stage/[stage-name].md`. The stage skill will load your local file instead of the library version.
```

---

## Step 5: Confirm

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

── Step 4: agent instruction file ─────────────────
  [✓/✗]  CLAUDE.md    [appended / updated / created / FAILED: reason]
  [✓/✗]  AGENTS.md    [appended / updated / not present — skipped]

═══════════════════════════════════════════════════
Status: [ALL OK / X FAILURE(S) — see above]

Next step: open [target project path] in your agent and run /ideation
```

If any row shows `✗`, do not silently continue — surface the failure clearly so the user can fix it before proceeding.
