---
name: research-brief-builder
description: >
  This skill should be used when the user wants to structure validated hypotheses into a tiered
  primary research question document with kill criteria — sharp enough to hand to a research team.
  Takes surviving hypotheses, stakeholder pushback notes, and kill criteria; returns a research
  brief with Tier 1 and Tier 2 questions in directional format.
disable-model-invocation: true
argument-hint: "[surviving-hypotheses] [stakeholder-notes] [kill-criteria]"
allowed-tools: Read Write
---

# Research Brief Builder

Structure validated hypotheses into a tiered primary research question document with kill criteria — sharp enough to hand to a research team.

## Workflow

1. **Parse inputs** — Collect from `$ARGUMENTS` or ask the user to provide:
   - Surviving hypotheses (scored list from Step 2 — Confirmed or Weakened)
   - Stakeholder pushback notes (from Step 3 conversations — what did stakeholders challenge or question?)
   - Kill criteria: 2–3 conditions that would stop the opportunity entirely
   If kill criteria are not provided, prompt the user to define at least 2 before generating the brief. Example prompt: "Please provide 2–3 conditions that would lead you to stop this investigation. Example: 'Regulatory approval takes 3+ years' or 'TAM is under $500M'."

2. **Frame research questions** — Convert each surviving hypothesis into primary research questions:
   - Every question must follow this format: "Does [specific segment] experience [specific problem]?"
   - NOT open-ended (not "Tell me about SMB payment needs")
   - Every question must have a directional implication: "If yes → [what changes]. If no → [what changes]."
   - Remove any question where a yes/no answer would not change the direction of the investigation

3. **Tier the questions** — Assign each question to a tier:
   - **Tier 1 (Must Answer):** Questions whose answer would directly validate or kill a surviving hypothesis
   - **Tier 2 (Good to Have):** Questions that add depth or reduce risk but are not directionally critical

4. **Incorporate stakeholder pushback** — For each stakeholder challenge noted:
   - Check whether an existing question already addresses it
   - If not, add a new question (Tier 1 or 2 as appropriate)
   - If the pushback suggests a loop-back to Step 2 is needed, flag this explicitly before proceeding

5. **Write kill criteria** — For each kill condition provided by the PM:
   - Make it specific: who decides, what threshold, what timeframe
   - Example: "If primary research shows fewer than 30% of target SMBs have cross-border payment needs, stop."

6. **Write output** — Save to `outputs/research-brief-[market].md`.

## Outputs

### `outputs/research-brief-[market].md`

```
# Primary Research Brief: [Market Name]

**Date:** [today's date]
**Surviving hypotheses:** [list]
**Owner:** [PM name if provided]

---

## Tier 1 — Must Answer

| # | Research Question | Hypothesis Tested | If Yes → | If No → |
|---|-------------------|-------------------|----------|---------|
| 1 | Does [segment] experience [problem]? | H[X] | [direction] | [direction] |

---

## Tier 2 — Good to Have

| # | Research Question | Hypothesis Tested | If Yes → | If No → |
|---|-------------------|-------------------|----------|---------|

---

## Kill Criteria

If any of the following conditions are confirmed by primary research, the investigation stops:

1. [Specific, time-bound kill condition]
2. [Specific, time-bound kill condition]
3. [If provided]

---

## Focus Areas for Research Design

- **Value proposition:** [Key value prop question to probe]
- **Customer segments:** [Target segments to recruit]
- **Monetization potential:** [Pricing/willingness-to-pay angles to explore]

---

## Stakeholder Pushback Addressed

| Pushback | Question Added | Tier |
|----------|---------------|------|
| ...      | ...           | ...  |
```

## Guidelines

- Every question must have a clear directional implication — remove questions that don't
- Frame questions as "Does [segment] experience [problem]?" — never open-ended
- Kill criteria must be specific and time-bound — do not accept vague criteria like "if the market is too small"
- If stakeholder pushback suggests returning to Step 2, flag this before generating the brief
- Do not add questions beyond what is needed to validate the surviving hypotheses — keep the brief actionable
