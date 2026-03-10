# AI Building Block Spec: Market Entry Research Brief

**Generated:** 2026-03-10 (revised 2026-03-10 — added Hypothesis Validation Agent)
**Workflow Definition Source:** `market-entry-research-brief-definition.md`

---

## Scenario Summary

| Field | Value |
|-------|-------|
| **Workflow Name** | Market Entry Research Brief |
| **Lens** | Individual |
| **Owner** | Anirudh (Product Manager) |
| **Trigger** | PM identifies a new market to enter with zero prior context |
| **Outcome** | Layered research brief: secondary findings + tiered primary research questions, ready to guide interviews, research sprints, and stakeholder alignment |
| **Business Objective** | Identify whether PayPal (or similar entrant) has a viable value proposition and monetization path in a new market over a 3–5 year horizon |
| **Platform** | Claude Code |

---

## Autonomy Level Assessment

**Workflow-level autonomy: Guided**

The sequence is mostly fixed (1→2→3→4→4b→5) with bounded loops at Steps 3, 4, and 4b. AI provides bounded judgment at specific steps — hypothesis scoring, competitive teardown, regulatory checks, value prop clustering — but the PM makes every go/no-go decision and owns all loop-back calls. The AI does not autonomously decide to backtrack; it provides analysis and the human steers. This places the workflow squarely in Guided territory.

```
Deterministic ———————— [Guided] ———————— Autonomous
```

---

## Orchestration Mechanism

**Mechanism: Skill-Powered Prompt + embedded sub-agent (hybrid)**
**Involvement Mode: Augmented**

The PM is the workflow orchestrator — they invoke skills in sequence, make decisions at each human gate, and own all loop-back calls. The workflow-level mechanism is Skill-Powered Prompt because all loops (Steps 3, 4, 4b) are triggered by human judgment, not AI sequencing.

**Exception: Step 2 uses an embedded sub-agent.** The Hypothesis Validation Agent autonomously sequences the three Step 2 skills (Competitive Teardown → Regulatory Feasibility Check → Hypothesis Pressure-Tester) and returns a single scored output. The PM invokes the agent once and reviews the consolidated result — they do not drive each skill call manually. The human gate at Step 2 remains: PM still decides which hypotheses survive before proceeding to Step 3.

Augmented involvement is determined by the manual trigger and Claude Code desktop deployment.

---

## Architecture Decisions

| Decision | Value | Rationale / Constraints |
|----------|-------|--------------------------|
| **Platform** | Claude Code | Code-capable; supports skills as markdown files, full model access, file read/write |
| **Involvement Mode** | Augmented | Manual trigger; PM drives and reviews at each gate |
| **Orchestration** | Skill-Powered Prompt + sub-agent (hybrid) | 7 skills + 1 agent; PM orchestrates workflow; agent orchestrates Step 2 internally |
| **Autonomy Cap** | Guided (workflow) / Autonomous (Step 2 internally) | All loop-back decisions are human-owned; Step 2 agent sequences autonomously within bounded scope |
| **Tools** | Web search, file read, file write | Secondary research requires web access; output artifacts require file write |
| **Browser Access** | TBD at Construct | eMarketer requires authenticated access — to be resolved during Construct |

---

## Step-by-Step Decomposition

| Step | Name | Autonomy | Building Blocks | Skill Candidate | Human Gate |
|------|------|----------|-----------------|-----------------|------------|
| 1 | Market Orientation | Guided (Track A) / Human (Track B) | Skill + Context | Market Orientation Query | Enough signals to form hypotheses? |
| 2 | Hypothesis Pressure-Testing | **Autonomous** (internally) | **Agent** + Skills + Context | Hypothesis Validation Agent (orchestrates: Competitive Teardown, Regulatory Check, Pressure-Tester) | PM reviews scored output, selects surviving hypotheses |
| 3 | Stakeholder Pressure-Test & Research Agenda | Human + Guided | Skill + Context | Research Brief Builder | Stakeholder alignment + research questions sharp enough |
| 4 | Signal Synthesis & Value Prop | Guided | Skill + Context | Value Prop Synthesizer | Defensible value prop identified? |
| 4b | Internal Feasibility Check | Guided | Skill + Context | Feasibility Checker | Gap closable in 3–5 years? |
| 5 | Business Case Input | Guided | Skill + Context | Decision Memo Builder | Leadership/Finance Go/No-Go |

### Autonomy Spectrum by Step

```
Step 1 Track B  → Human
Step 1 Track A  → Guided      [Market Orientation Query]
Step 2          → Autonomous  [Hypothesis Validation Agent → Competitive Teardown + Regulatory Check + Pressure-Tester]
                              human gate: PM reviews scored output, selects survivors
Step 3          → Guided      [Research Brief Builder] — human gate: stakeholder sync
Step 4          → Guided   [Value Prop Synthesizer] — human gate: value prop call
Step 4b         → Guided   [Feasibility Checker]    — human gate: build/partner/kill
Step 5          → Guided   [Decision Memo Builder]  — human gate: leadership Go/No-Go
```

---

## Skill Candidates

### Skill 1 — Market Orientation Query

**Purpose:** Generate a high-level market landscape overview and surface 2–4 early hypotheses about gaps or opportunities.

**Inputs:**
- Market name (e.g., "India digital payments")
- Product/company context (e.g., "PayPal entering India, UPI is dominant")

**Outputs:**
- Market picture: key players, dynamics, known gaps
- 2–4 draft hypotheses (unvalidated, labeled as such)

**Decision Logic:**
- Frame the output as hypotheses, not conclusions
- Surface what is unknown as explicitly as what is known
- Flag conflicting signals rather than resolving them

**Failure Modes:**
- Hypotheses too vague → skill should prompt for more specificity before outputting
- Market name too broad → ask for a sub-segment or use case to narrow scope

---

### Skill 2 — Competitive Teardown

**Purpose:** Map existing players in the target market — their positioning, moat, and what is genuinely undefended — to pressure-test whether a gap actually exists.

**Inputs:**
- Market name and segment
- Named competitors (or prompt to surface them)
- Entrant context (e.g., "PayPal — cross-border payments strength, limited India domestic presence")

**Outputs:**
- Competitor map: player, positioning, moat, weakness
- Gap analysis: what is undefended and why
- Flag: any competitor already occupying the hypothesized gap

**Decision Logic:**
- Do not assume a gap exists without checking who already owns it
- Distinguish between gaps that are uncontested vs. gaps that are contested but winnable

**Failure Modes:**
- Insufficient public data on competitors → flag as research needed, do not fabricate
- All gaps appear owned → surface this honestly; do not force an opportunity

---

### Skill 3 — Regulatory Feasibility Check

**Purpose:** Surface regulatory blockers for a foreign payment company entering a specific market — early, before investing in strategy.

**Inputs:**
- Market / country name
- Company name and nature of operation (e.g., "PayPal — foreign payment processor, domestic retail payments")
- Known constraints (e.g., "RBI data localisation rules, PayPal shut down India domestic payments in 2021")

**Outputs:**
- Regulatory status: what the company can legally do today
- Known blockers: rules that would prevent entry or require approval
- Open questions: areas requiring legal/regulatory expert verification
- Confidence level: High / Medium / Low (based on available public information)

**Decision Logic:**
- Always surface regulatory blockers before strategy discussion
- Distinguish between current rules and pending/proposed changes
- Flag when legal expert verification is required

**Failure Modes:**
- Regulatory landscape too complex for secondary sources → flag explicitly, recommend expert consultation
- Rules have changed since training cutoff → flag recency risk, recommend verification

---

### Skill 4 — Hypothesis Pressure-Tester

**Purpose:** Score each hypothesis against secondary data and return a shortlist of validated (or at least data-backed) hypotheses.

**Inputs:**
- List of 2–4 hypotheses (from Step 1)
- Secondary research sources or findings (eMarketer, news, reports — pasted or referenced)

**Outputs:**
- Scored hypothesis list: Confirmed / Weakened / Killed — with evidence
- Shortlist of 1–3 surviving hypotheses
- Open questions: what secondary research could not answer (flagged for primary research)

**Decision Logic:**
- Score based on evidence, not intuition
- "Weakened" means partially supported — keep with caveats, not kill
- "Killed" requires positive evidence against, not just absence of evidence

**Failure Modes:**
- No secondary data exists for a hypothesis → treat as high-uncertainty, flag for primary research, do not kill without evidence
- AI-generated data without source verification → prompt user to verify against original sources before scoring

---

### Skill 5 — Research Brief Builder

**Purpose:** Structure validated hypotheses into a tiered primary research question document with kill criteria — sharp enough to hand to a research team.

**Inputs:**
- Surviving hypotheses (scored list from Step 2)
- Stakeholder pushback notes (from Step 3 conversations)
- Kill criteria inputs: 2–3 conditions that would stop the opportunity

**Outputs:**
- Research brief document with:
  - Tiered primary research questions (Tier 1: must answer; Tier 2: good to have)
  - Each question framed as: "Does [segment] experience [problem]?" (not "tell me about X")
  - Kill criteria: explicit conditions that would end the investigation
  - Focus areas: value proposition, customer segments, monetization potential

**Decision Logic:**
- Every question must have a clear "what would a yes/no answer change about our direction?"
- Questions that don't have a directional implication should be removed
- Kill criteria must be specific and time-bound (e.g., "regulatory approval takes 3+ years")

**Failure Modes:**
- Questions too broad → skill flags and rewrites to specific, directional format
- No kill criteria provided → skill prompts user to define at least 2 before outputting

---

### Skill 6 — Value Prop Synthesizer

**Purpose:** Cluster primary research findings into a clear value proposition statement with supporting evidence.

**Inputs:**
- Primary research findings document (from research team)
- Surviving hypotheses from Step 2
- Company capabilities context (PayPal Capabilities Brief)

**Outputs:**
- Value proposition statement: "[Company] can win in [segment] by [differentiation] and monetize via [model]"
- Supporting evidence: findings that back the value prop
- Open risks: what primary research left unanswered
- Monetization path: viable over 3–5 year horizon? Yes / Uncertain / No

**Decision Logic:**
- Surface the critical differentiator, underserved segment, and monetization path separately before synthesizing
- Flag if any of the three elements is missing — do not force a value prop
- If findings are inconclusive, output "open questions" rather than a fabricated value prop

**Failure Modes:**
- Research findings are inconclusive → flag open questions, do not force a value prop
- Value prop has no monetization path → output clearly: "value prop exists but monetization path is unclear — park or kill"

---

### Skill 7 — Decision Memo Builder

**Purpose:** Package all research findings into a one-page Go/No-Go decision memo ready for leadership and finance.

**Inputs:**
- Value proposition statement (from Step 4)
- Feasibility summary (from Step 4b)
- Key findings from secondary and primary research
- Open risks and questions

**Outputs:**
- Decision memo (one page, three sections):
  1. **What we found** — key findings, value prop, target segment
  2. **What it means** — implications, monetization hypothesis, confidence level
  3. **What we recommend** — Go / No-Go / Investigate Further, with rationale and open risks
- Financial modeling assumptions (TAM, revenue model, cost structure) — flagged if not available

**Decision Logic:**
- Even "here's what we still don't know" is a valid recommendation
- Monetization hypothesis must be explicit and time-bound (3–5 year horizon)
- Open risks should be listed, not buried

**Failure Modes:**
- Memo too long → enforce one-page discipline, three sections only
- No monetization hypothesis → prompt user to provide before generating memo

---

## Agent Configuration

### Hypothesis Validation Agent

| Component | Specification |
|-----------|--------------|
| **Name** | Hypothesis Validation Agent |
| **Description** | Autonomously validates market entry hypotheses by running competitive analysis, regulatory feasibility check, and hypothesis scoring in sequence. Invoked once by the PM with a hypotheses list; returns a single scored shortlist. |
| **Model** | `claude-opus-4-6` — reasoning-heavy; required for multi-source synthesis across competitive, regulatory, and market data in a single run |
| **Tools** | Web search, Competitive Teardown skill, Regulatory Feasibility Check skill, Hypothesis Pressure-Tester skill |
| **Invocation** | PM provides: market name, company context, list of 2–4 hypotheses → agent runs autonomously → PM reviews scored output |

**Instructions:**

```
Mission: Validate market entry hypotheses autonomously. You receive a list of 2–4 unvalidated
hypotheses about a market opportunity. Your job is to pressure-test each one and return a scored
shortlist — without requiring the PM to drive each step.

Responsibilities:
1. For each hypothesis, run Competitive Teardown to check whether the gap is genuinely undefended.
   If competitive teardown reveals a competitor already owns the gap with a strong moat, mark the
   hypothesis Killed immediately — do not continue to regulatory check for that hypothesis.
2. For hypotheses that survive competitive analysis, run Regulatory Feasibility Check to surface
   legal blockers. If a blocker is fatal (e.g., legal prohibition), mark the hypothesis Killed.
3. Run Hypothesis Pressure-Tester across all surviving hypotheses using evidence from Steps 1 and 2.
   Score each: Confirmed / Weakened / Killed — with evidence cited.
4. Synthesize into a final scored shortlist with: score, evidence summary, open questions flagged
   for primary research.

Behavior:
- Never fabricate data. If public sources are insufficient, flag as "high uncertainty" — do not kill
  without evidence.
- If web search returns conflicting signals, surface both sides — do not resolve arbitrarily.
- Flag recency risk on regulatory findings: rules may have changed; recommend verification.
- Return one consolidated output. Do not surface intermediate step outputs to the PM.

Output format:
- Scored hypothesis table: Hypothesis | Score | Evidence | Open Questions
- Shortlist: 1–3 surviving hypotheses (Confirmed or Weakened with caveats)
- Flags: anything requiring primary research or expert verification
```

**Context requirements:**
- Market name and company context (provided by PM at invocation)
- India Fintech Regulatory Context document (if available — read from project context)
- Web search access for live secondary research

**Goal / Trigger:**
PM invokes after Step 1 completes with 2–4 draft hypotheses. Agent runs end-to-end and surfaces scored output for PM review. PM then decides which hypotheses survive before Step 3.

**Human review gate:**
PM reviews the scored shortlist and makes the final call on which hypotheses proceed. The agent does not determine what survives — it provides the evidence; the PM decides.

---

## Step Sequence and Dependencies

```
Step 1 (Market Orientation)
  ├── Track A: Market Orientation Query skill [AI]
  └── Track B: PM conversations with peer PMs [Human — parallel]
        ↓
Step 2 (Hypothesis Pressure-Testing)
  └── Hypothesis Validation Agent [autonomous]
        ├── Competitive Teardown skill
        ├── Regulatory Feasibility Check skill
        └── Hypothesis Pressure-Tester skill
  [Human gate: PM reviews scored shortlist, selects survivors]
        ↓
Step 3 (Stakeholder Pressure-Test & Research Agenda)
  ├── [Human gate: stakeholder conversations]
  ├── Loop back to Step 2 if disagreement ↑
  └── Research Brief Builder skill (on alignment)
        ↓
Step 4 (Signal Synthesis & Value Prop)
  ├── [Primary research received from research team]
  ├── Value Prop Synthesizer skill
  ├── Loop back to Step 3 if no clear value prop ↑
        ↓
Step 4b (Internal Feasibility Check)
  ├── Feasibility Checker skill
  ├── Loop back to Step 4 if value prop needs reshaping ↑
        ↓
Step 5 (Business Case Input)
  └── Decision Memo Builder skill
        ↓
  [Leadership/Finance: Go / No-Go / Investigate Further]
```

---

## Prerequisites

- Claude Code installed and running
- Web search access (for secondary research in Steps 1–2)
- eMarketer or equivalent research platform access (authenticated — resolve at Construct)
- PayPal Capabilities Brief (to be created before Step 4b)
- India Fintech Regulatory Context document (to be created before Step 2)
- Research Brief Template (created by Research Brief Builder skill at Step 3)
- Decision Memo Template (created by Decision Memo Builder skill at Step 5)

---

## Context Inventory

| Artifact | Used In | Status | Notes |
|----------|---------|--------|-------|
| Market name + product context | Step 1 | PM provides at runtime | e.g., "PayPal entering India, UPI dominant" |
| eMarketer / Industry Reports | Steps 1, 2 | Needs access | Authenticated login — flag for Construct |
| India Fintech Regulatory Context | Steps 2, 4 | Needs creation | RBI guidelines, UPI governance, foreign payment rules |
| PayPal Capabilities Brief | Steps 4, 4b, 5 | Needs creation | Existing features, monetization models, past market entries |
| PM / Market Expert Network | Step 1 | Exists (informal) | Names, context, availability |
| Primary Research Findings | Steps 4, 4b | Produced during workflow | From research team post-Step 3 |
| Research Brief Template | Step 3 | Created by Skill 5 | Output of Research Brief Builder |
| Decision Memo Template | Step 5 | Created by Skill 7 | Output of Decision Memo Builder |

---

## Tools and Connectors Required

- Web search (secondary research — Steps 1, 2)
- File read (PayPal Capabilities Brief, primary research documents — Steps 4, 4b, 5)
- File write (research brief output, decision memo — Steps 3, 5)
- eMarketer or equivalent (authenticated — Steps 1, 2)

---

## Integration Research Needed

| Tool | Used For | Steps | Research Needed |
|------|----------|-------|-----------------|
| eMarketer | Secondary research on India fintech, UPI, digital payments | 1, 2 | Does Claude Code support authenticated browser access or MCP connector for eMarketer? |
| Web search | General secondary research, news, competitor data | 1, 2 | Verify web search MCP availability in Claude Code |
| File read/write | Reading capabilities brief, writing brief/memo outputs | 3, 4, 4b, 5 | Native to Claude Code — confirm file path conventions |

---

## Model Recommendation

| Steps | Recommended Model Class | Model ID | Rationale |
|-------|------------------------|----------|-----------|
| 1, 2 (market research, competitive teardown, regulatory check) | Reasoning-heavy | `claude-opus-4-6` | Complex multi-source synthesis, hypothesis generation, regulatory judgment |
| 3, 5 (brief structuring, memo drafting) | Fast | `claude-haiku-4-5-20251001` | Structured output generation from well-defined inputs |
| 4, 4b (value prop synthesis, feasibility assessment) | Balanced | `claude-sonnet-4-6` | Requires product judgment and multi-step reasoning, but not the full depth of Opus |

---

## Recommended Implementation Order

### Phase 1 — Quick Wins (implement first)
1. **Market Orientation Query** — highest immediate value; first skill the PM uses on every new market
2. **Competitive Teardown** — used in Step 2; standalone utility even outside this workflow

### Phase 2 — Core Workflow
3. **Regulatory Feasibility Check** — Step 2 skill; critical for market entry, often missed
4. **Hypothesis Pressure-Tester** — Step 2 skill; ties the first two skills into a scored output
5. **Hypothesis Validation Agent** — Step 2 agent; wire the three Step 2 skills together into autonomous execution; build after all three skills are tested individually
6. **Research Brief Builder** — Step 3; the primary deliverable of the workflow

### Phase 3 — Downstream Synthesis
7. **Value Prop Synthesizer** — Step 4; depends on primary research completing
8. **Decision Memo Builder** — Step 5; final output, depends on all prior steps

---

## Where to Run

**Claude Code** — skills implemented as markdown files (`SKILL.md` in `~/.claude/skills/[skill-name]/`). PM invokes each skill manually from the Claude Code interface in sequence, driving the workflow as the human orchestrator. Outputs written as markdown files to a project `outputs/` directory.

---

*AI Building Block Spec generated: 2026-03-10 | revised 2026-03-10 — added Hypothesis Validation Agent*
*Ready for Construct phase: `/business-first-ai:constructing-workflows`*
