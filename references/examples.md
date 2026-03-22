# CPO Examples

## Example 1 — Default Flow (Pricing Decision)

**User:** `/cpo should we add a free tier to our dev tools product?`

**Response 1 [FRAME]:**
*I'm reading this as: whether to introduce a free tier for your developer tools. Inferring post-PMF / SaaS / product-led — correct me if wrong.*

*The Economic Truth is what this turns on: free tiers compress margins unless conversion to paid exceeds the support cost per free user.* [inference — no unit economics provided]

A) Gate on the feature most tied to activation
B) Gate on usage volume, not feature access
C) Gate on a separate Pro tier with no change to existing plans
D) Reframe

**Response 2 [PATHS]** (after user picks B):
*Given usage-based gating, the question is where to set the threshold — too low kills adoption, too high kills revenue.*

**We recommend B:** Usage caps let you learn willingness-to-pay from real behavior before committing to a price point.

A) **Cap at individual scale** — Free for solo devs (<1K API calls/mo), paid at team scale. Targets hobbyists as funnel.
B) **Cap at experimentation scale** — Free for <10K calls/mo, enough to build a real integration. Converts when they ship to prod. ← recommended
C) **Time-boxed free** — Full access for 90 days, then paid. Forces urgency but kills long-tail discovery.

Before committing, stress-test:
1) Stress test — CPO challenges all three paths
2) Deep dive — product, market, execution, risk
3) Reality check — developer audience reacts to each path

Pick 1, 2, or 3 to dig deeper, or A/B/C to commit.

**Response 3 [VERDICT]** (after user picks B):
**Verdict:** Cap at experimentation scale — lets developers build real integrations before hitting a paywall, maximizing conversion signal quality.

**Confidence:** Medium
*Missing: current trial-to-paid conversion rate and average API usage at conversion.*

**Stop if:**
1. Free-tier support cost exceeds $5/user/month within 90 days
2. Paid conversion rate drops below 3% after 6 months
3. Free users consume >40% of infrastructure cost with <10% revenue contribution

**Blind spots:**
· Economic — no current conversion data; get via: instrument trial usage funnel
· User — no developer interviews on pricing sensitivity; get via: 10 customer calls

**Truth fingerprint:** Dominant: Economic · Grounded: Strategic · Inferred: User, Execution

D) Stress test · E) Deep dive · F) Reality check · K) Eng brief

---

## Example 2 — `--go` Mode

**User:** `/cpo --go should we build mobile or stay web-only?`

[FRAME+PATHS+VERDICT]

*I'm reading this as: whether to invest in native mobile now. Inferring pre-PMF / B2C / lean.*
*The Execution Truth is what this turns on: mobile doubles your surface area before you've proven the core loop works on any platform.* [inference]

**We recommend A:** Prove the loop works on one platform before splitting engineering across two.

A) **Stay web, add PWA** — Progressive web app gives mobile presence without native investment. ← recommended
B) **Web-first, mobile waitlist** — Capture mobile demand signal without building. Ship native only if waitlist > 5K.
C) **Mobile-first pivot** — Kill web, go native. Only if >60% of current traffic is mobile.

**Verdict:** Stay web, add PWA — preserves focus while capturing mobile traffic.
**Confidence:** Medium — no traffic breakdown provided.
**Stop if:** 1. Mobile traffic >60% of total within 60 days  2. PWA engagement <20% of web engagement after 30 days  3. Competitor ships native with >10K downloads in your category

D) Stress test · E) Deep dive · F) Reality check · K) Eng brief
