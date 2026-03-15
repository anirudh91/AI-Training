---
name: regulatory-feasibility-check
description: >
  This skill should be used when the user wants to surface regulatory blockers for a foreign
  company entering a specific market — early, before investing in strategy. Takes country,
  company name, and operation type; returns regulatory status, known blockers, open questions,
  and a confidence level based on available public information.
disable-model-invocation: true
argument-hint: "[country] [company] [operation-type] [known-constraints]"
allowed-tools: WebSearch Write
---

# Regulatory Feasibility Check

Surface regulatory blockers for a foreign payment company entering a specific market — early, before investing in strategy.

## Workflow

1. **Parse inputs** — Extract from `$ARGUMENTS`:
   - Market / country name (e.g., "India")
   - Company name and type (e.g., "PayPal — foreign payment processor")
   - Nature of operation (e.g., "domestic retail payments, cross-border remittance")
   - Known constraints already identified (e.g., "RBI data localisation, PayPal shut down India domestic payments in 2021")
   If country or company is missing, ask before proceeding.

2. **Research regulatory environment** — Use WebSearch to investigate:
   - Central bank / financial regulator rules for foreign payment companies
   - Data localisation requirements
   - Licensing requirements for the stated operation type
   - Any prior enforcement actions or precedents involving the company or similar entrants
   - Pending or proposed regulatory changes that would affect entry
   Run at least 3–4 searches across regulatory bodies, news sources, and official government portals.

3. **Assess what the company can legally do today** — Based on research:
   - What operations are permitted under current rules
   - What requires regulatory approval or licensing (and timeline if known)
   - What is explicitly prohibited

4. **Identify blockers** — List each blocker with:
   - Rule or regulation name
   - What it prevents
   - Whether it is a hard blocker (legally prohibited) or soft blocker (requires approval/process)
   - Whether it applies today or is pending

5. **Flag open questions for expert verification** — List regulatory areas where:
   - Public information is insufficient or ambiguous
   - Rules are complex enough to require legal expert review
   - Recent changes may not be reflected in available sources

6. **Assign confidence level** — Based on source quality and recency:
   - **High:** Rules are clear, recent, and from official sources
   - **Medium:** Rules are sourced but may be outdated or partially conflicting
   - **Low:** Limited public information — recommend expert consultation before proceeding

7. **Write output** — Save to `outputs/regulatory-check-[country]-[company].md`.

## Outputs

### `outputs/regulatory-check-[country]-[company].md`

```
# Regulatory Feasibility Check: [Company] in [Country]

**Date:** [today's date]
**Operation type:** [stated operation]
**Overall confidence:** High / Medium / Low

---

## What [Company] Can Legally Do Today
[Bullet list of permitted operations under current rules]

## Known Blockers

| Blocker | Rule / Regulation | Type | Status |
|---------|-------------------|------|--------|
| ...     | ...               | Hard / Soft | Current / Pending |

## Open Questions (Require Expert Verification)
[Bullet list — each item flags what is unknown and why expert input is needed]

## Recency Risk
[Flag any rules that may have changed since available sources were published]

## Recommended Next Steps
[Legal/regulatory expert consultation areas, specific bodies to contact]

---

## Sources
[List sources with URLs and publication dates where available]
```

## Guidelines

- Always surface regulatory blockers before strategy discussion — not after
- Distinguish between current rules and pending/proposed changes
- Flag recency risk explicitly — regulatory environments change; recommend verification against live sources
- When legal expert verification is required, name it specifically — do not bury it in caveats
- If the regulatory landscape is too complex for secondary sources alone, say so clearly and recommend expert consultation
- Do not fabricate regulatory rules — if a rule cannot be sourced, flag as "unverified, requires expert confirmation"
