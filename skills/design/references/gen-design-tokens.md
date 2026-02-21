# gen-design-tokens
# Layer: 2 Executor
# Output: persistent → .loom/design/system.md

## Purpose

Generate a design token specification — color, typography, spacing, and border radius — that fits the product's character and target user.

## Input

- Required: `.loom/ideation/requirements.md`
- Optional: transient brief from ideation stage (product_summary, target_user, differentiation)

## Execution

Derive design direction from the product description, then generate tokens:

**1. Infer style direction** from the product:
- Consumer vs. professional tool → affects density and formality
- Target user age/context → affects contrast and font size
- Product differentiation → affects brand personality (e.g. "compliance-focused" → conservative palette)

**2. Generate color tokens:**

| Token | Value | Usage |
|-------|-------|-------|
| `color.brand.primary` | #hex | Primary actions, key highlights |
| `color.brand.secondary` | #hex | Supporting accents |
| `color.neutral.0–900` | scale | Backgrounds, borders, text |
| `color.semantic.success` | #hex | Confirmations |
| `color.semantic.warning` | #hex | Cautions |
| `color.semantic.error` | #hex | Errors |
| `color.semantic.info` | #hex | Informational |

**3. Generate typography tokens:**

| Token | Value |
|-------|-------|
| `font.family.base` | font stack |
| `font.family.mono` | monospace stack (if needed) |
| `font.size.xs–3xl` | scale in rem |
| `font.weight.regular / medium / bold` | 400 / 500 / 700 |
| `font.lineHeight.tight / normal / loose` | values |

**4. Generate spacing tokens:**

| Token | Value |
|-------|-------|
| `space.1–12` | 4px-based scale |

**5. Generate border radius tokens:**

| Token | Value |
|-------|-------|
| `radius.sm / md / lg / full` | values |

## Checkpoint

If the product description is too vague to infer a clear style direction, pause and ask:

```
I need a style direction before generating design tokens.

Please choose one (or describe your own):
1. Clean / minimal — neutral palette, tight spacing, professional
2. Friendly / approachable — warm colors, generous spacing, rounded corners
3. Bold / expressive — strong brand color, high contrast, distinctive
```

## Output (persistent → .loom/design/system.md)

Write a markdown file with all token tables and a brief rationale for the style choices.
