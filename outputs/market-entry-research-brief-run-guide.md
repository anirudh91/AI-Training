# Run Guide — Market Entry Research Brief

**Build Path:** Manual (you build artifacts from the spec)
**Platform:** Claude Code
**Generated:** 2026-03-15

---

## A. What to Build

| Artifact | Purpose | Format | Priority |
|---|---|---|---|
| `market-orientation-query` | Generate market landscape + 2–4 draft hypotheses | Skill (`SKILL.md`) | Phase 1 — First |
| `competitive-teardown` | Map competitors, moats, and undefended gaps | Skill (`SKILL.md`) | Phase 1 — Second |
| `regulatory-feasibility-check` | Surface legal blockers for market entry | Skill (`SKILL.md`) | Phase 2 |
| `hypothesis-pressure-tester` | Score hypotheses against secondary evidence | Skill (`SKILL.md`) | Phase 2 |
| `hypothesis-validation-agent` | Autonomously runs Skills 2, 3, 4 in sequence | Agent (`.md`) | Phase 2 — after skills tested |
| `research-brief-builder` | Structure validated hypotheses into tiered research questions | Skill (`SKILL.md`) | Phase 2 |
| `value-prop-synthesizer` | Cluster primary research into a value proposition | Skill (`SKILL.md`) | Phase 3 |
| `decision-memo-builder` | Package findings into a Go/No-Go leadership memo | Skill (`SKILL.md`) | Phase 3 |

---

## B. Build Sequence

### Setup — Directory Structure

Before building, confirm these directories exist:

```
~/.claude/
├── skills/
│   ├── market-orientation-query/
│   │   └── SKILL.md
│   ├── competitive-teardown/
│   │   └── SKILL.md
│   ├── regulatory-feasibility-check/
│   │   └── SKILL.md
│   ├── hypothesis-pressure-tester/
│   │   └── SKILL.md
│   ├── research-brief-builder/
│   │   └── SKILL.md
│   ├── value-prop-synthesizer/
│   │   └── SKILL.md
│   └── decision-memo-builder/
│       └── SKILL.md
└── agents/
    └── hypothesis-validation-agent.md
```

Skills in `~/.claude/skills/` are available globally across all projects.
The agent in `~/.claude/agents/` is also globally available.

---

### Phase 1 — Build First (Quick Wins)

#### Skill 1: `market-orientation-query`

**Create:** `~/.claude/skills/market-orientation-query/SKILL.md`

**Key content to include:**
- **Frontmatter:** `name: market-orientation-query` | `disable-model-invocation: true` | `argument-hint: "[market-name] [company-context]"` | `allowed-tools: WebSearch Write`
- **Inputs expected:** Market name, company/entrant context
- **Instructions to model:** Search for key players, market dynamics, known gaps. Frame output as unvalidated hypotheses. Surface unknowns explicitly. Flag conflicting signals — do not resolve.
- **Output:** `outputs/market-orientation-[market].md` — market picture + 2–4 draft hypotheses
- **Failure handling:** If hypotheses too vague, prompt for specificity before outputting. If market too broad, ask for sub-segment.

**Test in isolation:**
```
/market-orientation-query "India digital payments" "PayPal entering India, UPI is dominant"
```
Expected: A market overview + 2–4 clearly labeled draft hypotheses. Should NOT read as conclusions.

---

#### Skill 2: `competitive-teardown`

**Create:** `~/.claude/skills/competitive-teardown/SKILL.md`

**Key content to include:**
- **Frontmatter:** `name: competitive-teardown` | `disable-model-invocation: true` | `argument-hint: "[market] [competitors] [entrant-context]"` | `allowed-tools: WebSearch Write`
- **Inputs expected:** Market name/segment, named competitors (or ask to surface them), entrant context
- **Instructions to model:** Map each competitor's positioning, moat, and weakness. Identify what is genuinely undefended. Distinguish uncontested gaps from contested-but-winnable gaps. Do not assume a gap exists without checking who owns it.
- **Output:** `outputs/competitive-teardown-[market].md` — competitor map + gap analysis
- **Failure handling:** Insufficient data → flag as research needed, do not fabricate. All gaps appear owned → surface honestly, do not force an opportunity.

**Test in isolation:**
```
/competitive-teardown "India digital payments" "PhonePe, Google Pay, Paytm" "PayPal — cross-border strength, limited India domestic presence"
```
Expected: Competitor table with positioning/moat/weakness columns + explicit gap analysis.

---

### Phase 2 — Core Workflow

#### Skill 3: `regulatory-feasibility-check`

**Create:** `~/.claude/skills/regulatory-feasibility-check/SKILL.md`

**Key content to include:**
- **Frontmatter:** `name: regulatory-feasibility-check` | `disable-model-invocation: true` | `argument-hint: "[country] [company] [operation-type]"` | `allowed-tools: WebSearch Write`
- **Inputs expected:** Market/country, company name, nature of operation, known constraints
- **Instructions to model:** Surface what the company can legally do today. List known blockers. Flag areas needing legal expert verification. Assign confidence level (High / Medium / Low). Distinguish current rules from pending/proposed changes.
- **Output:** `outputs/regulatory-check-[market].md` — regulatory status + blockers + open questions
- **Failure handling:** Too complex for secondary sources → flag, recommend expert. Rules may have changed → flag recency risk.

**Test in isolation:**
```
/regulatory-feasibility-check "India" "PayPal" "foreign payment processor, domestic retail payments — PayPal shut down India domestic payments in 2021"
```
Expected: Regulatory status table, blockers list, confidence level, open questions for legal review.

---

#### Skill 4: `hypothesis-pressure-tester`

**Create:** `~/.claude/skills/hypothesis-pressure-tester/SKILL.md`

**Key content to include:**
- **Frontmatter:** `name: hypothesis-pressure-tester` | `disable-model-invocation: true` | `argument-hint: "[hypotheses-list] [secondary-research-sources]"` | `allowed-tools: WebSearch Write`
- **Inputs expected:** List of 2–4 hypotheses, secondary research sources or pasted findings
- **Instructions to model:** Score each hypothesis: Confirmed / Weakened / Killed — with evidence cited. "Weakened" = partially supported, keep with caveats. "Killed" requires positive counter-evidence, not just absence of evidence. Flag what secondary research could not answer.
- **Output:** `outputs/hypothesis-scored-[market].md` — scored table + 1–3 surviving hypotheses + open questions
- **Failure handling:** No secondary data → treat as high-uncertainty, flag for primary research. Unverified AI-generated data → prompt user to verify before scoring.

**Test in isolation:**
```
/hypothesis-pressure-tester "H1: SMBs doing cross-border payments are underserved by UPI. H2: Merchants want multi-currency settlement. H3: Regulatory environment will ease for foreign players." "eMarketer India fintech 2025 report — [paste key excerpts]"
```
Expected: Scored table with Confirmed/Weakened/Killed for each, evidence cited, open questions flagged.

---

#### Agent: `hypothesis-validation-agent`

**Only build after Skills 2, 3, 4 are individually tested.**

**Create:** `~/.claude/agents/hypothesis-validation-agent.md`

**Key content to include:**
- **Frontmatter:**
  - `name: hypothesis-validation-agent`
  - `model: opus` (claude-opus-4-6 — required for multi-source synthesis)
  - `tools: WebSearch, WebFetch, Read`
  - `skills: competitive-teardown, regulatory-feasibility-check, hypothesis-pressure-tester`
  - `description:` Must start with "Use this agent when..." + include 2–3 `<example>` blocks
- **Agent body:** Role statement as Market Entry Hypothesis Analyst. Process: (1) run competitive teardown per hypothesis, kill immediately if moat is strong; (2) run regulatory check on survivors, kill if fatal blocker; (3) run pressure-tester across all survivors; (4) return one consolidated output — no intermediate results surfaced.
- **Constraints:** Never fabricate. Surface conflicting signals. Flag recency risk on regulatory findings. One consolidated output only.

**To load after creating the file:** Run `/agents` in Claude Code to refresh, or restart the session.

**Test:**
Invoke by describing what you want — Claude will launch the agent:
```
"Run hypothesis validation on these three hypotheses about PayPal entering India: [paste H1, H2, H3 from market orientation output]"
```
Expected: One consolidated scored shortlist — not three separate step outputs.

---

#### Skill 5: `research-brief-builder`

**Create:** `~/.claude/skills/research-brief-builder/SKILL.md`

**Key content to include:**
- **Frontmatter:** `name: research-brief-builder` | `disable-model-invocation: true` | `allowed-tools: Read Write`
- **Inputs expected:** Surviving hypotheses (scored list from Step 2), stakeholder pushback notes, kill criteria (2–3 conditions that would stop the opportunity)
- **Instructions to model:** Build tiered research questions — Tier 1 (must answer) and Tier 2 (good to have). Frame every question as "Does [segment] experience [problem]?" not open-ended. Every question must have a directional implication. Kill criteria must be specific and time-bound.
- **Output:** `outputs/research-brief-[market].md` — tiered question document with kill criteria
- **Failure handling:** Questions too broad → rewrite to directional format. No kill criteria provided → prompt user to define at least 2 before outputting.

**Test:**
```
/research-brief-builder
```
Then paste: surviving hypotheses + any stakeholder pushback notes + your kill criteria.

---

### Phase 3 — Downstream Synthesis

#### Skill 6: `value-prop-synthesizer`

**Create:** `~/.claude/skills/value-prop-synthesizer/SKILL.md`

**Key content to include:**
- **Frontmatter:** `name: value-prop-synthesizer` | `disable-model-invocation: true` | `allowed-tools: Read Write`
- **Inputs expected:** Primary research findings doc, surviving hypotheses from Step 2, company capabilities context
- **Instructions to model:** Identify critical differentiator, underserved segment, and monetization path separately before synthesizing. Output: "[Company] can win in [segment] by [differentiation] and monetize via [model]." Flag if any of the three elements is missing — do not force a value prop. If findings are inconclusive, output open questions only.
- **Output:** `outputs/value-prop-[market].md` — value prop statement + supporting evidence + open risks + monetization path (Yes / Uncertain / No)
- **Failure handling:** Inconclusive findings → open questions, not fabricated value prop. No monetization path → "value prop exists but monetization path is unclear — park or kill."

---

#### Skill 7: `decision-memo-builder`

**Create:** `~/.claude/skills/decision-memo-builder/SKILL.md`

**Key content to include:**
- **Frontmatter:** `name: decision-memo-builder` | `disable-model-invocation: true` | `allowed-tools: Read Write`
- **Inputs expected:** Value prop statement, feasibility summary, key findings, open risks
- **Instructions to model:** Produce a one-page memo, three sections only: (1) What we found — key findings, value prop, target segment; (2) What it means — implications, monetization hypothesis, confidence level; (3) What we recommend — Go / No-Go / Investigate Further with rationale and open risks. Monetization hypothesis must be explicit and time-bound (3–5 year horizon). Open risks listed, not buried.
- **Output:** `outputs/decision-memo-[market].md`
- **Failure handling:** Memo too long → enforce one-page / three sections. No monetization hypothesis → prompt user before generating.

---

## C. First Run — Guided Test

Use **India digital payments (PayPal)** as your test case — this matches the scenario in your spec.

### Step 1 — Market Orientation

```
/market-orientation-query "India digital payments" "PayPal entering India — UPI is dominant, PayPal shut down domestic payments in 2021"
```

**What good output looks like:**
- 3–5 paragraphs covering UPI dominance, key players (PhonePe, Google Pay, Paytm), merchant pain points, cross-border flow data
- 2–4 clearly labeled draft hypotheses (e.g., "H1: SMBs doing cross-border payments are underserved")
- Unknown areas explicitly called out

**Common issues:**
- Output reads as conclusions, not hypotheses → add "Frame all findings as hypotheses, not conclusions" to the skill body
- Too broad → add failure mode instruction to ask for sub-segment

---

### Step 2 — Hypothesis Validation (Agent)

Take the 2–4 hypotheses from Step 1 output and say:

```
"Run hypothesis validation on these PayPal India hypotheses: [paste list]"
```

Claude will launch `hypothesis-validation-agent` automatically.

**What good output looks like:**
- One consolidated table: Hypothesis | Score (Confirmed/Weakened/Killed) | Evidence | Open Questions
- 1–3 surviving hypotheses with caveats where "Weakened"
- Flags for anything needing primary research

**Common issues:**
- Agent surfaces intermediate step outputs → tighten "return one consolidated output only" constraint in agent body
- Skills not loading in agent context → confirm skills are listed in `skills:` frontmatter of the agent file

---

### Steps 3–5 — Brief → Value Prop → Memo

Run sequentially as the workflow progresses. Each skill takes the output of the previous step as input. For eMarketer data (flagged as manual): paste relevant excerpts directly into the skill invocation as additional context.

---

## D. What to Do Next

### Repeatable trigger

Each new market entry investigation starts with:
```
/market-orientation-query "[market name]" "[company context]"
```
Then follow the step sequence through to `/decision-memo-builder`.

### Before Step 2 — create two context documents

These are needed before the agent can run effectively:
1. **India Fintech Regulatory Context** — RBI guidelines, UPI governance, foreign payment rules. Create as a markdown file; paste or reference when invoking the regulatory check skill.
2. **PayPal Capabilities Brief** — Existing features, monetization models, past market entries. Needed for Steps 4, 4b, 5.

### eMarketer access

eMarketer does not have an MCP connector for Claude Code. Workaround: open eMarketer in browser, copy relevant data, paste as input context when invoking Steps 1–2 skills.

### When to update skills

Revisit and revise a skill if:
- Output is consistently too vague or too broad
- A new market has different regulatory or competitive dynamics not handled by the current instructions
- Primary research findings reveal patterns the skills are missing

### Sharing with teammates

Skills in `~/.claude/skills/` are personal (global to your Claude Code). To share with teammates:
- Move skills to a project-level `.claude/skills/` directory inside a shared repo
- Teammates clone the repo and skills are available when they open Claude Code in that project directory
- Agent files follow the same pattern: project-level `.claude/agents/` for shared agents

---

## Artifact Inventory

| Artifact | Location | Status |
|---|---|---|
| Building Block Spec | `outputs/market-entry-research-brief-building-block-spec.md` | Done (from Design phase) |
| Construction Guide | Delivered in conversation (Construct phase) | Done |
| Run Guide | `outputs/market-entry-research-brief-run-guide.md` | This document |
| Skills (7) | `~/.claude/skills/[skill-name]/SKILL.md` | To build |
| Agent (1) | `~/.claude/agents/hypothesis-validation-agent.md` | To build |

---

*Run Guide generated: 2026-03-15*
*Next: Build Phase 1 skills, test in isolation, then proceed to Phase 2.*
