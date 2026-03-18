# Moat Taxonomy & Assessment Rubric

Deep reference for the Head of Product & Strategy skill. Load when performing moat analysis, competitive assessment, or defensibility evaluation.

## Table of Contents
1. [The Six Moat Types](#six-moat-types)
2. [Moat Assessment Rubric](#assessment-rubric)
3. [Domain-Specific Moat Patterns](#domain-specific)
4. [Moat Building Playbook](#building-playbook)
5. [Moat Decay Patterns](#moat-decay)

---

## The Six Moat Types

### 1. Data Moats
**Definition**: Usage of the product generates data that improves the product, creating a virtuous cycle competitors can't replicate without equivalent usage.

**Strength indicators**:
- Each user interaction makes the product measurably better for all users
- The data is proprietary (not available from public sources)
- The improvement is visible to users (they feel it getting better)
- The data has compounding value (more data = disproportionately better, not just linearly better)

**Examples in relevant domains**:
- AI: fine-tuning data from user interactions, preference data, evaluation datasets
- Identity: verified credential network (more issuers + more verifiers = more useful)
- Auth: risk signals from authentication attempts (more logins = better fraud detection)

**Common mistake**: Confusing "we have data" with "we have a data moat." The question is whether the data creates a feedback loop that improves the product. Storing data isn't a moat.

**Durability**: High when the data is proprietary and the feedback loop is tight. Low when the data is commoditized or can be synthetically generated.

### 2. Network Effect Moats
**Definition**: The product becomes more valuable as more people use it.

**Types**:
- **Direct**: More users → more value for each user (messaging, social)
- **Indirect**: More users on one side → more value for the other side (marketplace)
- **Data network effects**: More users → better data → better product for all (overlaps with data moats)
- **Protocol network effects**: More adopters → more integrations → more adopters (identity networks, crypto protocols)

**Strength indicators**:
- Value increase per new user is measurable
- Users actively recruit other users (inherent, not incentivized)
- Leaving means losing access to the network
- Local network effects exist (small groups can get value before mass adoption)

**Critical mass**: Most network effects have a threshold below which they don't work. Identify yours. Below threshold, you're pushing a boulder uphill. Above it, gravity helps.

**Common mistake**: Claiming network effects when the product is just "better with more users" in a generic sense. True network effects mean the product is *useless or significantly worse* without the network.

### 3. Switching Cost Moats
**Definition**: The cost (time, money, risk, effort) of moving to a competitor is high enough to prevent switching.

**Types of switching costs**:
- **Data migration**: Moving data is painful (databases, CRMs, identity stores)
- **Workflow integration**: Product is embedded in daily processes (auth systems, CI/CD)
- **API integration**: Developers have built on your API; rewriting is expensive
- **Learning curve**: Team has invested in learning your product (design tools, dev platforms)
- **Contractual**: Long-term agreements, data retention requirements

**Strength indicators**:
- Customers complain about your product but don't leave
- Competitors offer free migration and customers still don't switch
- Integration depth increases over time (more API calls, more data, more workflows)

**Common mistake**: Confusing lock-in with switching costs. Lock-in is adversarial; switching costs should be a natural byproduct of the product being deeply useful. The best switching costs are the ones customers create voluntarily by going deeper.

**Durability**: High when switching costs are structural (data gravity, API integration). Low when switching costs are artificial (contractual lock-in — customers resent this and will leave when they can).

### 4. Distribution Moats
**Definition**: You have access to customers through channels that competitors cannot easily replicate.

**Types**:
- **Owned channel**: Direct relationship with users (email list, installed base, app presence)
- **Partnership channel**: Exclusive or preferred distribution through partners
- **Platform position**: Embedded in a platform's ecosystem (Shopify app, Salesforce integration)
- **Regulatory channel**: Government mandate or regulatory requirement that drives adoption
- **Community channel**: Organic community that drives awareness and trust

**Strength indicators**:
- CAC is significantly lower than competitors' CAC
- New products can be distributed to existing base cheaply
- Channel is defensible (exclusive partnership, owned audience, regulatory moat)
- Channel quality is high (right users, not just volume)

**Common mistake**: Treating paid advertising as a distribution moat. Anyone can buy ads. A distribution moat is a channel that's structurally cheaper or better for you than for competitors.

### 5. Technical Moats
**Definition**: The product requires genuinely hard technical capabilities that competitors cannot easily replicate.

**Strength indicators**:
- The core technology took years to develop
- Key expertise is held by a small number of people
- Published research or patents protect the approach
- Competitors' attempts to replicate produce measurably worse results
- The technical advantage compounds (each improvement makes the next one easier)

**Domain applications**:
- AI: custom model architectures, unique training approaches, proprietary inference optimizations
- Crypto: novel consensus mechanisms, ZK proof systems, cross-chain protocols
- Identity: privacy-preserving verification systems, biometric matching algorithms
- Auth: hardware-backed security, side-channel resistance

**Common mistake**: Claiming a technical moat for implementing well-known technology well. "We built a good API" is not a technical moat. "We developed a novel ZK circuit that verifies identity attributes 100x faster than existing approaches" might be.

**Durability**: Medium. Technical advantages erode as knowledge spreads, papers are published, and talent moves. The best technical moats are ones where the advantage compounds — each improvement makes the next one possible.

### 6. Brand & Trust Moats
**Definition**: Users choose you because of accumulated trust, reputation, and credibility that competitors cannot quickly build.

**Strength indicators**:
- Users choose you at a premium over cheaper alternatives
- "Nobody got fired for choosing [your product]"
- Trust is a prerequisite in your domain (identity, auth, financial, healthcare)
- Brand drives inbound (users seek you out specifically)
- Recovery from incidents doesn't destroy the brand (deep trust reservoir)

**Domain applications**:
- Identity/Auth: trust is the product. A security breach destroys the moat entirely.
- Crypto: community trust in the team and protocol. Rug-pull risk makes trust extremely valuable.
- Developer platforms: developer trust (don't break APIs, be transparent about pricing, respond to issues)

**Common mistake**: Confusing awareness with trust. Being known is not a moat. Being trusted is.

**Durability**: Very high when earned over years. Very fragile — a single trust-breaking event (data breach, rug pull, broken backward compatibility) can destroy it overnight.

---

## Assessment Rubric

Score each moat type 0-5 for your product:

| Score | Meaning |
|-------|---------|
| **0** | Not present |
| **1** | Theoretical — could exist but doesn't yet |
| **2** | Nascent — early signs but not yet defensible |
| **3** | Meaningful — provides real competitive advantage |
| **4** | Strong — would take competitors 1-2 years to replicate |
| **5** | Dominant — structural advantage that's nearly impossible to replicate |

### Assessment Template

```
## Moat Assessment: [Product/Feature]

### Data Moat: [0-5]
- Feedback loop: [describe or "none"]
- Data uniqueness: [proprietary / semi-unique / commoditized]
- Compounding: [yes/no — does more data = disproportionately better?]

### Network Effects: [0-5]
- Type: [direct / indirect / data / protocol]
- Critical mass reached: [yes/no]
- Local network effects: [yes/no — can small groups get value?]

### Switching Costs: [0-5]
- Primary switching cost: [data / workflow / API / learning / contractual]
- Customer-created: [yes/no — did they create the lock-in voluntarily?]
- Growing over time: [yes/no — does switching cost increase with usage?]

### Distribution: [0-5]
- Primary channel: [describe]
- CAC advantage: [quantify if possible]
- Channel defensibility: [exclusive / preferred / commoditized]

### Technical: [0-5]
- Core technical advantage: [describe]
- Replication difficulty: [months / years / near-impossible]
- Compounding: [yes/no — does the advantage grow?]

### Brand/Trust: [0-5]
- Trust level: [unknown / emerging / established / authoritative]
- Domain trust premium: [high / medium / low — how much does trust matter in this domain?]
- Fragility: [high / medium / low — how easily could trust be broken?]

### Overall Moat Score: [sum / 30]
- Primary moat: [strongest type]
- Moat gaps: [weakest types that matter for this market]
- Moat building priority: [what to invest in to strengthen defensibility]
```

---

## Domain-Specific Moat Patterns

### AI Products
**Strongest moats**: Data (usage-driven improvement), Technical (if truly novel), Switching costs (API integration)
**Weakest moats**: Brand (moves fast, new entrants constantly), Network effects (most AI products are single-user)
**Key risk**: Foundation model provider adds your feature natively. Your moat must exist ABOVE the model layer.
**Strategy**: Build the data flywheel early. Your unique data is the one thing the model provider doesn't have.

### Identity / Proof-of-Human
**Strongest moats**: Network effects (more verifiers = more valuable credentials), Brand/Trust (trust IS the product), Distribution (regulatory mandates)
**Weakest moats**: Technical (approaches converge), Switching costs (credential portability reduces these)
**Key risk**: Government-issued digital identity makes private solutions redundant.
**Strategy**: Build the network first. Be the standard, not a vendor. Interoperability is a feature, not a bug (it expands the network).

### Crypto / Web3
**Strongest moats**: Network effects (liquidity, composability), Community (brand/trust in crypto context), Technical (novel cryptography)
**Weakest moats**: Switching costs (portability is a feature in crypto), Distribution (crypto distribution is commoditized via exchanges/aggregators)
**Key risk**: Fork risk. If your code is open-source, anyone can copy the technology. The moat is the community and network, not the code.
**Strategy**: Build community and liquidity first. Protocol-level network effects (composability with other protocols) are the strongest moat in crypto.

### Developer Platforms
**Strongest moats**: Switching costs (API integration), Data (usage analytics, performance baselines), Distribution (ecosystem/marketplace)
**Weakest moats**: Brand (developers are pragmatic, will switch for better tech), Technical (good APIs are table stakes)
**Key risk**: Developers will pay for value but will leave instantly if you break trust (pricing surprises, API instability, poor documentation)
**Strategy**: Maximize integration depth. The more developers build on your platform, the harder it is to leave. Never break backward compatibility without a very good reason and a very long migration path.

---

## Moat Building Playbook

### Phase 1: Pre-PMF (No Moat Yet)
- Don't worry about moats. Focus on solving the problem.
- Choose a wedge that naturally leads to moat-building (e.g., start with an API that creates switching costs, not a UI that doesn't)
- Collect data from day one, even if you don't use it yet
- Build trust through transparency and reliability

### Phase 2: Early PMF (Plant Moat Seeds)
- Identify which moat type is most natural for your product
- Invest in the feedback loops (data → product improvement, users → more users)
- Start building switching costs through integration depth
- Be excellent at customer support (brand/trust moat building)

### Phase 3: Growth (Strengthen and Stack)
- Double down on your primary moat
- Add a second moat type (moats are strongest when stacked)
- Make the moat visible to investors and customers (it's a selling point)
- Monitor moat decay signals (see below)

### Phase 4: Scale (Defend and Extend)
- Extend the moat to adjacent markets
- Use distribution moat to launch new products
- Invest in technical moats that compound
- Build brand moat through category leadership

---

## Moat Decay Patterns

Every moat decays. Know the signs.

| Moat Type | Decay Signal | What's Happening |
|-----------|-------------|-----------------|
| Data | Competitors reaching feature parity without your data | Synthetic data, public datasets, or good-enough proxies |
| Network | User growth slowing while competitor networks grow | Multi-homing (users on multiple networks) or niche competition |
| Switching | Customers migrating despite costs | Switching cost < pain of staying. Competitor offering migration tools. |
| Distribution | CAC rising while growth slows | Channel saturation or competitors entering same channels |
| Technical | Feature parity from competitors | Knowledge diffusion, talent poaching, open-source alternatives |
| Brand/Trust | Price sensitivity increasing | Brand premium eroding; product becoming commodity |

### Response to Moat Decay
1. **Acknowledge it early.** Moats decay slowly, then suddenly.
2. **Stack another moat.** If your data moat weakens, strengthen switching costs or network effects.
3. **Move upmarket.** Commoditization at the bottom pushes value to higher-order problems.
4. **Redefine the category.** If the moat is decaying because the market is maturing, create a new market.
