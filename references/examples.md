# Example Library

Compact worked examples anchoring the quality bar for each major mode. These illustrate the expected voice, density, and specificity — not templates to follow literally.

---

## Example: Default Evaluation

**Input:** "We're thinking about building a marketplace for freelance designers."

> **Verdict first:** Crowded space with durable demand — the question isn't whether to build, it's what you'll be the best at for a specific segment.
>
> **The economic problem with two-sided marketplaces at this stage:** CAC is double because you're acquiring both sides. Margins compress as the platform matures because designers go direct once they have relationships. Your unit economics must account for this leakage from day one.
>
> **The strategic question you haven't answered:** What prevents the top 10% of designers from leaving after 6 months? Retention asymmetry kills marketplaces — the best supply leaves first. What's your supply-side moat?
>
> **The wedge that works here:** Own one vertical deeply before going horizontal. Figma-adjacent workflows, pitch deck designers, e-commerce creative — pick one and be the best address in the world for that job, then expand.
>
> **Biggest timing risk:** AI-generated design is compressing the low end of this market fast. The freelancer you're displacing in 24 months is the one you're acquiring today. Build for the high end, where human judgment is irreplaceable.

---

## Example: `ceo` Mode

**Input:** `product-strategy ceo Should we expand into Europe before hitting $10M ARR in the US?`

> **My read:** No — and the instinct to expand is usually a distraction from fixing something at home.
>
> **Why founders do this too early:** Europe feels like diversification. It's actually complexity. You're adding regulatory overhead (GDPR compliance, VAT, employment law), a second sales motion, potential currency exposure, and management attention — before the US motion is proven at scale.
>
> **The test:** If you can't answer "why are we at $X ARR not $3X ARR in the US?" before the board asks, you don't have a solved GTM. Europe won't fix that — it'll hide it for two quarters and then amplify it.
>
> **The exception:** If a large European enterprise is actively pulling you in with a committed contract, that's inbound demand worth following. Opportunistic expansion on a signed deal is different from proactive expansion as strategy.
>
> **Recommendation:** Stay US-focused until $15M ARR minimum, with a clear path to $30M in sight. Use the next 18 months to prove repeatability and reduce CAC. Then expand with a dedicated regional hire, not with bandwidth borrowed from the US team.

---

## Example: `eng-brief` Mode

**Input:** `product-strategy eng-brief We want to build real-time collaboration features into our product`

> **What this actually is:** Operational transformation (OT) or CRDT-based state sync, presence indicators, conflict resolution logic, and — often overlooked — the backend infrastructure to fan out updates at low latency across connected clients.
>
> **Why it's harder than it looks:** Collaborative editing is one of the most studied hard problems in distributed systems. Google Docs took years to get right. The surface area is: cursor/presence sync, merge conflict resolution, undo/redo across multiple actors, and offline/reconnection state. Any of these done wrong produces corrupted state that users blame on data loss.
>
> **What to evaluate before committing:**
> - Is collaboration a core workflow or a nice-to-have? If users don't share documents today, adding collaboration won't change behavior.
> - What's the conflict frequency? If 95% of sessions are single-user, the engineering cost may exceed the value.
> - What's the latency requirement? Sub-100ms feels real-time; 200ms+ feels laggy. This has infrastructure implications.
>
> **Recommended path:** Use an existing library (Yjs, Automerge, ShareDB) rather than building OT/CRDT from scratch. Define the minimum collaboration surface — start with presence + last-write-wins, not full OT — and ship that. Measure usage before adding conflict resolution complexity.

---

## Example: `red-team` Mode

**Input:** `product-strategy red-team We're launching a B2B SaaS tool for HR teams to automate compliance reporting`

> **Strongest Kill Argument:** Enterprise HR buyers are the slowest-moving buyers in software. Compliance is high-stakes and low-risk-tolerance — they will not adopt a new tool from an unproven vendor for something with legal exposure. Sales cycles will be 9–18 months, and you'll lose deals not to competitors but to "we'll wait another quarter."
>
> **Hidden Fragility:** The value proposition depends on regulatory requirements being complex and manual. The moment compliance frameworks simplify (possible with AI-generated compliance tooling from incumbents like Workday or SAP), the wedge disappears. You're building a moat in a place where the walls may move.
>
> **Competitor Exploit:** ADP, Workday, and Rippling all have distribution to this exact buyer. They don't need to build a better product — they need to add a compliance module and bundle it for free. Your likely response window before a credible bundled competitor emerges is 18–24 months.
>
> **Red-Team Verdict:** Fragile. The market is real, the pain is real, but the distribution and adoption dynamics are hostile for an early-stage company. If you proceed: the first 12 months must prove a ≤6-month enterprise sales cycle with at least 5 reference customers who've completed a full compliance cycle on the platform. If you can't show that, don't raise the Series A.

---

## Example: `narrative` Mode

**Input:** `product-strategy narrative Posthog — product analytics built for engineers`

> **Status Quo Frame:** Analytics tools were built for PMs and data analysts. They required separate teams to instrument, query, and interpret data. Engineers were consumers of reports, not participants in the insight loop. Every time an engineer wanted to understand user behavior, they filed a ticket.
>
> **The Insight:** The people closest to the code are the people best positioned to ask precise questions about product behavior — but they were systematically excluded from the analytics workflow by tools that required SQL skills, separate infrastructure, and a data team intermediary.
>
> **Category Claim:** PostHog is product analytics that engineers own end-to-end — deployed in your infrastructure, queried in code, and integrated into the development workflow.
>
> **Narrative Arc:** Engineers build the product. They should be able to understand it. PostHog gives engineering teams full control over their product data — event capture, session replay, feature flags, and A/B testing — without routing every question through a BI team. It's the difference between watching a dashboard and understanding your product.
>
> **Narrative Verdict:** Ownable. "Built for engineers" is a genuine category claim that major incumbents (Amplitude, Mixpanel) cannot credibly copy without alienating their existing buyer base. The risk is that "engineers" is a means to an end for most companies — if the PM/data analyst eventually needs ownership, PostHog must answer that question.

---

## Example: `launch-os` Mode

**Input:** `product-strategy launch-os Launching a new AI-powered contract review feature for our legal SaaS`

> **Phase Readiness Summary:**
> | Phase | Status | Confidence | Critical Gap |
> |-------|--------|-----------|-------------|
> | 1. Concept | GO | High | None — validated with 12 design partners |
> | 2. Product | AT RISK | Medium | Edge cases in multi-jurisdiction contracts untested |
> | 3. Whole-product | NO-GO | Low | Pricing not set; no onboarding flow for new buyers |
> | 4. Narrative | AT RISK | Medium | "AI-powered" framing triggers legal buyer skepticism — needs reframing |
> | 5. GTM | AT RISK | Medium | No channel strategy beyond existing customer base |
> | 6. Support/Ops | GO | High | Legal team briefed; escalation path defined |
> | 7. Go/No-Go | NO-GO | — | — |
> | 8. Post-launch | AT RISK | Medium | Week 1 learning plan exists; no named owner |
>
> **Go/No-Go Recommendation:** NO-GO in current state. Whole-product gaps (pricing, onboarding) and narrative risk are launch-killers for a legal buyer — this segment has zero tolerance for "figure it out as you go." Recommend a 4-week remediation sprint targeting Phase 3 and Phase 4 before setting a new launch date. Do not announce externally until Phase 3 is GO.

---

## Example: `blue-ocean` Mode

**Input:** `product-strategy blue-ocean AI coding assistant space — where's the actual whitespace?`

> **The consensus play no one should build:** A better Copilot. That race is over for new entrants — GitHub, Cursor, and JetBrains have distribution you can't match. The question isn't "how do we compete in autocomplete" — it's "what workflow does autocomplete not solve?"
>
> **The whitespace:** Code review as a professional discipline. The AI-generated code explosion means more code, lower quality, faster — and the review bottleneck is becoming the constraint. Every mid-size engineering team will have a review backlog problem within 18 months. No one owns this at the level of "the tool senior engineers actually reach for."
>
> **The category claim:** Not "AI code review" (sounds like a feature). "Institutional code knowledge" — the system that knows your conventions, your architecture decisions, and your tech debt map, and uses that to review every PR like your best senior engineer would.
>
> **Why this compounds:** Every review trains the system on your codebase. Day 1 value is moderate; Month 6 value is high. That's a switching cost moat and a retention moat simultaneously.
>
> **The wedge:** Mid-size engineering teams (50–200 engineers) who've adopted AI-generated code and are experiencing review bottlenecks. Not enterprise (too slow to buy), not startups (no review culture yet).

---

## Example: `sequence` Mode

**Input:** `product-strategy sequence We have 6 initiatives: SSO, mobile app, API v2, marketplace, analytics dashboard, white-labeling`

> **Before I rank:** Two things I need to know — are you pre-PMF or post-PMF? And who are the 10 customers you'd be devastated to lose?
>
> **Provisional sequence (assuming B2B SaaS with existing revenue):**
>
> **P0 — API v2:** Everything downstream depends on a stable API. Building mobile, marketplace, and white-labeling on a v1 you're planning to deprecate is building on sand. Pay this debt now.
>
> **P1 — SSO:** Enterprise deals gate on this. If even one top-10 account has flagged SSO as a blocker, it becomes P0. SSO unlocks budget — it's an economic-truth decision disguised as a features decision.
>
> **P2 — Analytics dashboard:** This is the proof-of-value mechanism. Customers who see ROI renew at higher rates and expand. Ship this before white-labeling — internal value visibility before partner distribution.
>
> **P3 — Mobile app:** Only if your core workflow is mobile-native. If it isn't, this is a distraction. Validate that mobile users are blocked, not just mildly preferred.
>
> **Deprioritized — Marketplace + White-labeling:** Both require product maturity and customer density you probably don't have yet. Marketplace without network density is a graveyard. White-labeling before the core product is solid creates a support nightmare.
>
> **The sequencing principle:** Unlock revenue before building distribution. Build distribution before building ecosystem.

---

## Example: `discovery` Mode

**Input:** `product-strategy discovery Building AI-powered onboarding for enterprise SaaS — what are our riskiest assumptions?`

> **Assumption #1 (highest risk):** Enterprise buyers will allow AI to handle onboarding conversations with their employees.
> Why risky: HR and IT security teams have explicit policies about external AI data flows. "AI-powered" triggers a procurement review that takes 3–6 months in most enterprises. The product may be great and still stall in security review.
> MVE: 5 discovery calls specifically with enterprise IT security teams — not HR buyers. Ask about AI vendor requirements, data residency, and procurement gates. Kill signal: any 3 of 5 requiring an on-premise option.
>
> **Assumption #2 (high risk):** Employees will engage with AI-powered onboarding rather than ignoring it.
> Why risky: Onboarding completion rates are low even with human-designed programs. Adding AI doesn't solve engagement — it may worsen it if employees associate AI with "the company cut corners."
> MVE: A/B test with 20 users — AI flow vs. static doc. Measure completion rate and Day 30 self-reported confidence. Kill signal: <40% completion on the AI flow.
>
> **Assumption #3 (medium risk):** HR teams will pay a premium for AI vs. their existing LMS.
> Why risky: Workday Learning, Docebo, and similar tools are deeply embedded. The question isn't "is this better?" — it's "is this so much better that it justifies migration?"
> MVE: Pricing conversation with 3 enterprise HR buyers after the demo. Ask for budget range before quoting. Kill signal: all three assume it's an add-on, not a replacement.

---

## Example: `premortem` Mode

**Input:** `product-strategy premortem We're betting on freemium as our primary growth motion`

> **Set the scene:** 18 months from now. The freemium motion has failed. Not "underperformed" — failed. 800,000 free users. 0.7% conversion to paid. Runway is 4 months.
>
> **Most likely cause of failure:** We optimized the free tier for adoption, not for the moment that makes people willing to pay. The paywall landed in the wrong place — it blocked features users wanted but not features core to their workflow. Users extracted value from free and left when they hit the wall. The free product accidentally taught users to work around our paid features.
>
> **Warning signs we ignored:** In month 4, free-tier DAU/MAU was 15% — healthy-looking activation metrics hiding weekly churn. We also saw that free users weren't sharing — the viral loop we assumed was never validated.
>
> **The assumption that was wrong:** We assumed free users who stayed past 30 days were qualified leads who'd eventually convert. In practice, the users who stayed were the ones who'd found a way to get permanent value from the free tier — the hardest converts, not the best leads.
>
> **To proceed safely:** Stress-test paywall placement against behavioral data from existing users: where does the product create a moment of "I need more of this"? That is the only valid paywall location. Validate with 20 free users before hardening the pricing architecture.

---

## Example: `advisory-roundtable` Mode

**Input:** `product-strategy advisory-roundtable Should we build our own auth layer or use Auth0?`
**Panel:** Collison · Lütke · Bezos

> **MODERATOR:** Build or buy auth. Running Collison first — he'll want to map the infrastructure decision before anything else.
>
> **Patrick Collison:** This is a one-way door question that most teams treat as a two-way door. Auth failure modes are the ones that damage user trust most severely — and you don't discover them on the happy path. Before evaluating build vs. buy, I want to know: have you mapped the full failure surface? Token expiration edge cases, SSO complexity, MFA, session invalidation at scale, device fingerprinting. If you haven't, you can't evaluate the build option honestly. What I'd tell most teams: the question isn't "can we build it?" — it's "do we want to own every failure mode in the auth surface forever?"
>
> **Tobi Lütke:** I'd build only if auth is the thing that differentiates you to the customer. For most products, it isn't. You're adding a maintenance burden that will slow every developer for years. Auth0 exists because auth is a solved problem that's genuinely complex to solve correctly. Ask instead: what can we ship in the time we'd spend on auth? That's the real opportunity cost.
>
> **Jeff Bezos:** Customer-backward: does the customer care who built the auth layer? Almost never. They care that login works, the account is secure, and they don't lose their data. Those are properties, not implementation decisions. If a partner satisfies the customer's requirements, the decision is obvious. Build only if you have a specific customer requirement that no available provider can meet. Write that requirement down first.
>
> **MODERATOR:** Early convergence: Lütke and Bezos push toward partner. Collison is conditional — map the failure surface first. Running a crossfire: Collison, does mapping the failure surface actually change the recommendation for most teams, or is it due diligence that arrives at the same answer?
>
> **Session controls →** `continue` · `verdict` · `focus [Collison]` · `crossfire [Collison vs Lütke]`

---

## Example: `eng-translate` Mode

**Input:** `product-strategy eng-translate "We need to rewrite the auth service from scratch — the current implementation doesn't scale past 10k concurrent sessions and has fundamental JWT design flaws."`

> **What engineering is actually telling you:** This isn't primarily a performance complaint. "Fundamental JWT design flaws" means the auth architecture made decisions that are expensive to change — probably around statelessness, token expiry, or session revocation that can't be patched incrementally. They're telling you the refactor risk is already embedded in the current system, not in the rewrite.
>
> **The hidden product risk:** Auth is a trust surface. If JWT design flaws include session persistence issues or revocation gaps, you have latent security exposure right now — not just a future scaling problem. That changes urgency.
>
> **What "doesn't scale past 10k concurrent sessions" actually means:** For most products, 10k concurrent sessions is a ceiling you'll hit at scale. If you're approaching it sooner than expected, something about usage pattern is different from your model — that's worth diagnosing before designing the replacement.
>
> **What product/leadership should do now:** Separate the security question from the scaling question. Security issues are P0 — they warrant emergency prioritization regardless of roadmap. Scaling issues are P1 — real but plannable. Ask engineering to document: (1) is there active security exposure today, and (2) what's the estimated timeline to hit the scaling ceiling at current growth? Those two answers determine the urgency level and the correct response.
>
> **What engineering needs from product:** Authorization to scope the rewrite properly — this isn't a sprint task. They need resource commitment and a feature freeze window on auth-adjacent work. If you can't give them that, the rewrite will be done around live features and the risk doubles.

---

## Example: `investor-story` Mode

**Input:** `product-strategy investor-story We're raising a $4M seed for Verified — a proof-of-human layer for AI-generated content. Target: Sequoia and a16z.`

> **The pitch that works for both firms:** Start with the insight, not the problem. The insight is: every major AI lab just trained billions of people to produce cheap synthetic content — and simultaneously destroyed the trust layer the internet was built on. Verified is infrastructure for the world that just became real.
>
> **Sequoia lens:** They want to know if this becomes a platform. Proof-of-human is a feature unless you can articulate the stack above it — credentials, reputation scores, access gates, regulatory compliance. Lead with the infrastructure thesis, not the content-verification use case.
>
> **a16z lens:** They'll fund the category thesis if you can frame this as the identity primitive for the AI era. They'll probe: "Why can't this be done by existing identity providers?" Your answer must be specific — not "we're focused on it" but a technical or trust-model reason why Auth0 can't solve this.
>
> **The hardest question from both firms:** Unit economics on verification. What does it cost to verify one human? At what scale does it become margin-positive? If you can't answer this, they'll assume the economics don't work.
>
> **Recommended arc:** Hook — "We just crossed the threshold where synthetic humans are indistinguishable from real ones online." Problem — one specific scenario (fake DAO votes, AI-written academic work, synthetic influencer accounts). Why now — the model capability step-change happened 18 months ago; the infrastructure response is 18 months late. Solution — Verified as the ZK-native trust layer. Ask — $4M to prove verification cost economics at 1M users.
>
> **Gap to close before the meeting:** You need a specific answer to fork risk (if this is a protocol) or a defensible technical moat reason (if this is a product). "First-mover" will not land with either firm.

---

## Example: `gtm` Mode

**Input:** `product-strategy gtm ZK-based identity verification SDK for developers building consumer apps that need age verification without storing PII.`

> **ICP in one sentence:** Backend engineers at consumer apps (fintech, social, gaming) who need to ship age verification in under a sprint and can't afford a data liability surface.
>
> **The beachhead:** Fintech apps under regulatory pressure — they have a hard legal deadline, low tolerance for custom builds, and engineering teams who will champion a clean SDK over a vendor with a legal procurement process. This segment converts fast when you solve their actual problem (compliance + no PII storage).
>
> **Motion: bottoms-up developer adoption → compliance mandate upsell.** Developer finds SDK via docs/GitHub, integrates in a day, ships. Compliance team later asks "what are we using for age verification?" and discovers it's already in production. Upsell path: audit logs, batch verification API, enterprise SLA.
>
> **The message that converts:** "Ship age verification in one day. No PII stored. SOC 2 compliant." Every word does work. Don't add anything to this headline until you've validated it converts.
>
> **What will kill this GTM:** Starting with enterprise sales. You don't have the proof points yet and the sales cycle is 6–9 months. Seed the market bottoms-up. Treat enterprise as a year-2 motion.
>
> **First 90-day plan:** 1. Publish SDK on GitHub with a working demo. 2. Get 3 fintech teams to integrate and share feedback publicly. 3. Sponsor one developer newsletter with a specific technical problem statement. Measure: time-to-first-integration and integration completion rate. Don't optimize for signups.

---

## Example: `postmortem` Mode

**Input:** `product-strategy postmortem Our enterprise sales motion. We spent 8 months building for enterprise but only closed 2 deals at $18K ACV. We burned $400K and a significant portion of our team's focus.`

> **Root cause (not the surface cause):** The enterprise motion failed because we confused "interest" with "demand." Every enterprise conversation generated enthusiasm and requirements lists — but no one had budget authority, and no one was in enough pain to drive a fast procurement cycle. We were solving a nice-to-have, not a burning problem.
>
> **The decision that locked us in:** We committed to enterprise infrastructure (SOC 2, audit logs, SSO, legal review process) before we had a single signed contract. That sequence is backwards. Infrastructure investment before revenue confirmation is a bet — we made it without naming it as a bet.
>
> **What the signals said (that we misread):** In month 3, average deal cycle was already 90 days with no signed contract. In month 5, we had 8 "active opportunities" and none had reached the procurement stage. We called this pipeline — it was actually a waiting room.
>
> **What to carry forward:** The 2 deals that closed both came through warm intros from technical champions who had prior experience with ZK-based systems. What do those 2 buyers have in common that our other prospects don't?
>
> **Decision for next 90 days:** Before re-entering enterprise, answer: who has a burning compliance or trust problem right now, with budget released, and a procurement cycle under 45 days? If you can't name 5 specific companies and the executive who owns the budget, don't restart the enterprise motion yet.

---

## Example: `org-design` Mode

**Input:** `product-strategy org-design We're 12 people, shipping fast, but product and engineering are starting to step on each other. Nothing is blocked but everything feels a little slow.`

> **What's actually happening:** At 12 people, "feels slow" is almost always a decision-ownership problem, not a process problem. Someone — or multiple someones — is waiting for permission before moving. That delay compounds across 10 decisions per day and manifests as friction everywhere.
>
> **The 12-person org anti-pattern:** At this stage, teams often have one fast-moving layer (engineers shipping) and one bottleneck layer (product or leadership reviewing everything before it moves). The bottleneck doesn't feel like a bottleneck to the person causing it — it feels like quality control.
>
> **What to change:** Assign explicit decision rights for the 5 most common decision types in your sprint cycle. For each: who decides, who's consulted, who's informed. Write it down. A decision rights matrix doesn't have to be formal — a Notion page is fine. The act of writing it surfaces the ambiguity that's causing the drag.
>
> **What not to do:** Don't add process (standups, syncs, Jira tickets) to solve a decision-rights problem. Process is coordination overhead — it helps when people know what they're doing but need to stay aligned. It doesn't help when people don't know who's allowed to decide.
>
> **For your next sprint:** Identify the last 3 things that were "waiting" and trace what they were waiting for. If the answer is "approval from [person]" — that's your bottleneck. If the answer is "clarity on direction" — that's an ownership gap. Those have different fixes.

---

## Example: `board-memo` Mode

**Input:** `product-strategy board-memo Q1 update — missed ARR target by 22%, but enterprise pipeline is strong and we've identified the root cause as a pricing packaging issue we've since fixed.`

> **Frame before the numbers:** A board memo is not a performance report. It's a strategic argument. Lead with the interpretation, not the data. "We missed Q1 ARR by 22%" is not an opening — it's a line item. The opening is: "We discovered our pricing architecture was misaligned with how enterprise buyers evaluate value. We've corrected it. Here's the evidence it's working."
>
> **The structure that builds confidence:** (1) What happened and why — one paragraph, direct, no hedging. (2) What we now know that we didn't know — the learning. (3) What we changed — the specific intervention. (4) Early evidence it's working — even a leading indicator. (5) What we're watching — the metric that will confirm or deny the thesis over the next 60 days.
>
> **What board members are actually asking:** "Do they understand their business?" Missing a target is not a failure signal to experienced investors — it happens. The failure signal is not knowing why, or knowing why but not having changed anything. Your memo needs to answer: do you have the mechanism?
>
> **The pipeline point:** "Enterprise pipeline is strong" is not a board-level claim without specifics. State: number of active opportunities, average deal size, expected close timeline, and one concrete reason to believe close rate will improve vs. Q1.
>
> **Tone calibration:** Direct, not defensive. Own the miss in the first sentence. Then pivot immediately to what you learned and why the next 90 days look different. Boards lose confidence not from bad quarters but from founders who seem surprised by bad quarters.

---

## Example: `board-story` Mode

**Input:** `product-strategy board-story Q2 board meeting — we're 18% ahead of ARR plan, strong NRR, but we're about to ask for approval to pause the enterprise motion and double down on PLG. That's our big ask.`

> **Board deck narrative spine — before any slides:**
> [1] We're ahead of plan (headline: ARR +18% vs. budget) — open with this. The board needs to hear that management is in control before they'll engage on strategy.
> [2] What changed: NRR improved, but enterprise pipeline velocity dropped. Two things are happening simultaneously — good news and a strategic fork. Name both explicitly.
> [3] What we're focused on: the PLG motion is showing early pull. Define "pull" precisely — retention rate, time-to-value, activation curve — so "early pull" isn't vague.
> [4] The decision: pausing enterprise isn't a retreat, it's a sequencing call. You need the board aligned on this before you execute — not informed after the fact.
> [5] The risk we're surfacing: enterprise pipeline has 3 late-stage deals. Pausing the motion mid-cycle without a handoff plan creates churn risk. Tell the board this before they ask.
> [6] What you'll show next meeting: PLG activation rate at 60-day mark + one enterprise deal that converted through PLG self-serve.
>
> **The most dangerous slide in this deck:** Slide 5 (Key Strategic Decisions). This is where the board will push hardest — "If enterprise was the Series A story, what's the Series A story now?" You need a precise answer before you walk in: PLG + upmarket expansion, once the product earns the right through retained usage. Have that sentence ready.
>
> **Pre-emptive Q&A — what the Lead VC will ask:** "The last time you presented this board with the enterprise plan, you had [X] in committed pipeline. What happened to those deals?" Answer: [status of each deal]. Be specific. Vague pipeline answers in board meetings become 20-minute discussions.
>
> **Context gap to fill before finalizing materials:** What is your runway at current burn? The board will recalculate this the moment you announce a motion change — they need to know you've done the math first.

---

## Example: `boardroom` Mode

**Input:** `product-strategy boardroom We have a board meeting next week. We're asking the board to approve pausing the enterprise motion and refocusing on PLG. We have a Lead VC (Sequoia), a Financial VC (growth equity firm), and an independent director who's a former enterprise sales VP.`

> *Setting up the room. Lead VC: Sequoia partner — governance-first, will frame the strategic question before anyone else. Financial VC: growth equity — will go directly to the model. Independent Director: former enterprise sales VP — this decision lands squarely in their domain expertise. Beginning with Lead VC calling the meeting to order.*

> **Lead VC:** "Thanks for the update — and congratulations on the ARR number, that's a good quarter. I want to hold the enterprise motion question until we've all had a chance to ask questions on the financial slide, but I'll say upfront: we backed this company on an enterprise thesis. Before we vote on anything, I want to understand whether we're changing the thesis or resequencing toward it. Those are very different decisions. Can you start by walking us through what specifically changed in enterprise velocity over the last 90 days?"

> *[Pause — user responds as CEO/founder]*

> **Independent Director** *(after CEO responds):* "I want to add something here. I ran an enterprise sales team for eight years. What you're describing — pipeline that doesn't move — is almost never about the product. It's usually about who owns the budget and whether you have a champion with authority. Before I can support pausing the motion, I want to understand: did you have economic buyers in these deals, or were you working with technical evaluators who couldn't sign? That changes my read on whether this is a product-market fit issue or a sales execution issue."

> **Financial VC:** "I'll be direct — I want to see the model before we discuss this further. What does the PLG growth curve look like at $1M, $3M, $5M ARR? And what's the NRR assumption once you remove enterprise expansion revenue? The enterprise upmarket was your NRR story. I need to understand what replaces it."

> *The room is engaged — Lead VC is open but wants thesis clarity, Independent Director is skeptical of the diagnosis, Financial VC is running the exit math. Your most important sentence right now: "We're not changing the enterprise thesis — we're changing the sequence." Lead with that.*

---

## Example: `investor-roundtable` Mode

**Input:** `product-strategy investor-roundtable We're building onchain identity infrastructure — a protocol layer that lets users carry verified credentials across Web3 apps without re-KYCing on every platform. Seed stage, targeting $3M raise.`

> *Setting up Tier 1 universal panel (always active): Company Builder · Category Maker · Numbers Analyst · User Validator · Contrarian*
>
> *Asking domain question before proceeding:*
> **"Would you like to add domain-specialist investors to the panel?**
> **A) Crypto/Web3 specialists** — network design, ecosystem pull, crypto-native demand, on-chain incentives
> **B) Enterprise/B2B SaaS specialists** — procurement cycles, land-and-expand, compliance
> **C) Consumer specialists** — retention loops, viral mechanics, subscription economics
> **D) Deep Tech/Hard Tech specialists** — technical defensibility, regulatory landscape
> **E) AI Products specialists** — inference economics, workflow vs. model moat
> **F) No — run with the universal panel only**"*

> *[User selects A — Crypto/Web3 panel added: Ecosystem Investor · Network Designer · Frontier Bettor]*

> **Company Builder (Round 1):** "Before we get into the identity protocol specifically — I want to understand the company. Identity infrastructure sounds like developer tooling. Developer tooling at the protocol layer almost never monetizes cleanly. The incumbents either acquire you or route around you. Walk me through the business model: who pays, how much, and why they can't build this themselves."

> **Network Designer (Round 1):** "The credential portability thesis is sound, but I want to understand the coordination problem. For this to work, you need wallets to integrate your credential layer, apps to accept it, and users to go through KYC once somewhere. That's a multi-sided cold-start problem with a bootstrapping catch-22 — no apps without users, no users without apps. What's your wedge into one side of this without needing the other side first?"

> **User Validator (Round 1):** "Who is the first user who uses this next week? Not a DeFi app integrating it — an actual person who creates a credential, carries it somewhere, and uses it to skip something painful. I need to visualize that behavior specifically. If you can't name the behavior in one sentence, the product isn't ready to validate yet."

> **Contrarian (Round 1):** "Here's my non-consensus view: the reason Web3 identity hasn't worked yet isn't technical — it's that users don't want portable credentials, they want the app they're already in to handle compliance invisibly. You might be building for the ideal version of Web3 that doesn't exist yet, not the one users are actually in. What behavior exists today that proves users feel the pain of re-KYC enough to change their workflow?"

> *Round 1 views in — moving to moderator synthesis before Round 2 pressure. The panel has surfaced three critical questions: (1) business model clarity, (2) cold-start bootstrapping strategy, (3) proof of user pull vs. developer pull. Round 2 will focus hardest on the network designer's bootstrapping question and the contrarian's behavior evidence challenge. Your response to those two will determine whether this stays at `explore_before_commit` or escalates to `conditional_yes`.*
