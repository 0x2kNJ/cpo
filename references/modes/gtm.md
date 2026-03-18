# Mode: `gtm` — Full Template

**Stance**: Go-to-market architect. Design how the product reaches the people who need it — which segment first, how they find you, what converts them, and how the motion compounds. GTM is not a launch checklist — it's a strategic bet on how you grow.

**Use when**: planning a product launch, designing a GTM motion, identifying the beachhead segment, defining the ICP, building growth loops, creating competitive positioning.
**Do NOT use when**: the product itself isn't defined yet (use `blue-ocean` or `ceo` first), you need fundraising narrative (use `investor-story`), or you need a detailed marketing calendar (that's execution, not strategy).

**Five Truths emphasis**: Strategic Truth (primary) — GTM is about positioning, wedge selection, and motion design, all of which compound or collapse strategically. Economic Truth (secondary) — GTM choices determine unit economics. The wrong motion destroys margins even with a great product.

**Evidence labeling**: "We'll win enterprise" is a **judgment**. "We have 3 signed LOIs from enterprise buyers" is a **fact**. "Enterprise buyers in this category typically have 6-month sales cycles" is an **inference**. Label accordingly.

---

## Calibration

Ask via `AskUserQuestion` before running the template. Max 4 total.
*If AskUserQuestion unavailable:* ask as a numbered list and wait for response.

| Question | Header | Options |
|---|---|---|
| What stage is this launch? | Stage | Zero → 1 (first customers ever) · 1 → 10 (early market expansion) · 10 → 100 (scaling a proven motion) |
| Who do you believe is the primary buyer/user? | Buyer | Developers / technical users · Product managers / operators · Enterprise buyers / procurement · Consumers |
| What motion are you currently using or considering? | Motion | Product-led growth (PLG) · Sales-led · Community-led · Partnership / channel · Not sure yet |
| What's the single biggest GTM uncertainty? | Risk | Finding / reaching the right buyers · Converting buyers who find us · Pricing / packaging · Competing vs. well-funded incumbents |

**Calibration adjustments**:
- *Zero → 1 + not sure of motion yet*: Don't design a growth loop yet. Start with beachhead and ICP — get those crisp first. Motion follows from who the buyer is and how they make decisions.
- *10 → 100 + PLG*: Compress beachhead and ICP (already known). Go deep on growth loop mechanics, unit economics, and what's breaking at scale.
- *Enterprise buyer + sales-led*: Weight ICP, messaging, and sales motion sections heavily. Add a competitive battlecard section. Skip virality mechanics.
- *Pricing uncertainty*: Run the Pricing & Packaging section at double depth. A great product with wrong pricing destroys GTM.

---

## Required Outputs

- [ ] Beachhead segment named (specific, not "SMBs" or "developers")
- [ ] ICP defined with 5 specific dimensions
- [ ] GTM motion selected with rationale
- [ ] Growth loop designed (or reason why one doesn't apply yet)
- [ ] Messaging hierarchy completed (headline → value props → proof points)
- [ ] Launch sequencing produced (what gates what)
- [ ] North Star GTM metric named with current baseline and target
- [ ] Competitive positioning stated (not just "we're better" — what specifically and for whom)

---

## Template

1. **Beachhead Segment** — The specific group you win first.
   The biggest GTM mistake is targeting too broadly. The beachhead is the smallest viable market you can dominate — and dominance creates the credibility and momentum to expand.
   - Name the beachhead in one sentence: "[Specific role] at [specific company type] in [specific context] who [specific pain]."
   - Why this segment first? (Network effects into adjacent segments? High willingness to pay? Low competitive intensity? Proof-of-concept for the broader market?)
   - What does winning this segment unlock? (Reference customers? Distribution into adjacent segments? Protocol / standard adoption?)
   - Beachhead size: How many of these exist? What's the accessible market in Year 1?
   - Anti-pattern check: "Developers" is not a beachhead. "Developers building auth for the first time at Series A startups" is getting closer.

2. **Ideal Customer Profile (ICP)** — Who you optimize every system to serve.

   | Dimension | Definition | Why it matters |
   |-----------|-----------|---------------|
   | Company type | [size, stage, industry, tech stack] | |
   | Buyer role | [title, decision authority, budget ownership] | |
   | User role | [who uses it daily — may differ from buyer] | |
   | Problem trigger | [the event that makes them search for a solution] | |
   | Success signal | [how they know it's working] | |

   - What does the ICP NOT look like? Name 2-3 anti-ICPs — who looks similar but churns or doesn't convert. Knowing who not to chase saves as much as knowing who to pursue.
   - Where does the ICP spend time? (Channels they actually use, events they attend, communities they trust)

3. **GTM Motion Selection** — How customers find you and decide to buy.

   | Motion | Best for | Unit economics | Time to first revenue | Risk |
   |--------|---------|----------------|----------------------|------|
   | **Product-led (PLG)** | Developer tools, bottoms-up SaaS, consumer | Low CAC, high volume, viral potential | Fast if product hooks work | Requires strong activation design |
   | **Sales-led** | Enterprise, complex buying, high ACV | High CAC, high LTV, long cycle | Slow (6-18 months pipeline) | Needs GTM hire early |
   | **Community-led** | Developer ecosystems, Web3, open source | Very low CAC, high trust, slow | Very slow, can't predict | Hard to convert to revenue |
   | **Partnership / channel** | Embedded integrations, distribution leverage | Shared economics, leveraged | Medium | Dependency risk |

   Select the primary motion. Explain why this fits your ICP, product maturity, and stage.
   Secondary motion: Is there a complementary motion for a later stage? When do you add it?

4. **Growth Loop** — The mechanism that makes growth compound.
   Not all products have viral loops. Don't force one. But if a natural loop exists, designing it deliberately is the highest-leverage GTM work.

   ```
   [USER ACTION] → [VALUE CREATED] → [SHARING TRIGGER] → [NEW USER ACQUIRED] → [loop]
   ```

   Example types:
   - **Viral loop**: Users invite others because they get direct value from it
   - **Content loop**: Users generate content that attracts organic search / social traffic
   - **Ecosystem loop**: Developers build integrations; integrations attract users; users attract more developers
   - **Network loop**: More users = more value for each user (marketplaces, communication tools)

   If no natural loop exists: What's the retention mechanism? What keeps users coming back and expanding usage?

5. **Messaging Hierarchy** — What you say and in what order.

   | Layer | Message | Audience |
   |-------|---------|---------|
   | **Category claim** | [What category are you? Or creating?] | All audiences |
   | **Primary value prop** | [The job you do better than anything else] | ICP buyer |
   | **Proof point 1** | [Most credible evidence: metric, customer, outcome] | ICP buyer |
   | **Proof point 2** | [Second best evidence] | ICP buyer |
   | **User value prop** | [Why the daily user loves it — may differ from buyer value prop] | ICP user |
   | **Competitive wedge** | [Why us vs. [specific competitor] for [specific use case]] | Buyers evaluating alternatives |

   - One-liner test: Can you summarize the category claim in one sentence that a smart person outside the industry understands immediately?
   - Jargon audit: Remove industry terms that require insider knowledge. If you need a glossary, the message is wrong.

6. **Pricing & Packaging** — How value is captured.
   - Pricing model: Per seat / usage-based / flat / freemium / token-gated / hybrid?
   - Anchoring: What's the reference price in the buyer's mind? (What do they pay today for the same job?)
   - Entry point: What's the lowest-friction way for an ICP to start? (Free tier, trial, pilot, self-serve?)
   - Expansion motion: How does revenue per customer grow? (More seats, more usage, more modules, upgraded tier?)
   - The test: Would a buyer feel the price is fair relative to the value they receive? If you're not sure — you need more discovery on willingness to pay before you lock pricing.

7. **Launch Sequencing** — What must be true before each gate.

   | Gate | What's needed | Who owns it | Timing |
   |------|--------------|-------------|--------|
   | **Private beta** | Core product works, 5-10 design partners signed | PM + Eng | [Date] |
   | **Public beta / waitlist** | Messaging validated, activation flow works, support ready | PM + Marketing | [Date] |
   | **GA launch** | Retention proven, pricing locked, sales playbook written | Full team | [Date] |
   | **Growth motion on** | Unit economics confirmed, channel proven, loop designed | GTM lead | [Date] |

   Kill condition at each gate: What would make you delay rather than proceed?

8. **GTM Metrics** — The numbers that tell you if the motion is working.

   | Metric | What it measures | Current baseline | Target (90 days) | Owner |
   |--------|-----------------|-----------------|-----------------|-------|
   | **North Star** | [Core adoption signal] | | | |
   | **Activation rate** | [% who hit first value moment] | | | |
   | **CAC** | [Cost to acquire a customer] | | | |
   | **Time to value** | [From signup to first "aha"] | | | |
   | **Expansion rate** | [Revenue growth within existing accounts] | | | |
   | **Churn rate** | [Logo and revenue retention] | | | |

   Warning: If you're tracking more than 5 GTM metrics actively, you're tracking none of them. Pick the North Star and 2-3 inputs. Add the rest to a monthly review.
