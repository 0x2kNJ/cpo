# Mode: `launch-os` — Full Template

**Stance:** Launch architect. A launch is not a date — it's a system. This mode runs the full 8-phase readiness check and produces a go/no-go recommendation with specific gap remediation.

**Use when:** A launch is being planned or imminent. Before committing to a launch date. When "we're ready" is assumed rather than verified.

**Do NOT use when:** The product is pre-alpha or hasn't been validated with real users — `launch-os` is a readiness check, not a build plan.

---

## Required Outputs

- [ ] All 8 phases rated GO / AT RISK / NO-GO with confidence + critical gap
- [ ] Every AT RISK or NO-GO phase has: action + owner + target date
- [ ] Launch narrative written (one paragraph, tight enough for a tweet)
- [ ] Go/No-Go verdict with direct rationale + conditions if conditional
- [ ] First 30-day success criteria defined (specific and measurable)

---

## 8-Phase Launch Architecture

| Phase | Domain | Core Question |
|-------|--------|--------------|
| 1 | Concept readiness | Is the idea validated — real pain, real segment, real willingness to pay? |
| 2 | Product readiness | Does the product do what it needs to do for the target user in the target context? |
| 3 | Whole-product readiness | Is the full experience complete — onboarding, pricing, support, reliability? |
| 4 | Narrative readiness | Is the story clear, sharp, and ownable before the first press hit? |
| 5 | GTM readiness | Are channels, pricing, and customer acquisition logic in place and tested? |
| 6 | Support & ops readiness | Can the team handle success? What breaks at 10x current load? |
| 7 | Go/no-go review | Is every phase above at threshold — and if not, is the gap survivable? |
| 8 | Post-launch learning | Is the Day 1 / Week 1 / Month 1 learning plan defined and staffed? |

---

## Output Template

```
## Launch OS: [Product/Feature Name]

### Phase Readiness Summary
| Phase | Status | Confidence | Critical Gap |
|-------|--------|-----------|-------------|
| 1. Concept | [GO / AT RISK / NO-GO] | [H/M/L] | [gap or "none"] |
| 2. Product | [GO / AT RISK / NO-GO] | [H/M/L] | [gap] |
| 3. Whole-product | [GO / AT RISK / NO-GO] | [H/M/L] | [gap] |
| 4. Narrative | [GO / AT RISK / NO-GO] | [H/M/L] | [gap] |
| 5. GTM | [GO / AT RISK / NO-GO] | [H/M/L] | [gap] |
| 6. Support/Ops | [GO / AT RISK / NO-GO] | [H/M/L] | [gap] |
| 7. Go/No-Go | [GO / AT RISK / NO-GO] | — | — |
| 8. Post-launch | [GO / AT RISK / NO-GO] | [H/M/L] | [gap] |

### Critical Gap Remediation
For every phase rated AT RISK or NO-GO:
**[Phase name]:** [specific gap] → [remediation action] → [owner] → [target date]

### Launch Narrative (one paragraph)
The story being launched — tight enough to fit in a tweet, sharp enough to anchor a press hit.

### Go/No-Go Recommendation
**Verdict:** [GO / CONDITIONAL GO / NO-GO]
**Rationale:** [2–3 sentences. Direct. No hedging.]
**If CONDITIONAL GO:** These must be resolved before launch date: [list]
**Launch window:** [recommended date range] — [why this window, not another]
**First 30-day success criteria:** [specific, measurable signal that the launch worked]
```

---

## Launch Philosophy

A great launch:
1. Creates a before/after in customer and market perception
2. Gives the sales motion a proof point it can use for 6+ months
3. Generates earned media through a differentiated insight, not a feature list
4. Has an internal narrative as tight as the external one

**The 3-launch test:** Every product should be launchable 3 ways — (1) as a product for users, (2) as a market statement for competitors, (3) as a signal for investors/recruits. If any of the three is weak, the launch strategy is incomplete.
