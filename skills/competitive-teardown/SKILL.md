---
name: competitive-teardown
description: >
  This skill should be used when the user wants to map existing players in a target market —
  their positioning, moat, and what is genuinely undefended — to pressure-test whether a market
  gap actually exists. Takes market name, known competitors, and entrant context; returns a
  structured competitor map and gap analysis.
disable-model-invocation: true
argument-hint: "[market] [competitors] [entrant-context]"
allowed-tools: WebSearch Write
---

# Competitive Teardown

Map existing players in the target market — their positioning, moat, and what is genuinely undefended — to pressure-test whether a gap actually exists.

## Workflow

1. **Parse inputs** — Extract from `$ARGUMENTS`:
   - Market name and segment (e.g., "India digital payments — domestic retail")
   - Named competitors (or none provided — then surface them via search)
   - Entrant context (e.g., "PayPal — cross-border payments strength, limited India domestic presence")
   If entrant context is missing, ask before proceeding.

2. **Surface competitors (if not provided)** — Use WebSearch to identify the key players in the market segment. Look for:
   - Market leaders by volume/share
   - Challengers and niche players
   - Any recent entrants

3. **Analyse each competitor** — For each player, research and record:
   - **Positioning:** What customer problem they solve and for whom
   - **Moat:** What makes them hard to displace (network effects, regulatory approval, distribution, pricing, technology)
   - **Weakness:** Where they are vulnerable or where customers are underserved
   - **Relevance to entrant:** Does this competitor directly block the entrant's hypothesized path?

4. **Run gap analysis** — Based on competitor map:
   - Identify gaps that are genuinely undefended (no competitor owns them with a strong moat)
   - Identify gaps that are contested but potentially winnable (weak moat, underserving, or dissatisfied users)
   - Flag any hypothesized gap that is already owned — do not soften this finding

5. **Check entrant fit** — Map the entrant's stated strengths against identified gaps. Flag mismatches explicitly.

6. **Write output** — Save to `outputs/competitive-teardown-[market].md`.

## Outputs

### `outputs/competitive-teardown-[market].md`

```
# Competitive Teardown: [Market Name]

**Date:** [today's date]
**Entrant:** [company + context]

---

## Competitor Map

| Player | Positioning | Moat | Weakness | Blocks Entrant? |
|--------|-------------|------|----------|-----------------|
| ...    | ...         | ...  | ...      | Yes / No / Partial |

---

## Gap Analysis

### Undefended Gaps
[Bullet list — gap description + why it is genuinely uncontested]

### Contested but Winnable
[Bullet list — gap description + why entrant could win despite competition]

### Gaps Already Owned (Entrant Should Not Enter)
[Bullet list — gap description + who owns it + why moat is strong]

---

## Entrant Fit Assessment
[Map entrant strengths to gaps. Flag mismatches.]

---

## Sources
[List sources with URLs where available]
```

## Guidelines

- Do not assume a gap exists without checking who already owns it
- Distinguish between gaps that are uncontested vs. contested but winnable — these require different strategies
- If all gaps appear owned, surface this honestly — do not force an opportunity
- If insufficient public data exists on a competitor, flag it as "requires primary research" — do not fabricate
- Conflicting signals on market share or positioning → surface both, do not resolve
