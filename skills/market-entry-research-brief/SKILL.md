---
name: market-entry-research-brief
description: >
  This skill should be used when the user wants to run a full market entry research workflow —
  from zero context to a Go/No-Go decision memo. Orchestrates the end-to-end sequence:
  market orientation, hypothesis validation, research brief, value prop synthesis, and decision
  memo. The PM drives each step and makes all go/no-go decisions at human gates. Invoke with
  a market name and company context to begin.
disable-model-invocation: true
argument-hint: "[market-name] [company-context]"
allowed-tools: WebSearch Read Write
---

# Market Entry Research Brief

Run the full market entry research workflow — from zero context to a Go/No-Go decision memo.

## Overview

This skill orchestrates 5 steps across 7 component skills and 1 agent. You (the PM) drive the sequence. At each human gate, you review the output and decide whether to proceed, loop back, or stop.

```
Step 1 → Market Orientation         → Human gate: enough signals to form hypotheses?
Step 2 → Hypothesis Validation      → Human gate: which hypotheses survive?
Step 3 → Research Brief             → Human gate: questions sharp enough for research team?
Step 4 → Value Prop Synthesis       → Human gate: defensible value prop identified?
Step 5 → Decision Memo              → Human gate: Leadership Go/No-Go
```

---

## Workflow

### Step 1 — Market Orientation

**Run:** `/market-orientation-query $ARGUMENTS`

If `$ARGUMENTS` is not provided, ask the user:
- What market are you entering? (e.g., "India digital payments")
- What is the company/entrant context? (e.g., "PayPal entering India, UPI is dominant")

Then run the market-orientation-query skill with those inputs. Wait for output.

**Human gate:** Present the draft hypotheses to the user and ask:
> "Here are your draft hypotheses. Do you have enough signals to proceed to validation, or do you want to refine any of them first? Also — have you spoken to any peer PMs who know this market? Their input can be fed in here before we proceed."

Only proceed to Step 2 when the user confirms.

---

### Step 2 — Hypothesis Validation (Agent)

**Run:** Launch the `hypothesis-validation-agent` with the confirmed hypotheses.

Tell the user:
> "Launching the Hypothesis Validation Agent. It will autonomously run competitive teardown, regulatory feasibility check, and hypothesis scoring — then return a single scored shortlist. This may take a few minutes."

Pass to the agent:
- The market name and company context
- The confirmed hypothesis list from Step 1

Wait for the agent's consolidated scored output.

**Human gate:** Present the scored shortlist and ask:
> "Here are your scored hypotheses. Which ones do you want to take forward to the research brief? You can proceed with Confirmed and Weakened hypotheses. Killed hypotheses are listed for your reference."

If the user wants to loop back (e.g., stakeholder challenged a hypothesis), return to Step 1.
Only proceed to Step 3 when the user selects surviving hypotheses.

---

### Step 3 — Research Brief

**Run:** `/research-brief-builder`

Pass to the skill:
- Surviving hypotheses selected by the PM
- Any stakeholder pushback notes (ask the user: "Have you had any stakeholder conversations since Step 2? Any pushback or new constraints to incorporate?")
- Kill criteria (ask the user: "Please provide 2–3 conditions that would stop this investigation entirely. Example: 'Regulatory approval takes 3+ years' or 'TAM under $500M'.")

Wait for output.

**Human gate:** Present the research brief and ask:
> "Here is your research brief. Are the questions sharp enough to hand to your research team? Does this reflect any stakeholder feedback? If yes, I'll save the brief and you can hand it off. If not, let me know what to adjust."

Save the brief to `outputs/research-brief-[market].md`.

---

### Step 4 — Value Prop Synthesis

**Run:** `/value-prop-synthesizer`

This step requires primary research findings from the research team. Ask the user:
> "Has your research team completed primary research? Please paste the key findings here, or provide the file path if you have a research findings document."

If findings are not yet available, pause here and tell the user:
> "Step 4 requires primary research findings. Resume this workflow when your research team has completed their work by running `/market-entry-research-brief` again and telling me you're at Step 4."

Once findings are provided, pass to the skill:
- Primary research findings
- Surviving hypotheses from Step 2
- Company capabilities context (ask: "Do you have a capabilities brief for [company]? If yes, provide the file path or paste the key points.")

Wait for output.

**Human gate:** Present the value prop and ask:
> "Here is your value proposition. Is this defensible? Does it reflect what the research showed? If the value prop is unclear or the monetization path is missing, we can loop back to Step 3 to sharpen the research questions."

If looping back, return to Step 3.

---

### Step 5 — Decision Memo

**Run:** `/decision-memo-builder`

Pass to the skill:
- Value prop statement from Step 4
- Any feasibility notes (ask: "Have you run an internal feasibility check? If yes, paste the summary. If not, I'll flag it as TBD in the memo.")
- Key findings summary
- Open risks identified across Steps 1–4

Wait for output.

**Human gate:** Present the memo and ask:
> "Here is your one-page Go/No-Go memo. Is this ready for leadership and finance? Would you like to adjust the recommendation or add any context before sharing?"

Save final memo to `outputs/decision-memo-[market].md`.

---

### Completion

When the memo is approved, tell the user:

> "Your market entry research brief workflow is complete. Here is a summary of all outputs:"

List all output files produced during the workflow.

---

## Outputs

All outputs saved to `outputs/` directory:

| File | Step | Contents |
|------|------|---------|
| `outputs/market-orientation-[market].md` | 1 | Market picture + draft hypotheses |
| `outputs/hypothesis-scored-[market].md` | 2 | Scored shortlist from agent |
| `outputs/research-brief-[market].md` | 3 | Tiered research questions + kill criteria |
| `outputs/value-prop-[market].md` | 4 | Value prop + supporting evidence |
| `outputs/decision-memo-[market].md` | 5 | Go/No-Go memo for leadership |

## Guidelines

- Never skip a human gate — the PM owns every proceed/loop-back/stop decision
- If the user is mid-workflow and returns later, ask which step they are resuming from
- If any component skill is not found, tell the user which skill is missing and where to find it: `~/.claude/skills/[skill-name]/SKILL.md`
- Do not generate outputs for steps the user has not confirmed — do not run ahead
- Steps 4 and 5 depend on external inputs (primary research, feasibility check) that may not be immediately available — pause gracefully and tell the user how to resume
