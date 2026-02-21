# analyze-competitors
# Layer: 2 Executor
# Output: transient

## Purpose

Map the competitive landscape based on the brief and identify market gaps.

## Input

transient: brief (from analyze-brief)

## Execution

Based on `product_summary` and `target_user`, identify 3–5 existing solutions and evaluate each:

| Competitor | Core approach | Strengths | Weaknesses | Market gap left open |
|------------|---------------|-----------|------------|----------------------|
| ...        | ...           | ...       | ...        | ...                  |

Then summarize:
- **Market gap**: What no existing solution does well for this specific user
- **Risk**: If the market gap is already covered, flag it explicitly

## Checkpoint

Pause if the market gap analysis concludes the space is saturated with no clear opening:

```
Competitor analysis suggests this space is well-covered:
[list top competitors and what they do]

The differentiation angle you mentioned ("[differentiation]") overlaps with [competitor].
Do you want to:
1. Refine the differentiation angle
2. Proceed anyway — you have context I don't
```

## Output (transient)

```
competitors:
  - name: ...
    approach: ...
    gap_left: ...
market_gap: ...
saturation_risk: low | medium | high
```
