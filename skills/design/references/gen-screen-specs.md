# gen-screen-specs
# Layer: 2 Executor
# Output: persistent → .loom/design/screens/

## Purpose

Generate a detailed layout and interaction specification for each page identified in the user flows.

## Input

- Required: `.loom/design/flows.md` (page inventory + flows)
- Required: `.loom/design/system.md` (design tokens)
- Required: `.loom/ideation/requirements.md` (feature details)

## Execution

For each page in the page inventory, generate a spec file at `.loom/design/screens/[page-id].md`.

Each spec follows this structure:

---

**Page: [Page Name] (`[page-id]`)**

### Purpose
One sentence — what does this page help the user accomplish?

### Layout

```
┌─────────────────────────────────┐
│ [Header / Nav]                  │
├─────────────────────────────────┤
│ [Main content area]             │
│   [Component A]                 │
│   [Component B]                 │
├─────────────────────────────────┤
│ [Footer / action bar]           │
└─────────────────────────────────┘
```

### Components

| Component | Type | Content | Behavior |
|-----------|------|---------|----------|
| [name] | button / input / list / card / ... | [what it shows] | [what happens on interaction] |

### States

| State | Trigger | UI change |
|-------|---------|-----------|
| Loading | Data fetch in progress | Skeleton placeholder |
| Empty | No data yet | Empty state message + CTA |
| Error | Request failed | Inline error + retry option |

### Navigation

| Action | Destination |
|--------|-------------|
| [user action] | [page-id or external] |

---

## Checkpoint

If a page requires a component not covered by the design tokens (e.g. a data visualization, a map, a rich text editor), pause and ask:

```
[Page name] needs a [component type] that isn't covered by the current design system.

Options:
1. Use a third-party library — specify which one
2. Describe custom behavior for me to spec manually
3. Simplify — replace with a basic alternative
```

## Output (persistent → .loom/design/screens/)

One markdown file per page: `.loom/design/screens/[page-id].md`

After all pages are written, also write `.loom/design/screens/index.md` with a summary table:

| Page ID | Name | File |
|---------|------|------|
| P01 | [name] | [screens/p01.md] |
