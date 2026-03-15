---
name: market-orientation-query
description: >
  This skill should be used when the user wants to generate a high-level market landscape overview
  and surface early hypotheses about gaps or opportunities before entering a new market.
  Takes a market name and company context, searches secondary sources, and produces a structured
  market picture with 2–4 clearly labeled draft hypotheses ready for validation.
disable-model-invocation: true
argument-hint: "[market-name] [company-context]"
allowed-tools: WebSearch Write
---

# Market Orientation Query

Generate a high-level market landscape overview and surface 2–4 early hypotheses about gaps or opportunities in a new market.

## Workflow

1. **Parse inputs** — Extract the market name and company/entrant context from `$ARGUMENTS`. If either is missing, ask the user before proceeding:
   - Market name (e.g., "India digital payments")
   - Company/entrant context (e.g., "PayPal entering India, UPI is dominant, domestic payments shut down in 2021")

2. **Run secondary research** — Use WebSearch to investigate the market. Search for:
   - Key players and their market share
   - Dominant platforms, rails, or infrastructure
   - Known pain points for merchants, consumers, or specific segments
   - Recent regulatory changes or news
   - Any recent market entry attempts by similar companies
   Search at least 3–4 distinct queries to triangulate across sources.

3. **Build the market picture** — Synthesize findings into a structured overview covering:
   - Who the dominant players are and what they own
   - What dynamics are driving the market (growth, consolidation, regulation)
   - Where the known gaps or underserved segments are
   - What is unknown or contested (flag explicitly — do not resolve)

4. **Generate draft hypotheses** — Produce 2–4 hypotheses based on the market picture. Each hypothesis must:
   - Be framed as a testable statement: "We believe [segment] experiences [problem] because [signal]"
   - Be labeled as UNVALIDATED
   - Identify what evidence would confirm or kill it
   - Not be presented as a conclusion

5. **Flag conflicting signals** — If sources disagree on key facts (e.g., market size estimates, regulatory status), list both sides. Do not resolve the conflict — surface it for the PM to investigate.

6. **Write output** — Save to `outputs/market-orientation-[market-name].md` using the format in Outputs below.

## Outputs

### `outputs/market-orientation-[market-name].md`

```
# Market Orientation: [Market Name]

**Date:** [today's date]
**Entrant context:** [company + context provided]

---

## Market Picture

### Key Players
[Table: Player | Positioning | Market Share/Reach | Notable Strength]

### Market Dynamics
[3–5 bullet points on key forces: growth drivers, consolidation trends, regulatory environment]

### Known Gaps
[Bullet list of underserved segments or unmet needs — with source signal for each]

### Conflicting Signals
[Any areas where sources disagree — surface both sides]

### What We Don't Know
[Explicit list of unknowns — gaps in secondary research that primary research must address]

---

## Draft Hypotheses (UNVALIDATED)

**H1:** We believe [segment] experiences [problem] because [signal].
- Confirmation signal: [what would confirm this]
- Kill signal: [what would disprove this]

**H2:** ...

**H3:** ...

**H4:** (if applicable)

---

## Sources
[List of sources used with URLs where available]
```

## Guidelines

- Frame all findings as hypotheses, not conclusions — even when evidence is strong
- Surface what is unknown as explicitly as what is known
- Flag conflicting signals rather than resolving them — the PM resolves, not the model
- If the market name is too broad (e.g., "payments" without geography), ask for a sub-segment or geography before searching
- If hypotheses are too vague after drafting, rewrite them to be specific and testable before outputting
- Do not fabricate market data — if a statistic cannot be sourced, flag it as unverified
