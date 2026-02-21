# gen-architecture
# Layer: 2 Executor
# Output: persistent → .loom/architecture/architecture.md

## Purpose

Generate a technical architecture document: stack choices, module breakdown, data models, and API design.

## Input

- Required: `.loom/design/system.md`
- Required: transient risk analysis from analyze-risks
- Optional: `.loom/ideation/requirements.md` (for non-functional requirements)

## Execution

**1. Stack selection** — propose a technology stack and justify each choice:

| Layer | Choice | Justification |
|-------|--------|---------------|
| Frontend | [framework] | [reason] |
| Backend | [framework/runtime] | [reason] |
| Database | [type + name] | [reason] |
| Auth | [approach] | [reason] |
| Hosting | [platform] | [reason] |
| Key libraries | [list] | [reason] |
| Test | [framework + command] | [reason] |

Cross-reference the risk analysis: if a high-risk area was flagged, the stack choice must address it.

**2. Module breakdown** — list top-level modules and their responsibilities:

```
[Module Name]
  Responsibility: [what it does]
  Inputs: [data it receives]
  Outputs: [data it produces]
  Dependencies: [other modules or external services]
```

**3. Data models** — define core entities:

```
[Entity Name]
  Fields:
    - [field]: [type] — [description]
  Relations:
    - [entity] [relation type] [entity]
```

**4. API design** — list key endpoints or service contracts:

| Method | Path | Input | Output | Auth |
|--------|------|-------|--------|------|
| GET | /[resource] | — | [response shape] | [required/none] |
| POST | /[resource] | [body shape] | [response shape] | [required/none] |

## Checkpoint

If a stack decision requires user input (e.g. the product has existing infrastructure constraints, or two valid options are equally appropriate), pause and ask:

```
Stack decision needed for [layer]:

Option A: [name] — [tradeoffs]
Option B: [name] — [tradeoffs]

Which should I use?
```

## Output (persistent → .loom/architecture/architecture.md)

Write a markdown file with all four sections: stack, modules, data models, API design.
