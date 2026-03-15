---
name: value-prop-synthesizer
description: >
  This skill should be used when the user wants to cluster primary research findings into a
  clear value proposition statement with supporting evidence. Takes primary research findings,
  surviving hypotheses, and company capabilities context; returns a value prop statement,
  supporting evidence, open risks, and a monetization path assessment.
disable-model-invocation: true
argument-hint: "[primary-research-findings] [surviving-hypotheses] [capabilities-context]"
allowed-tools: Read Write
---

# Value Prop Synthesizer

Cluster primary research findings into a clear value proposition statement with supporting evidence.

## Workflow

1. **Parse inputs** — Collect from `$ARGUMENTS` or ask the user to provide:
   - Primary research findings document (from research team post-Step 3)
   - Surviving hypotheses from Step 2 scored shortlist
   - Company capabilities context (e.g., PayPal Capabilities Brief — read from file if available)
   If primary research findings are not provided, stop and ask — do not synthesize without them.

2. **Identify the three elements separately** — Before synthesizing, identify each element independently:
   - **Critical differentiator:** What can this company do that competitors cannot or do not?
   - **Underserved segment:** Which specific customer segment is most underserved, per research findings?
   - **Monetization path:** What is the viable revenue model over a 3–5 year horizon?
   If any of the three elements is missing or inconclusive, flag it explicitly — do not proceed to synthesis.

3. **Synthesize the value proposition** — Only if all three elements are present:
   - Draft: "[Company] can win in [segment] by [differentiation] and monetize via [model]"
   - Support it with 3–5 specific findings from primary research
   - Identify open risks: what primary research left unanswered that could undermine the value prop

4. **Assess monetization path** — Rate the monetization path:
   - **Yes:** Clear revenue model with comparable analogues and willingness-to-pay evidence
   - **Uncertain:** Model exists but evidence on pricing, scale, or timing is weak
   - **No:** No viable monetization path found — state this clearly

5. **Write output** — Save to `outputs/value-prop-[market].md`.

## Outputs

### `outputs/value-prop-[market].md`

```
# Value Proposition: [Market Name]

**Date:** [today's date]
**Based on:** [research brief reference + primary research date]

---

## Value Proposition Statement

> [Company] can win in [segment] by [differentiation] and monetize via [model].

---

## Supporting Evidence

| Finding | Source | Supports |
|---------|--------|---------|
| ...     | Primary research | Differentiator / Segment / Monetization |

---

## Open Risks

[Bullet list of what primary research left unanswered — each with its implication]

---

## Monetization Path

**Assessment:** Yes / Uncertain / No

[Explanation — revenue model, pricing evidence, comparable analogues, timeline]

---

## Missing Elements (if any)

[If any of the three elements — differentiator, segment, monetization — is missing, list here with explanation. Do not synthesize if this section is non-empty.]
```

## Guidelines

- Surface the critical differentiator, underserved segment, and monetization path separately before synthesizing — never collapse them prematurely
- Flag if any of the three elements is missing — do not force a value prop with incomplete inputs
- If findings are inconclusive, output open questions only — not a fabricated value prop
- If value prop exists but monetization path is unclear, output: "Value prop exists but monetization path is unclear — park or kill"
- Do not generalise findings beyond what primary research actually shows
