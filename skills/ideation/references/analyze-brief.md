# analyze-brief
# Layer: 2 Executor
# Output: transient

## Purpose

Parse the user's raw one-line idea and extract the core elements needed for downstream analysis.

## Input

Raw description provided by the user in the conversation (any format).

## Execution

Extract the following fields. Mark missing fields as "TBD":

| Field | Description | Example |
|-------|-------------|---------|
| product_summary | One sentence describing what this product is | "A tool for freelancers to manage invoicing and payments" |
| target_user | Specific user group — the more specific the better | "Independent designers in China taking overseas clients" |
| core_problem | How users currently solve this, and where it hurts | "WeChat transfers + manual bookkeeping, reconciliation is a mess" |
| differentiation | The angle this product takes vs. existing solutions | "Focused on cross-border payment compliance — other tools ignore this" |

## Checkpoint

Pause and ask the user if both `target_user` and `core_problem` are "TBD":

```
I need two things before continuing:
1. Who is this product mainly for? (specific is better — e.g. "college students" not "everyone")
2. How do they currently solve this problem? What's the most painful part?
```

## Output (transient)

```
brief:
  product_summary: ...
  target_user: ...
  core_problem: ...
  differentiation: ...
```
