# gen-prd
# Layer: 2 Executor
# Output: persistent → .loom/ideation/requirements.md, .loom/ideation/brief.md, .loom/ideation/research.md

## Purpose

Consolidate all ideation stage outputs into a complete requirements document, plus standalone brief and research files.

## Input

transient: brief, user_stories, competitors, requirements (all prior steps)

## Execution

### 1. Write requirements document

Write a requirements document with the following structure:

```markdown
# [Product Name] — Requirements

## Overview
[product_summary]
Target user: [target_user]
Core problem: [core_problem]
Differentiation: [differentiation]

## Market Context
[3-line summary of competitive landscape and identified market gap]

## Requirements

### P0 — Must Have
- [ ] [story]
- [ ] [story]

### P1 — Should Have
- [ ] [story]

### P2 — Nice to Have
- [ ] [story]

## Open Questions
[list any unresolved conflicts or missing items from analyze-requirements]
```

### 2. Write brief file

Extract the brief data from analyze-brief's transient output and write:

```markdown
# Brief

## Product Summary
[product_summary]

## Target User
[target_user]

## Core Problem
[core_problem]

## Differentiation
[differentiation]
```

### 3. Write research file

Extract the competitor data from analyze-competitors's transient output and write:

```markdown
# Market Research

## Competitors

| Name | Approach | Gap Left |
|------|----------|---------|
| ... | ... | ... |

## Market Gap
[market_gap]

## Saturation Risk
low | medium | high
```

## Output (persistent)

Pass each document to `infra.md` for writing:
- Requirements → `.loom/ideation/requirements.md`
- Brief → `.loom/ideation/brief.md`
- Research → `.loom/ideation/research.md`
