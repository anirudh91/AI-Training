---
name: hypothesis-pressure-tester
description: >
  This skill should be used when the user wants to score market entry hypotheses against
  secondary data and return a shortlist of validated or data-backed hypotheses. Takes a list
  of 2–4 hypotheses and secondary research sources; returns each hypothesis scored as
  Confirmed, Weakened, or Killed — with evidence — and flags open questions for primary research.
disable-model-invocation: true
argument-hint: "[hypotheses-list] [secondary-research-sources]"
allowed-tools: WebSearch Write
---

# Hypothesis Pressure-Tester

Score each hypothesis against secondary data and return a shortlist of validated (or at least data-backed) hypotheses.

## Workflow

1. **Parse inputs** — Extract from `$ARGUMENTS`:
   - List of 2–4 hypotheses (from Step 1 market orientation output)
   - Secondary research sources or pasted findings (eMarketer excerpts, news articles, reports)
   If no hypotheses are provided, ask the user to paste them before proceeding.

2. **Supplement with search (if needed)** — If secondary research is sparse, use WebSearch to find additional evidence relevant to each hypothesis. Search for:
   - Data points that confirm or contradict the hypothesis
   - Market reports, analyst commentary, news coverage
   - Comparable market analogies (other geographies, similar entrants)

3. **Score each hypothesis** — Apply the following scoring rules strictly:
   - **Confirmed:** Multiple independent sources support the hypothesis with specific data or evidence
   - **Weakened:** Some evidence exists but is partial, contested, or limited to a specific sub-segment — keep with caveats, do not kill
   - **Killed:** Positive evidence exists that directly contradicts the hypothesis — absence of evidence alone is NOT sufficient to kill
   For each score, cite the specific evidence used.

4. **Build shortlist** — Identify 1–3 surviving hypotheses (Confirmed or Weakened). These proceed to Step 3.

5. **Flag open questions** — List what secondary research could not resolve. Each open question must:
   - State what is unknown
   - Explain why it matters (what direction would change if answered)
   - Flag it as requiring primary research

6. **Write output** — Save to `outputs/hypothesis-scored-[market].md`.

## Outputs

### `outputs/hypothesis-scored-[market].md`

```
# Hypothesis Scored Shortlist: [Market Name]

**Date:** [today's date]

---

## Scored Hypotheses

| Hypothesis | Score | Evidence Summary | Source(s) |
|-----------|-------|-----------------|-----------|
| H1: ...   | Confirmed / Weakened / Killed | [evidence] | [source] |
| H2: ...   | ...   | ...             | ...       |

---

## Surviving Shortlist (Proceed to Step 3)

**H[X] — [Score with caveats if Weakened]**
Summary: [1–2 sentences on what the evidence supports and what remains uncertain]

---

## Open Questions for Primary Research

1. **[Question]** — Why it matters: [directional implication if answered yes vs. no]
2. ...

---

## Sources
[Full list of sources used]
```

## Guidelines

- Score based on evidence, not intuition
- "Weakened" means partially supported — keep with caveats; never kill without positive counter-evidence
- "Killed" requires direct contradicting evidence, not just absence of data
- If no secondary data exists for a hypothesis, classify as high-uncertainty and flag for primary research — do not kill it
- If secondary data appears AI-generated or unverifiable, note this and prompt the user to verify against original sources before accepting the score
- Do not force a surviving shortlist — if all hypotheses are killed by evidence, surface this honestly
