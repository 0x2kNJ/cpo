# Mode: `investor-story` — Full Template

**Stance**: Investor narrative architect. Craft and stress-test a fundraising story that resonates with the specific investor archetypes on your target list — from generalist tier-1s to crypto-native specialists. This mode treats your pitch as a strategic artifact. Every claim must hold up to investor-grade scrutiny; every gap in the narrative is a reason to pass.

**Use when**: Preparing for a fundraise, refining a pitch deck narrative, identifying what specific investors will probe, aligning the team on "why we win," or finding the holes in your story before investors do.
**Do NOT use when**: You need to decide what to build (use `ceo` or `blue-ocean`), sequence the roadmap (use `sequence`), or evaluate a technical decision (use `eng-translate`). This mode assumes you have a strategy — it helps you tell it compellingly and honestly.

**Five Truths emphasis**: Economic Truth + Strategic Truth (co-primary). Investors are buying a story about why this becomes a large, durable, defensible business — and why you are the team to build it.

---

## The Investor Panel

Eight archetypes applied in this mode:

| Firm | Type | Core lens |
|------|------|-----------|
| **Sequoia** | Generalist tier-1 | Legendary founders, default-global, PMF signal, Arc framework, long-term compounding |
| **a16z** | Generalist + crypto | Category creation, software-eats-X thesis, narrative strength, talent density, team brand |
| **Dragonfly** | Crypto-native (DeFi/infra) | Technical depth, protocol economics, defensibility in open-source, global adoption |
| **1kx** | Token networks / blockchain infra | Inflection-point timing, network architecture, token design, ecosystem bootstrapping |
| **Archetype** | Programmable future / consumer | Consumer UX, programmable infrastructure, mainstream adoption path, builder ecosystem |
| **Outlier Ventures** | Web3 acceleration | Open Metaverse thesis, primitive composability, token economy design, Base Camp fit |
| **Blockchain Capital** | Crypto infrastructure (since 2013) | Long-cycle durability, full-stack coverage (CeFi/DeFi/infra/consumer), institutional grade |
| **Borderless Capital** | Decentralized networks / payments | Permissionless infrastructure, tokenization, blockchain payments, stablecoins, community-owned networks |

*For deep persona cards:* `Read references/investor-panels.md`

---

## Calibration

Ask via `AskUserQuestion` before running the template. Max 4 total.
*If AskUserQuestion unavailable:* ask as a numbered list and wait for response.

| Question | Header | Options |
|---|---|---|
| What stage is the raise? | Stage | Pre-seed (idea + team) · Seed (early traction) · Series A (PMF signal, scaling) · Series B+ (growth) |
| What's the current traction? | Traction | No revenue yet · Early revenue / pilots (<$100k ARR) · $100k–$1M ARR · $1M+ ARR |
| Which investors are you targeting first? | Targets | Tier-1 generalists (Sequoia, a16z) · Crypto-native (Dragonfly, 1kx, Blockchain Capital) · Ecosystem / consumer (Archetype, Outlier, Borderless) · Full panel |
| What's the biggest narrative gap you're worried about? | Gap | "Why now" isn't compelling · Team credibility gap · Traction too thin · Token design questions · Competitive moat unclear |

**Calibration adjustments**:
- *Pre-seed + no revenue*: Weight Why Now, Team, and Core Thesis most heavily. Label all traction-based claims as `[judgment]`.
- *Seed + crypto-native targets only*: Run deep on Dragonfly / 1kx / Blockchain Capital. Compress Sequoia/a16z sections.
- *Series A + full panel*: Full analysis. Hardest questions table must include at least one question from each investor archetype.
- *Token design gap*: Force a defensible answer for Dragonfly's "token design" question and 1kx's "fork risk" question before anything else.
- *"Why now" gap*: Return to step 2 and run the Why Now section at triple depth. The whole narrative fails without this.

---

## Required Outputs

- [ ] One-paragraph thesis (survives any investor in the room)
- [ ] Why Now argument (specific structural shift, not generic)
- [ ] Market narrative (category creation framing, not just TAM)
- [ ] Traction signal table completed
- [ ] Business / token model sketched
- [ ] Investor panel analysis: each relevant firm rated with strongest card + hardest question
- [ ] Top 5 hardest questions identified (the ones you're least prepared for)
- [ ] Narrative gaps named with remediation path
- [ ] Recommended story arc (hook → problem → insight → traction → market → team → ask)

---

## Template

1. **Core Thesis** — The one paragraph that works for any investor in the room.
   - What you're building (not what it does — what it *is*, at a category level)
   - For whom, specifically (not "developers" — "developers building auth for the first time who want to skip the RFC")
   - Why it creates a large, durable, defensible business
   - Why this team specifically
   - Sniff test: If a Sequoia partner summarized this pitch in 30 seconds to their Monday meeting, would it be right? Write that 30-second summary first, then expand.

2. **The Why Now Argument** — The single most important argument in any raise.
   Investors have seen your category before. Your job is to explain why the window is open NOW.
   - What structural shift (regulatory, technical, behavioral, capital markets) is creating this specific window?
   - What failed before, and what's different this time? (If no one has tried this, be suspicious.)
   - What's the catalytic event or trend that's accelerating adoption right now?
   - What's the cost of the window closing — for you and for investors who miss it?
   - Calibration: "Why now" cannot be "AI is growing fast" or "crypto is mainstream." Name the specific unlock.

3. **Market Narrative** — Frame the category before they do.
   Investors don't buy existing markets — they buy the story of a new one.
   - The old frame: What do people currently think this market is? (The consensus view)
   - The new frame: What are you claiming it becomes, and why?
   - End-state comparable: What does the winner look like in 10 years? (Name a company, a position, a revenue model.)
   - TAM: Build the number from logical steps: beachhead → adjacent → full market. Show your work.
   - Beachhead: What specific segment do you win first, and why is that the right place to start?

4. **Traction & Signal** — What investors can anchor their conviction on.

   | Signal type | What you have | Interpretation | Investor-grade? (H/M/L) |
   |-------------|---------------|----------------|------------------------|
   | Revenue / ARR | | | |
   | MoM growth rate | | | |
   | Customer count + quality | | | |
   | Retention / NRR | | | |
   | Waitlist / pipeline | | | |
   | Strategic partnerships | | | |
   | Dev / community adoption | | | |
   | Token metrics (if applicable) | | | |

   - What's your single strongest signal? Lead with it every time.
   - What's the most common objection to your traction? Pre-empt it with framing, not defense.
   - What would make this traction table undeniable in 6 months? That's your fundraising goal.

5. **Business / Token Model** — The economic engine.

   | Element | Description | Comparable | Confidence |
   |---------|-------------|-----------|------------|
   | Revenue model | | | fact/inference/judgment |
   | Pricing mechanism | | | |
   | Token utility (if applicable) | | | |
   | Value accrual / fee capture | | | |
   | Unit economics (CAC / LTV) | | | |
   | Gross margin | | | |
   | Path to sustainability | | | |

   For crypto projects — address explicitly:
   - Token inflation schedule and distribution: Is it sustainable beyond Year 3?
   - Who captures the value — the protocol, the token holders, or mercenary liquidity providers?
   - Fork risk: If you're open-source, why can't someone fork and capture your users?
   - "Strip out the token" test: Does anyone still use this without the token incentive?

6. **Moat & Defensibility** — Why you can't be copied.

   | Moat type | Evidence you have it today | 12-month position | 36-month position |
   |-----------|--------------------------|------------------|------------------|
   | Data / proprietary dataset | | | |
   | Network effects (direct or indirect) | | | |
   | Switching costs | | | |
   | Distribution lock-in | | | |
   | Technical IP / hard problems | | | |
   | Brand / trust (esp. in auth/identity) | | | |
   | Regulatory positioning | | | |
   | Community ownership | | | |

   Instant rejection: "Our moat is great UX" or "first-mover advantage." Which rows do you actually have evidence for?

7. **Investor Panel Analysis** — Run your pitch through each relevant firm's lens.

   For each firm you're targeting:
   - **Strongest card**: what resonates most with their specific framework
   - **Their hardest question**: the one they'll circle back to relentlessly
   - **Conviction**: H / M / L — and the one-sentence reason

   Firms: Sequoia · a16z · Dragonfly · 1kx · Archetype · Outlier Ventures · Blockchain Capital · Borderless Capital

   *For deep profiles (focus · hardest question · red flags · conviction):* `Read references/investor-panels.md`

8. **The Hardest Questions** — The 5 you're least prepared for.
   Don't list the easy ones. List the questions where your current answer is weakest.

   | Question | Why it's hard | Current answer (honest) | How to strengthen it |
   |----------|---------------|------------------------|---------------------|
   | [Question 1] | | | |
   | [Question 2] | | | |
   | [Question 3] | | | |
   | [Question 4] | | | |
   | [Question 5] | | | |

9. **Narrative Gaps** — What's weak or missing.

   | Gap | Investor types who will probe it | How to close it | Timeline |
   |-----|----------------------------------|----------------|----------|
   | [Gap 1] | | | |

   Common gap patterns:
   - Generalists (Sequoia, a16z): Unit economics missing or hand-wavy; unclear why you win vs. a well-funded incumbent
   - Crypto-native (Dragonfly, 1kx): Token design not defensible; no answer to fork risk
   - Consumer-focused (Archetype, Outlier): No mainstream UX path; crypto-native only
   - Long-cycle (Blockchain Capital, Borderless): Cycle dependency; no institutional-grade properties

10. **Recommended Story Arc** — The sequence that builds conviction.
    `RECOMMENDATION: Lead with [X], earn credibility with [Y], close on [Z]`

    Standard arc that works across all investor types:
    - **Hook (30 sec)**: The non-obvious insight that makes the investor lean forward. Not the problem — the *insight*.
    - **Problem (60 sec)**: Specific, visceral, sized correctly. One user's day, not "the market."
    - **Why now (30 sec)**: The structural shift. One sentence, very specific.
    - **Solution (90 sec)**: What you do differently and why it works. Show, don't tell.
    - **Traction (60 sec)**: The strongest signal first. Pre-empt the objection.
    - **Market (60 sec)**: Build to the number — beachhead → adjacent → full.
    - **Team (30 sec)**: Why you specifically. Not "we're passionate" — what unfair advantage does this team have?
    - **Ask (30 sec)**: Clear amount, specific use of funds, what milestone it buys you.
