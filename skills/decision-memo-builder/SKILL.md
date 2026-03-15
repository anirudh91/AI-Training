---
name: decision-memo-builder
description: >
  This skill should be used when the user wants to package all market entry research findings
  into a one-page Go/No-Go decision memo ready for leadership and finance. Takes value proposition
  statement, feasibility summary, key findings, and open risks; returns a structured three-section
  memo with a clear recommendation.
disable-model-invocation: true
argument-hint: "[value-prop] [feasibility-summary] [key-findings] [open-risks]"
allowed-tools: Read Write
---

# Decision Memo Builder

Package all research findings into a one-page Go/No-Go decision memo ready for leadership and finance.

## Workflow

1. **Parse inputs** — Collect from `$ARGUMENTS` or ask the user to provide:
   - Value proposition statement (from Step 4 value-prop-synthesizer output)
   - Feasibility summary (from Step 4b feasibility checker)
   - Key findings from secondary and primary research
   - Open risks and questions
   If the monetization hypothesis is not provided or unclear, prompt the user to state it explicitly before generating the memo. Do not proceed without it.

2. **Draft the three-section memo** — Write exactly three sections, one page discipline:
   - **Section 1 — What we found:** Key findings, value prop, target segment (facts only)
   - **Section 2 — What it means:** Implications, monetization hypothesis with timeline, confidence level
   - **Section 3 — What we recommend:** Go / No-Go / Investigate Further — with rationale and open risks listed explicitly

3. **Enforce one-page discipline** — If the draft exceeds one page:
   - Trim Section 1 to the 3 most important findings only
   - Move supporting detail to an appendix section at the end
   - Keep the recommendation section complete — never trim it

4. **List open risks prominently** — Open risks go in Section 3, not buried in Section 1. Format as a numbered list.

5. **Add financial modeling assumptions** — If TAM, revenue model, or cost structure data is available, include it as a brief table. If not available, flag explicitly: "Financial modeling assumptions: Not available — recommend finance team review before Go decision."

6. **Write output** — Save to `outputs/decision-memo-[market].md`.

## Outputs

### `outputs/decision-memo-[market].md`

```
# Decision Memo: [Market Name]

**Date:** [today's date]
**Owner:** [PM name if provided]
**Recommendation:** Go / No-Go / Investigate Further

---

## 1. What We Found

- **Value proposition:** [Company] can win in [segment] by [differentiation]
- **Target segment:** [specific customer segment]
- **Key finding 1:** [most important finding]
- **Key finding 2:** [second most important finding]
- **Key finding 3:** [third most important finding]

---

## 2. What It Means

**Monetization hypothesis:** [Company] can generate [revenue model] in [segment] by [year], targeting [scale].

**Confidence level:** High / Medium / Low — [one sentence rationale]

**Implications:**
- [Implication 1]
- [Implication 2]

---

## 3. What We Recommend

**Recommendation: Go / No-Go / Investigate Further**

**Rationale:** [2–3 sentences explaining the recommendation]

**Open risks:**
1. [Risk with implication]
2. [Risk with implication]
3. [Risk with implication]

---

## Financial Modeling Assumptions (if available)

| Item | Estimate | Source |
|------|---------|--------|
| TAM  | ...     | ...    |
| Revenue model | ... | ... |
| Cost structure | ... | ... |

*If not available:* Financial modeling assumptions not available — recommend finance team review before Go decision.

---

## Appendix (if needed)
[Supporting detail trimmed from main memo for length]
```

## Guidelines

- Enforce one-page discipline for Sections 1–3 — move overflow to Appendix, never the recommendation
- Monetization hypothesis must be explicit and time-bound (3–5 year horizon) — prompt the user if missing
- Open risks go in Section 3, listed explicitly — they must never be buried
- "Investigate Further" is a valid recommendation when evidence is directionally positive but incomplete
- Do not generate the memo without a monetization hypothesis — an incomplete memo is worse than no memo
