# Frameworks Reference

## Five Truths — Extended

### User Truth
- Behavior > stated preference. What people do, not what they say.
- Jobs-to-be-Done: what progress is the user trying to make?
- Switching cost: what must the user stop doing to adopt this?
- Desperation test: is the user actively searching for a solution, or passively accepting the status quo?

### Strategic Truth
- Positioning: what do we uniquely offer that alternatives don't?
- Moat reality check: does our advantage compound over time, or is it static?
  - Data moat requires: data -> model -> better product -> more data (feedback loop, not just storage)
  - Network effect requires: product nearly useless without the network (1-user test: would it work alone?)
- Competitive response: if this works, what does the incumbent do in 6 months?

### Economic Truth
- Pre-revenue: lead with displacement cost (what must the user stop paying for?), not TAM/SAM/SOM
- Post-revenue: unit economics must work at CURRENT scale, not "at 10x"
- CAC payback: months to recover customer acquisition cost
- LTV/CAC ratio: >3x healthy, <2x concerning, <1x broken

### Macro-Political Truth
- Regulatory exposure: could a law change kill this?
- Platform risk: are we building on someone else's distribution?
- Ecosystem shifts: what technology or market change could make this irrelevant?

### Execution Truth
- Team capability: can the current team build this, or does it require a hire?
- Timeline honesty: double your estimate, then ask if the bet still makes sense
- Technical debt: is this additive or does it require rearchitecting?

---

## Kill Criteria Patterns

Every kill criterion must have: **metric + threshold + timeframe**.

**Good examples:**
- "Weekly active users drop >20% MoM within 60 days post-launch"
- "CAC exceeds $50 per user after 90 days of paid acquisition"
- "Free-to-paid conversion rate below 3% after 6 months"
- "Engineering velocity drops >30% within one quarter of launch"
- "NPS score below 30 after 500 responses"

**Bad examples (missing components):**
- "Users don't like it" (no metric, no threshold, no timeframe)
- "Revenue is too low" (no threshold)
- "Churn increases" (no threshold, no timeframe)

---

## Stage-Specific Frameworks

### Pre-PMF
- RICE scoring is premature — you don't have reliable data for any component
- Focus: displacement cost, narrowest wedge, first 10 users
- Kill fast: if 10 targeted users won't use it, 10,000 random ones won't either

### Post-PMF
- NRR (Net Revenue Retention) is the north star
- Expansion > acquisition: easier to grow revenue from existing users
- Compounding loops: does each user make the product better for the next?

### Series B+
- Rule of 40: growth rate + profit margin should exceed 40%
- CAC payback under 18 months
- Path to profitability must be visible, not theoretical
