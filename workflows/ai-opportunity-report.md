# AI Opportunity Report

| | |
|---|---|
| **Name** | Anirudh |
| **Role** | Product Manager |
| **Date** | 2026-03-06 |
| **Lens** | Individual |
| **Opportunities identified** | 4 |
| **Top recommendation** | Structured Research Brief Generator — directly eliminates both pain points (knowing what to ask + making sense of outputs) in one workflow |

---

## Summary Table

| # | Opportunity | Autonomy | Involvement | Impact |
|---|---|---|---|---|
| 1 | Structured Research Brief Generator | Guided | Augmented | High |
| 2 | Customer Problem Synthesis Engine | Autonomous | Automated | High |
| 3 | Research-to-Decision Translator | Guided | Augmented | High |
| 4 | Competitive Landscape Mapper | Deterministic | Automated | Medium |

---

## Top Recommendations

1. **Structured Research Brief Generator** — Eliminates the "I don't know what to research" problem by giving you a structured starting brief before you open a single chat window. Highest immediate impact.
2. **Customer Problem Synthesis Engine** — Converts your scattered AI chat outputs into a clean, structured problem map. Directly fixes the "can't make head or tail of it" frustration.
3. **Research-to-Decision Translator** — Bridges the gap between raw research and actual product decisions, so your choices are documented and defensible.

---

## Detailed Opportunity Cards

### 🔵 Deterministic

---

**[4] Competitive Landscape Mapper**

**Autonomy:** Deterministic
**Involvement:** Automated

**Why it's a good candidate:**
Competitive research follows a predictable pattern — same questions, same structure, every time. It's templatable and repeatable, making it ideal for a fixed AI workflow.

**Current pain point:**
You're asking ad-hoc questions across chat sessions with no consistent structure. Each new market means starting from scratch, and the outputs don't compare cleanly across competitors.

**How AI helps:**
You provide a market name and product category. AI runs through a fixed competitive research template — players, positioning, pricing, key features, gaps — and returns a structured comparison table ready to use.

**Getting started:**
Write a single reusable prompt: *"You are a product research assistant. Given a market [X] and product category [Y], produce a competitive landscape covering: top 5 players, their positioning, pricing model, key features, and market gaps."* Save it. Run it every time you enter a new market.

---

### 🟡 Guided

---

**[1] Structured Research Brief Generator**

**Autonomy:** Guided
**Involvement:** Augmented

**Why it's a good candidate:**
The hardest part of 0-to-1 research isn't finding answers — it's knowing the right questions to ask. This is a language and reasoning task that AI is exceptionally good at. It's also highly repeatable across every new market you enter.

**Current pain point:**
You go into research without a clear framework, which means you ask whatever comes to mind, get answers that don't connect to each other, and end up with a pile of responses that's hard to synthesize.

**How AI helps:**
You describe the new market you're entering and the product hypothesis. Claude generates a structured research brief: a tiered question bank covering customer segments, problem depth, existing alternatives, willingness to pay, and market signals. You use this brief as your research agenda — not a blank chat window.

**Getting started:**
Try this prompt today: *"I'm a PM exploring a 0-to-1 product in [market]. Help me build a structured research agenda — group questions by: (1) Who is the customer, (2) What problems do they have, (3) How do they solve it today, (4) What would make them switch."*

---

**[3] Research-to-Decision Translator**

**Autonomy:** Guided
**Involvement:** Augmented

**Why it's a good candidate:**
PMs often struggle to show their reasoning — you know what you decided, but the trail from research to decision is invisible. This workflow makes that link explicit and shareable.

**Current pain point:**
After all your research, the jump to "what do we build" is still done in your head. There's no structured artifact that shows: here's what I learned → here's what it means → here's what I recommend. That makes decisions hard to defend to stakeholders.

**How AI helps:**
You paste your collated research notes (even messy ones). Claude extracts the key signals, identifies patterns, and produces a decision memo: key findings, what they imply, recommended product direction, and open questions. You review and edit — AI does the heavy structuring.

**Getting started:**
After your next research session, paste all your notes into Claude with this prompt: *"Here are my raw research notes. Identify the top 3 customer problems, what they imply about product direction, and what's still unclear. Format as a decision brief."*

---

### 🔴 Autonomous

---

**[2] Customer Problem Synthesis Engine**

**Autonomy:** Autonomous
**Involvement:** Automated

**Why it's a good candidate:**
Synthesizing unstructured qualitative data into a structured problem map is exactly what AI excels at — it's pattern recognition across language, done at scale. You're currently doing this manually and it's costing you hours.

**Current pain point:**
You run multiple chat sessions, collect responses, and then face a wall of text with no clear structure. Summarizing it, spotting patterns, and identifying the most important problems requires significant mental effort and time.

**How AI helps:**
You dump all your raw research (chat outputs, notes, quotes) into a single Claude session. Claude autonomously clusters the content by theme, scores problems by frequency and severity, identifies contradictions, and outputs a structured Customer Problem Map with ranked insights. This becomes the foundation for your PRD.

**Getting started:**
At the end of your next research sprint, paste everything into Claude Code with this prompt: *"Here is all my customer research. Cluster the insights by problem theme, rank them by how frequently and severely they appear, flag any contradictions, and produce a structured problem map I can use for product decisions."*

---

## Workflow Candidate Summary

*(To be appended after candidate selection)*

---

## Appendix: Classification Definitions

**Autonomy — How much decision-making does AI have?**
- **Deterministic**: Fixed rules, no judgment. Same input → same output every time.
- **Guided**: AI makes bounded decisions within guardrails you set. Human steers direction.
- **Autonomous**: AI plans, decides, and adapts independently. Executes end-to-end.

**Human Involvement — Is a human in the loop?**
- **Augmented**: You participate during the workflow — review, steer, or decide at key points.
- **Automated**: AI runs solo end-to-end. You review only the final output.
