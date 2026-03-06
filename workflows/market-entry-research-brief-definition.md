# Workflow Definition: Market Entry Research Brief

---

## Scenario Metadata

| Field | Value |
|---|---|
| **Workflow Name** | Market Entry Research Brief |
| **Lens** | Individual |
| **Type** | Guided |
| **Execution Mode** | Augmented |
| **Trigger** | PM identifies a new market to enter with zero prior context (blank slate) |
| **Process Outcome** | A layered research brief: secondary findings + tiered primary research questions, ready to guide interviews, team research sprints, and stakeholder alignment |
| **Business Objective** | Identify whether PayPal (or similar entrant) has a viable value proposition and monetization path in a new market (e.g. India / UPI) over a 3–5 year horizon |
| **Current Owner** | Anirudh (Product Manager) |
| **Stakeholders** | PM (owner), Peer PMs / Market PMs (input), Manager (pressure-test), Research Team (execution), Leadership + Finance (decision) |
| **Success Metrics** | Quality and sharpness of primary research questions; % of hypotheses confirmed or killed by secondary research; speed from blank slate to brief ready for research team |

---

## Refined Steps

### Step 1 — Market Orientation

**Action:** Get a high-level picture of the market and form early hypotheses — using two parallel tracks simultaneously.

**Sub-steps:**
- Track A: Open Claude or ChatGPT and ask broad questions about the market landscape, key players, dynamics, and potential gaps
- Track B: Talk to peer PMs and market PMs to gather experiential knowledge and source pointers (e.g. eMarketer, industry reports)
- Combine both tracks to form 2–4 early hypotheses about where gaps or opportunities might exist

**Decision Points:**
- Are there enough signals from both tracks to form at least one hypothesis? If yes → proceed. If no → widen the net (more conversations, broader AI queries)

**Data In:**
- Market name + product context (e.g. "PayPal entering India, UPI is dominant")
- PM network knowledge
- AI-generated market overviews

**Data Out:**
- High-level market picture (in your head or rough notes)
- 2–4 early hypotheses (unvalidated)

**Context Needs:**
- Access to eMarketer or similar research platforms
- Network of market/peer PMs with relevant experience
- Claude or GPT access

**Failure Modes:**
- Hypotheses are too vague to pressure-test → push for more specificity before moving on
- Conflicting signals from AI vs. human input → flag as a hypothesis to resolve, don't pick one arbitrarily

---

### Step 2 — Hypothesis Pressure-Testing

**Action:** Take each early hypothesis back to AI with targeted questions, and find secondary research data to confirm or kill them. Solo activity — sit with findings before sharing.

**Sub-steps:**
- For each hypothesis, form a specific AI question (not a broad one)
- **Run a structured competitive teardown**: Map existing players (PhonePe, Google Pay, Paytm etc.) — their positioning, moat, and what's genuinely undefended. Don't assume a gap exists without checking who already owns it.
- **Regulatory feasibility check**: Explicitly ask — can PayPal legally operate in this space? (Note: PayPal shut down domestic India payments in 2021 due to RBI data localisation issues. Always surface regulatory blockers early.)
- Search for secondary data: eMarketer reports, industry publications, competitor analysis
- Score each hypothesis: Confirmed / Weakened / Killed
- Retain only hypotheses with data backing

**Decision Points:**
- Is there enough secondary data to make a call on each hypothesis?
- Which hypotheses survive and are strong enough to take to stakeholders?

**Data In:**
- 2–4 early hypotheses from Step 1
- Secondary research sources (eMarketer, news, reports)

**Data Out:**
- Shortlist of 1–3 validated (or at least data-backed) hypotheses
- Notes on what secondary research confirmed vs. left unanswered

**Context Needs:**
- eMarketer access or equivalent
- Targeted AI prompts (Claude/GPT)
- Specific market data: UPI transaction volumes, PayPal India presence, India fintech landscape, regulatory context

**Failure Modes:**
- No secondary data exists for a hypothesis → treat as high-uncertainty, flag for primary research
- Over-reliance on AI-generated data without verifying sources → always check original sources

---

### Step 3 — Stakeholder Pressure-Test & Research Agenda

**Action:** Share surviving hypotheses with manager and market PMs to get pushback or new angles. Then convert validated hypotheses into a pointed research brief.

**Sub-steps:**
- Share hypotheses informally (1:1 conversations or quick sync)
- Two paths:
  - **Disagreement / new angles** → rework hypotheses, add new angles, loop back to Step 2
  - **Alignment** → move forward to building the brief
- Structure surviving hypotheses into a tiered research question document
- Focus questions on identifying the value proposition in the target market
- **Define explicit kill criteria**: Before handing off, agree on 2–3 conditions that would stop the opportunity (e.g. "if regulatory approval takes 3+ years" or "if no segment can generate $Xm revenue in 5 years"). This keeps the business case clean and prevents drift.
- Output: a document sharp enough to hand to a research team

**Decision Points:**
- Do stakeholders broadly agree with the direction? (Gate to proceed)
- Are the research questions pointed enough — i.e. not "tell me about X" but "does segment Y experience problem Z?"

**Data In:**
- Validated hypotheses from Step 2
- Stakeholder perspectives (managers, market PMs)

**Data Out:**
- Research brief document with tiered primary research questions
- Focus on: value proposition identification, customer segments, monetization potential

**Context Needs:**
- Stakeholder availability for informal syncs
- Template or structure for the research brief (to be created as part of this workflow)

**Failure Modes:**
- Stakeholders have fundamentally different views → don't force alignment, treat divergence as a new hypothesis to test
- Research questions are too broad → each question must have a clear "what would a yes/no answer change about our direction?"

---

### Step 4 — Signal Synthesis & Value Prop Identification

**Action:** Primary research findings come in. Apply product sense to spot the critical differentiator, an ignored segment, and long-term monetization potential.

**Sub-steps:**
- Review primary research findings from the research team
- Apply product judgment: Is there a critical differentiator? An underserved or ignored segment? A monetization path over 3–5 years?
- Optionally use AI to help cluster or pattern-match findings (not always needed)
- Land on a clear value proposition angle worth pursuing

**Decision Points:**
- Is there a defensible value prop? If yes → proceed to Step 4b. If no → loop back to Step 3 with new angles
- Is the monetization path viable over the 3–5 year horizon?

**Data In:**
- Primary research outputs (interviews, surveys, research team synthesis)
- Your product sense + market intuition

**Data Out:**
- Clear value proposition statement: "PayPal can win in [segment] by [differentiation] and monetize via [model]"
- Supporting evidence from primary research

**Context Needs:**
- Primary research findings document
- Knowledge of PayPal's existing capabilities, business model, and constraints
- India fintech regulatory environment (RBI guidelines, UPI governance)

**Failure Modes:**
- Research findings are inconclusive → flag open questions, don't force a value prop
- Value prop sounds compelling but has no monetization path → kill or park for later

---

### Step 4b — Internal Feasibility Check

**Action:** Before packaging into a business case, pressure-test whether PayPal can actually execute on the value prop given their current tech, partnerships, and regulatory standing in India.

**Sub-steps:**
- Map PayPal's existing strengths relevant to this opportunity (cross-border payments, merchant network, brand trust)
- Identify capability gaps: what would PayPal need to build, acquire, or partner for?
- Make a build-vs-partner call for each gap
- Confirm the opportunity is achievable within the 3–5 year horizon

**Decision Points:**
- Is the gap closable within the 3–5 year horizon? If no → kill or significantly reshape the value prop
- Build vs. partner vs. acquire for each capability gap?

**Data In:**
- Value prop from Step 4
- PayPal Capabilities Brief (existing tech, partnerships, India presence)
- India regulatory context (what PayPal can/cannot do today)

**Data Out:**
- Feasibility summary: what PayPal can do now vs. what needs to be built/partnered
- Confidence level: High / Medium / Low on execution within 3–5 years

**Context Needs:**
- PayPal Capabilities Brief (from Context Shopping List)
- RBI / regulatory constraints for foreign payment companies in India

**Failure Modes:**
- Skipping this step and going straight to business case → finance will ask these questions and you won't have answers
- Overestimating PayPal's India capabilities → validate against actual regulatory standing, not global product features

---

### Step 5 — Business Case Input

**Action:** Package findings into a decision memo that becomes an input to a formal business case for leadership and finance.

**Sub-steps:**
- Write a structured decision memo: what you found → what it means → what you recommend
- Include: key findings, value proposition, target segment, monetization hypothesis, open risks/questions
- Share with leadership and finance teams
- Output: Go / No-Go / Investigate Further decision

**Decision Points:**
- Does the memo have enough to make a recommendation? (Even "here's what we still don't know" is valid)
- Leadership / Finance gate: pursue, pivot, or kill

**Data In:**
- Value prop from Step 4b
- All secondary and primary research artifacts

**Data Out:**
- Decision memo (input to business case)
- Leadership/finance decision

**Context Needs:**
- Business case template (if one exists at your company)
- Financial modeling assumptions (TAM, revenue model, cost structure)
- Precedents: how PayPal has entered other markets

**Failure Modes:**
- Memo is too long / unfocused → one page, three sections: findings, implications, recommendation
- Finance rejects without clear reasoning → ensure monetization hypothesis is explicit and time-bound (3–5 year horizon)

---

## Step Sequence and Dependencies

### Sequential Steps
1 → 2 → 3 → 4 → 4b → 5 (with loops)

### Parallel Steps
- Step 1: Track A (AI) and Track B (Human conversations) run simultaneously

### Loop Points
- Step 3 → Step 2: If stakeholders disagree, rework hypotheses and re-pressure-test
- Step 4 → Step 3: If no clear value prop emerges, return with new research angles
- Step 4b → Step 4: If PayPal can't execute on the value prop, reshape and retest

### Critical Path
Market Orientation → Hypothesis Pressure-Testing (+ Competitive Teardown + Regulatory Check) → Research Brief (+ Kill Criteria) → Primary Research → Value Prop → Internal Feasibility → Decision Memo

---

## Context Shopping List

| Artifact | Description | Used In | Status | Key Contents |
|---|---|---|---|---|
| eMarketer / Industry Reports | Secondary research on India fintech, UPI, digital payments | Steps 1, 2 | Needs Access | UPI volumes, PayPal India presence, competitor landscape |
| India Fintech Regulatory Context | RBI guidelines, UPI governance, foreign payment rules | Steps 2, 4 | Needs Creation | What PayPal can/cannot do in India |
| PayPal Capabilities Brief | What PayPal's existing product, tech, and business model can support | Steps 4, 5 | Needs Creation | Existing features, monetization models, past market entries |
| Research Brief Template | Structured template for tiered primary research questions | Step 3 | Needs Creation | Sections: Customer Segment, Problem Depth, Existing Alternatives, WTP, Market Signals |
| Decision Memo Template | One-page structure: findings → implications → recommendation | Step 5 | Needs Creation | Three sections, business case ready |
| PM / Market Expert Network | List of PMs with India fintech or payments experience | Step 1 | Exists (informal) | Names, context, availability |

---

*Workflow Definition generated: 2026-03-06*  
*Ready for Step 3 — Build*
