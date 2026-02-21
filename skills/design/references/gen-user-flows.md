# gen-user-flows
# Layer: 2 Executor
# Output: persistent → .loom/design/flows.md

## Purpose

Generate user flow diagrams (text-based) and a page relationship map from the requirements document.

## Input

- Required: `.loom/ideation/requirements.md`
- Required: `.loom/design/system.md` (to confirm design context)

## Execution

**1. Extract entry points** — identify all distinct ways a user enters the product:
- Onboarding / first-time setup
- Returning user login
- Deep links or external triggers (e.g. email notification → specific page)

**2. Map core task flows** — for each must-have feature, trace the full user path:

```
Flow: [Feature Name]
Trigger: [what starts this flow]
Steps:
  1. [Page/State] → [action] → [Page/State]
  2. [Page/State] → [action] → [Page/State]
  ...
End state: [what success looks like]
Error paths:
  - [condition] → [fallback page/state]
```

**3. Build page inventory** — list every unique page/screen:

| Page ID | Name | Reached from | Leads to |
|---------|------|-------------|----------|
| P01 | [name] | [entry points] | [next pages] |

**4. Draw page relationship map** (text diagram):

```
[Page A] --action--> [Page B]
         --action--> [Page C]
[Page B] --action--> [Page D]
```

## Checkpoint

If a flow has an ambiguous branch (e.g. "what happens if the user cancels mid-flow?"), pause and ask:

```
Ambiguous flow branch in [Feature Name]:
[description of the branch]

Options:
1. [option A]
2. [option B]

Which behavior is intended?
```

## Output (persistent → .loom/design/flows.md)

Write a markdown file with:
- All core task flows (step-by-step)
- Complete page inventory table
- Page relationship map (text diagram)
