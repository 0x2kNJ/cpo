# Adoption Playbooks: B2B, B2C, and Hybrid

Deep reference for the Head of Product & Strategy skill. Load when evaluating go-to-market strategy, adoption mechanics, or growth models.

## Table of Contents
1. [B2B Adoption Mechanics](#b2b-adoption)
2. [B2C Adoption Mechanics](#b2c-adoption)
3. [B2B2C / Hybrid Models](#hybrid-models)
4. [Developer-Led Adoption (PLG for Devs)](#developer-led)
5. [Crypto/Web3 Adoption Patterns](#crypto-adoption)
6. [Growth Model Selection](#growth-model-selection)

---

## B2B Adoption

### The Buying Committee
Enterprise sales is never one person. Map the committee:

| Role | What they care about | How to win them |
|------|---------------------|----------------|
| **Economic Buyer** | ROI, budget, strategic alignment | Business case, competitive risk |
| **End User** | Does it make my job easier/better? | Product experience, time to value |
| **Champion** | Career advancement, innovation credit | Enablement, success stories, internal positioning |
| **Technical Evaluator** | Security, integration, scalability | Documentation, architecture review, SOC 2 |
| **Blocker** | Risk, disruption, change management | Address objections proactively, reference customers |

### Sales Motions

**Product-Led Growth (PLG)**
- Users discover, try, and adopt before sales gets involved
- Works when: product is self-serve, value is immediate, individual use case is strong
- Fails when: procurement requires enterprise features to even evaluate
- Key metric: PQL (Product Qualified Lead) → how do you identify users ready for sales?
- Examples: Slack, Notion, Vercel

**Sales-Led**
- Sales drives discovery, demo, and close
- Works when: deal size > $50K ARR, buying process requires hand-holding
- Fails when: product needs too much customization to demo well
- Key metric: pipeline velocity (time from first touch to close)

**Channel / Partner-Led**
- Partners resell, integrate, or co-sell
- Works when: partner has distribution you don't, product is horizontal
- Fails when: partner doesn't understand your value prop or can't implement
- Key metric: partner activation rate (% of partners generating pipeline)

**Community-Led**
- Community creates awareness, trust, and advocacy
- Works when: target users are active in communities (devs, security, crypto)
- Fails when: community engagement is inauthentic or purely extractive
- Key metric: community-sourced pipeline %

### B2B Adoption Stages

**1. Land (First Contract)**
- Sell a wedge use case, not the full platform
- Minimize time to value (ideally < 1 week to see results)
- Reduce procurement friction (free tier, self-serve, design partner terms)
- Success = one team using the product and getting value

**2. Expand (Grow Within Account)**
- Department → department spread (horizontal expansion)
- More seats, more usage, more use cases (vertical expansion)
- Upsell to higher tier (feature expansion)
- Key: make expansion feel organic, not sales-driven

**3. Retain (Prevent Churn)**
- Monitor usage signals (declining logins, support tickets, feature adoption)
- Quarterly business reviews tied to their KPIs, not yours
- Make the switching cost real (data, integrations, workflow dependencies)
- Warning sign: champion leaves the company

**4. Advocate (Create References)**
- Turn successful customers into case studies, references, speakers
- This is marketing infrastructure, not a nice-to-have
- Best advocates are ones who got promoted because of your product

### B2B Pricing Dynamics
- Annual contracts > monthly (cash flow, commitment, churn reduction)
- Land cheap, expand fast (low entry price, high expansion revenue)
- Usage-based components create natural expansion
- Enterprise tier must include security, SSO, audit logs, SLAs (these are table stakes, not premium features)

---

## B2C Adoption

### The Funnel (Pirate Metrics / AARRR)

**Acquisition** — How users find you
- Channels: organic search, social, paid ads, referral, content, app store
- Key: know your CAC per channel and measure quality, not just volume
- Best early signal: which channel produces the highest-retaining users?

**Activation** — First value experience
- Define the "aha moment" — the specific action that correlates with retention
- Measure time-to-aha and remove every friction point between sign-up and that moment
- Examples: Facebook (add 7 friends in 10 days), Slack (2000 team messages)
- If you don't know your aha moment, run cohort analysis: what do retaining users do that churning users don't?

**Retention** — Users come back
- This is the only metric that matters at scale
- Measure by cohort, not in aggregate (aggregate masks decay)
- Retention curves should flatten, not approach zero
- D1/D7/D30 for consumer; Weekly/Monthly for less-frequent products
- If retention is broken, growth = pouring water into a leaky bucket

**Revenue** — Users pay
- Free → paid conversion rate by cohort
- Willingness to pay test: what percentage upgrade when you introduce a paywall?
- Premium value must be 10x the price (perceived, not actual)
- Subscription fatigue is real — monthly fees face higher scrutiny than ever

**Referral** — Users bring others
- Viral coefficient (K) = invites sent per user × conversion rate of invites
- K > 1 = exponential growth (very rare). K > 0.5 = meaningful growth boost
- Referral mechanics: inherent (product requires sharing) vs. artificial (incentivized referral codes)
- Inherent virality > artificial virality (more sustainable, higher-quality users)

### Consumer Growth Loops

**Content Loop**
User creates content → content is discoverable (SEO, social) → new user discovers content → new user creates content
- Works for: UGC platforms, tools that produce shareable output
- Key: the content must be valuable to non-users

**Viral Loop**
User invites others → new user joins → new user invites others
- Works for: social products, collaboration tools
- Key: invitation must deliver value to both sender and recipient

**Paid Loop**
Revenue funds acquisition → acquisition produces revenue → revenue funds more acquisition
- Works for: high-LTV products with predictable conversion
- Key: LTV:CAC ratio must sustain the loop with margin

**Product-Led Loop**
User adopts → product gets better with more users → attracts more users
- Works for: marketplace, social, data-network-effect products
- Key: the improvement must be tangible and visible

### Consumer Monetization Models
- **Freemium**: Free core, paid premium. Works when free tier drives growth and paid tier is clearly differentiated.
- **Subscription**: Recurring payment for access. Works when value is continuous and frequency is high.
- **Transaction fee**: Take a cut of each transaction. Works for marketplaces and payment products.
- **Advertising**: Monetize attention. Works at massive scale only. Degrades experience.
- **Data monetization**: Monetize aggregate insights. Privacy/trust risk. Increasingly regulated.

---

## Hybrid Models

### B2B2C
You sell to a business, which uses your product to serve their consumers.

**Key tension**: Business buyer wants control, ROI, compliance. End consumer wants simplicity, speed, delight. You must satisfy both.

**Who to optimize for first**: Almost always the end consumer. If the consumer experience is bad, the business buyer gets no ROI. But you must meet the business buyer's minimum requirements (security, branding, reporting) or you can't get in the door.

**Example framework**:
1. Build for end consumers first (get the experience right)
2. Layer on business controls (admin panel, branding, compliance)
3. Sell to businesses using consumer metrics as proof (adoption rate, NPS, completion rate)

### PLG → Enterprise
Start with self-serve adoption, layer on enterprise sales when ready.

**The transition checklist**:
- [ ] Individual users are adopting without sales involvement
- [ ] Usage within companies is crossing team boundaries
- [ ] Companies are hitting the limits of the free/self-serve tier
- [ ] Security and compliance requests are increasing
- [ ] You're seeing > $1K MRR accounts organically

**What to build for the transition**:
- Admin controls (user management, SSO, SCIM)
- Usage analytics dashboard (for the buyer to see ROI)
- Custom billing and invoicing
- Dedicated support tier
- Security documentation (SOC 2, penetration testing, data residency)

---

## Developer-Led Adoption

Developers are the most discerning, least forgiving, and highest-leverage buyers in tech.

### What Developers Actually Care About
1. **Does it work?** (reliability, correctness)
2. **Can I understand it?** (documentation, examples, error messages)
3. **Does it respect my time?** (fast setup, sensible defaults, no boilerplate)
4. **Can I trust it?** (stability, backward compatibility, transparent pricing)
5. **Does it make me look good?** (impressive output, production quality)

### Developer Adoption Funnel
1. **Discovery**: GitHub, Hacker News, Twitter/X, StackOverflow, conference talks, peer recommendation
2. **Evaluation**: README, quickstart, docs quality, example projects (this is your product for developers)
3. **First integration**: Time from discovery to "hello world" (target: < 5 minutes)
4. **Production use**: Migration from POC to real workload (where most drop off)
5. **Advocacy**: Developer writes about you, speaks about you, contributes to you

### Developer-Specific Growth Tactics
- **Documentation is marketing.** Developers Google problems, not products. Rank for their problems.
- **SDKs in their language.** Not just REST API docs — native SDKs that feel idiomatic.
- **Free tier must be real.** Not 14-day trial. Developers prototype on weekends; they need persistent access.
- **Open source signal.** Even if your core is proprietary, open-source SDKs, examples, and tools build trust.
- **Changelog as content.** Developers read changelogs. A good changelog is a trust signal.
- **Community ≠ Discord server.** It's GitHub issues responded to promptly, PRs reviewed, and feedback incorporated visibly.

---

## Crypto/Web3 Adoption Patterns

### Unique Dynamics
- **Community IS the product** (in protocol land). No community = no network = no value.
- **Tokens as distribution** — incentivize early adoption with token rewards (but beware mercenary capital)
- **Composability** — your protocol's adoption increases when other protocols build on top of it
- **Governance as feature** — token holders influence roadmap (alignment mechanism AND coordination cost)

### Adoption Stages in Crypto
1. **Testnet / Beta**: Developer and degen adoption. Focus on technical correctness.
2. **Mainnet Launch**: First real users. Token event. Focus on security and stability.
3. **Ecosystem Growth**: Other projects building on you. Focus on developer experience and grants.
4. **Mainstream Bridge**: Non-crypto users arriving. Focus on abstracting complexity (wallets, gas, signing).

### Crypto Adoption Anti-Patterns
- Token launch before product-market fit (attracts speculators, not users)
- Airdrop farming (users extract value and leave)
- Governance theater (no one votes, decisions are made by core team anyway)
- Decentralization theater (technically decentralized, practically controlled)
- Building for "crypto-native" only (limits TAM to <1% of internet users)

### Bridging Crypto and Mainstream
- Abstract the wallet (social login, MPC wallets, passkeys)
- Abstract the gas (gasless transactions, session keys)
- Abstract the chain (chain abstraction, intent-based architectures)
- Lead with the use case, not the technology ("verify your identity" not "sign a ZK proof")

---

## Growth Model Selection

### Decision Matrix

| Factor | Content-Led | Viral | Sales-Led | PLG | Community-Led |
|--------|------------|-------|-----------|-----|--------------|
| **Best for** | SEO-heavy, educational | Social, collab tools | Enterprise, high ACV | Dev tools, prosumer | Open source, crypto |
| **CAC** | Low (long-term) | Very low | High | Low-medium | Low |
| **Time to scale** | 6-12 months | Fast if K>0.5 | Immediate with team | 3-6 months | 6-18 months |
| **Defensibility** | Medium (SEO moat) | High (network effects) | Low (team can be poached) | High (product moat) | High (community moat) |
| **When it fails** | Content is generic | Product isn't inherently social | Product needs too much customization | Too complex for self-serve | Community is inauthentic |

### The Right Answer is Usually a Combination
- Primary loop (drives majority of growth)
- Amplifier loop (accelerates primary loop)
- Example: PLG (primary) + Content (amplifier) + Sales (enterprise overlay)

### Choosing Your Primary Loop
1. How does your target user discover tools? (Where do they hang out?)
2. Does your product naturally involve others? (inherent virality?)
3. What's the ACV? (>$50K → sales-assisted, <$500 → must be self-serve)
4. Is the value immediate or does it compound? (immediate → PLG, compound → community)
5. Can you produce content that's valuable independent of the product? (yes → content-led)
