# Product Strategy Frameworks

Extended reference material for the Head of Product & Strategy skill. Load this when you need deeper framework application beyond what's in the main SKILL.md.

## Table of Contents
1. [RICE Scoring](#rice-scoring)
2. [Kano Model](#kano-model)
3. [Jobs-to-be-Done (JTBD)](#jobs-to-be-done)
4. [Product-Market Fit Indicators by Stage](#pmf-indicators)
5. [Porter's Five Forces (Tech-Adapted)](#porters-five-forces)
6. [Unit Economics Framework](#unit-economics)
7. [Pricing Strategy Models](#pricing-strategy)
8. [Platform vs. Product Decision Framework](#platform-vs-product)

---

## RICE Scoring

Use when you need to compare multiple initiatives against each other. Never use as the sole decision tool — it's a conversation starter, not a conversation ender.

| Factor | Definition | How to Estimate |
|--------|-----------|----------------|
| **Reach** | How many users/accounts will this affect in a defined time period? | Use existing data: DAU, segment size, funnel step volume. Be specific about the segment. |
| **Impact** | How much will this move the metric that matters? | 3 = massive, 2 = high, 1 = medium, 0.5 = low, 0.25 = minimal. Justify your score. |
| **Confidence** | How sure are you about reach and impact? | 100% = high (strong data), 80% = medium (some signal), 50% = low (gut feeling) |
| **Effort** | Person-weeks to ship (design + eng + QA) | Get an eng estimate. If you can't, use T-shirt sizes: S=1, M=2, L=4, XL=8 |

**RICE Score = (Reach × Impact × Confidence) / Effort**

**When RICE misleads:**
- High-reach, low-impact features will outscore low-reach, high-impact ones. For early-stage products, impact matters more than reach.
- RICE doesn't capture strategic value — a feature that unlocks a new segment or creates a moat may score low on RICE but be the right call.
- RICE assumes effort is linear. Some high-effort items are foundational and amortize across future work.

**Rule**: If RICE says "do X" but your strategic instinct says "do Y", investigate the gap. The gap is where the interesting insight lives.

---

## Kano Model

Use when evaluating feature portfolios or understanding what drives satisfaction vs. dissatisfaction.

### Feature Categories

**Must-haves (Basic)**: Absence causes dissatisfaction. Presence doesn't create delight. These are table stakes.
- Example: Login works. Data doesn't disappear. API returns correct responses.
- Strategy: Ship these first. Don't over-invest. Meet the bar, move on.

**Performance (Linear)**: More = better. Direct correlation between investment and satisfaction.
- Example: Speed, accuracy, number of integrations, storage limits.
- Strategy: Invest here when competing on the same axis as competitors. Diminishing returns at the top end.

**Delighters (Attractive)**: Absence doesn't cause dissatisfaction. Presence creates outsized delight.
- Example: Unexpected automation, proactive suggestions, beautiful data visualizations.
- Strategy: These differentiate. But they decay into must-haves over time as expectations rise.

**Indifferent**: Users don't care either way. Stop building these.

**Reverse**: Some users actively dislike this feature. Common with "helpful" features that feel intrusive.

### Application
1. List your top 15-20 features/capabilities
2. For each, ask: "How would you feel if this feature was present?" and "How would you feel if it was absent?"
3. Map to categories
4. Ensure your roadmap has the right mix: must-haves covered, performance features competitive, 1-2 delighters in each release

---

## Jobs-to-be-Done

Use when you're unsure about product-market fit or need to reframe the problem from the user's perspective.

### The Core Question
"What job is the user hiring this product to do?"

### Job Statement Format
**When [situation], I want to [motivation], so I can [expected outcome].**

Example: "When I'm onboarding a new enterprise customer, I want to verify their employees' identities in bulk, so I can activate their accounts within the SLA window without manual review."

### Dimensions of a Job
- **Functional**: The practical task (verify identity, process payment, deploy code)
- **Emotional**: How the user wants to feel (confident, in control, not worried about security)
- **Social**: How the user wants to be perceived (competent, modern, trustworthy)

### Competing Solutions
Every job has existing solutions — including doing nothing. Map:
1. What do users do today? (current solution)
2. What's painful about it? (underserved outcomes)
3. What would they switch from? (real competition — often not who you think)
4. What's the switching cost? (effort, risk, retraining)

### When JTBD Changes Strategy
- If the job is "look compliant for an audit" vs. "actually prevent fraud" — these are different products
- If the job is "ship fast" vs. "ship correctly" — different positioning, different pricing
- If the job is done infrequently, acquisition cost must be low (users won't remember you)

---

## PMF Indicators

Product-market fit is not binary. It's a spectrum with measurable signals at each stage.

### Pre-PMF (Searching)
| Signal | What to look for |
|--------|-----------------|
| Problem interviews | 8/10 prospects describe the same pain unprompted |
| Waitlist / sign-up intent | >5% conversion on landing page with no product |
| Design partner commitment | 3+ companies willing to invest time in co-development |
| Willingness to pay | Prospects name a price before you do |

### Early PMF (Found Something)
| Signal | What to look for |
|--------|-----------------|
| Organic sign-ups | Users finding you without paid acquisition |
| Sean Ellis test | >40% of users say they'd be "very disappointed" if the product disappeared |
| Usage frequency | Users returning at expected cadence (daily for tools, weekly for analytics, etc.) |
| Support signal | Users complaining about missing features (not about core value prop) |
| Word of mouth | "How did you hear about us?" → "A friend/colleague told me" > 30% |

### Strong PMF (Scaling)
| Signal | What to look for |
|--------|-----------------|
| Retention curves | D30 retention > 20% (consumer), D90 > 40% (B2B SaaS) |
| Net revenue retention | >100% (expansion exceeds churn) |
| Sales cycle compression | Deals closing faster quarter over quarter |
| Inbound > outbound | More pipeline from inbound than outbound |
| Hiring is the bottleneck | Not product-market fit, but ability to serve demand |

### Anti-PMF Signals (Red Flags)
- High sign-ups, low activation (value prop mismatch)
- Users complete onboarding but don't return (wrong job-to-be-done)
- Lots of feature requests, no usage (building for imagined users)
- Customers churning after initial contract (sold a vision, not a product)
- Need discounts to close (value not demonstrated)

---

## Porter's Five Forces (Tech-Adapted)

Use when assessing market attractiveness and competitive dynamics.

### 1. Competitive Rivalry
- How many direct competitors? How well-funded?
- Is competition on price, features, distribution, or brand?
- In AI/crypto: how fast do advantages decay? (model parity happens fast)
- Is the market growing fast enough that competition is positive-sum?

### 2. Threat of New Entrants
- What are the barriers? (Technical complexity, data requirements, regulatory, network effects)
- In AI: barriers are LOW for application layer, HIGH for infrastructure layer
- In crypto: barriers are LOW for forks, HIGH for community/liquidity
- In identity: barriers are HIGH (trust takes years to build)

### 3. Threat of Substitutes
- What else solves the same job? (Including manual processes, spreadsheets, "do nothing")
- AI risk: GPT-wrapper problem — if a foundation model adds your feature natively, you're a substitute away from zero
- Crypto risk: L1/L2 adding native functionality that makes your protocol unnecessary

### 4. Bargaining Power of Buyers
- How concentrated are your customers?
- B2B enterprise: high buyer power (few large contracts)
- B2C: low individual power, high collective power (churn in aggregate)
- Developer platforms: medium power (high switching costs once adopted, but loud and demanding)

### 5. Bargaining Power of Suppliers
- In AI: model providers (OpenAI, Anthropic) have enormous supplier power
- In crypto: validator economics, gas costs, bridge dependencies
- In identity: government data sources, credential issuers
- Key question: can your supplier become your competitor?

---

## Unit Economics Framework

Use when evaluating commercial viability or pricing decisions.

### Key Metrics

| Metric | Formula | Healthy Range |
|--------|---------|--------------|
| **CAC** (Customer Acquisition Cost) | Total sales & marketing spend / New customers | Varies by segment |
| **LTV** (Lifetime Value) | ARPU × Gross Margin × (1 / Churn Rate) | LTV:CAC > 3:1 |
| **Payback Period** | CAC / (ARPU × Gross Margin) | < 12 months (SaaS), < 3 months (consumer) |
| **Gross Margin** | (Revenue - COGS) / Revenue | > 70% (software), > 50% (AI with inference costs) |
| **Net Revenue Retention** | (Starting MRR + Expansion - Contraction - Churn) / Starting MRR | > 110% for B2B SaaS |
| **Burn Multiple** | Net Burn / Net New ARR | < 2x is excellent, < 3x is good |

### AI-Specific Cost Considerations
- Inference cost per request (and how it scales with usage)
- Fine-tuning costs (one-time vs. recurring)
- Data storage and processing costs
- Model API pricing risk (your COGS is someone else's revenue)

### Crypto-Specific Economics
- Gas costs as variable COGS
- Token incentive costs (are you paying for growth with inflation?)
- Protocol revenue vs. token value (sustainable vs. speculative)

---

## Pricing Strategy Models

### Value-Based Pricing (Default Choice)
Price based on the value delivered, not cost to serve.
1. Quantify the customer's status quo cost (time, money, risk)
2. Your price = 10-30% of the value you create
3. Test willingness to pay before building pricing pages

### Usage-Based Pricing
Align cost with value consumption. Good for developer platforms and API products.
- Pro: Low barrier to entry, scales with customer success
- Con: Revenue unpredictability, hard to forecast
- Works when: usage correlates strongly with value delivered

### Seat-Based Pricing
Traditional SaaS model. Good for collaboration tools.
- Pro: Predictable revenue, easy to understand
- Con: Incentivizes minimizing seats, not maximizing adoption
- Anti-pattern for developer tools (penalizes the behavior you want)

### Freemium vs. Free Trial
- **Freemium**: Permanent free tier. Use when the free tier creates network effects or generates valuable data.
- **Free trial**: Time-limited full access. Use when the product's value requires experiencing the full version.
- **Reverse trial**: Start with premium, downgrade to free. Best conversion rates for products where the difference between tiers is obvious.

### Pricing Anti-Patterns
- Pricing by cost-plus (leaves money on the table)
- Too many tiers (decision paralysis)
- Enterprise tier = "call us" with no anchor (buyers need a reference point)
- Charging for things that should drive adoption (paywalling API docs, limiting integrations)

---

## Platform vs. Product Decision Framework

Use when deciding whether to build a product, a platform, or both.

### Products
- Solve a specific problem for a specific user
- Revenue comes from the product itself
- You control the experience end-to-end
- Faster to build, easier to iterate

### Platforms
- Enable others to build products
- Revenue comes from the ecosystem (fees, subscriptions, data)
- You control the primitives, others control the experience
- Slower to build, harder to iterate, much harder to get right

### The Sequence Matters
1. Build a product that works
2. Extract the reusable primitives
3. Open those primitives as a platform
4. Let the ecosystem extend beyond what you'd build alone

**Mistake**: Starting with "we're building a platform" before having a successful product. Platforms without products are empty marketplaces.

### When to Platformize
- Multiple teams (internal or external) are building similar things on your infrastructure
- The value of your system increases with the number of builders on it
- You can't serve all use cases yourself, but you can serve the primitive layer
- Lock-in from platform adoption is strategically valuable

### When NOT to Platformize
- You don't have product-market fit yet
- Your API surface area is still changing rapidly
- You can't maintain backward compatibility
- The abstraction isn't clear (if you can't explain the platform boundary, it's not ready)
