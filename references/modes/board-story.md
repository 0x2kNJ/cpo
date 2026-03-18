# Board Story Mode — Board Presentation Materials

## Purpose

Build the materials for a board presentation. Different from a board memo (which is a written update) and different from boardroom (which is the live meeting). `board-story` prepares the deck narrative, ensures the right context is surfaced, and makes sure you walk in with the right story — not just the right slides.

---

## What Boards Actually Care About

Before building any materials, understand the audience. Boards are not your team. They see you for 2 hours every 6–12 weeks. They are calibrating one question:

> **Is this management team in control of the business, and is the strategy correct?**

Everything they ask is a variant of that question. Shape your materials to answer it.

### What boards need to see

| Category | What they're asking |
|----------|---------------------|
| **Financial performance vs. plan** | Did you hit the numbers you promised? If not, why, and is management in control of the miss? |
| **Forward trajectory** | Is the business better or worse than 6 months ago? Is momentum building or eroding? |
| **Key strategic decisions** | What are you asking the board to approve, align on, or be aware of? |
| **Risk** | What could derail this? Are you aware of it, and do you have a plan? |
| **Team** | Is the team intact and capable? Any leadership changes, gaps, or succession issues? |
| **Capital / runway** | How long is the runway? Are you on track to the next milestone before you need more capital? |

---

## Pre-Boarding Context Collection

Before building materials, collect (via AskUserQuestion if not in context):

1. **What period does this board meeting cover?** (Q1, H1, full year, special session)
2. **What is the headline number?** (ARR, revenue, key metric vs. plan)
3. **Is performance ahead / in-line / behind plan?** (one word)
4. **What is the one most important decision or ask?** (what do you need the board to do, decide, or hear?)
5. **What is the current runway and burn rate?**
6. **What are the top 2 risks you want to surface proactively?**

If any are unknown and `--silent` is not set: STOP and ask. Materials built without these answers will be structurally wrong.

---

## Standard Board Presentation Structure

### Slide 1: Executive Summary
**What boards want:** The meeting in one page. If they read nothing else, this tells them performance, status, and the ask.

Structure:
- **Headline performance:** [Key metric] vs. plan — [ahead / in-line / behind] by [%]
- **Business momentum:** [One sentence — is the business accelerating, holding, or decelerating?]
- **Key strategic milestone since last meeting:** [What did we do that mattered?]
- **The ask:** [What do you need the board to do today?]
- **Runway:** [X months at current burn]

---

### Slide 2: Financial Performance vs. Plan

**What boards want:** Actuals vs. budget, explained honestly. No surprises, no spin.

Standard metrics (adapt to stage):
| Metric | Actual | Budget | Delta | Commentary |
|--------|--------|--------|-------|------------|
| ARR / Revenue | | | | |
| MRR growth | | | | |
| Gross margin | | | | |
| Burn / OpEx | | | | |
| Runway (months) | | | | |

**For each miss:** explain root cause in one line. Boards forgive misses; they don't forgive surprises or unexplained variances.

**Cash bridge:** Current cash → burn rate → runway → next milestone. This is non-negotiable at every board meeting.

---

### Slide 3: Commercial / Customer Update

**What boards want:** Is the commercial engine working? Are customers arriving, staying, and expanding?

Structure:
- **New business:** [# of new logos / ARR added] vs. plan
- **Churn / contraction:** [$ARR churned] — named accounts if significant
- **Net Dollar Retention (NDR):** [%] — trajectory direction
- **Pipeline:** [$X] at [stage], [X] deals > $[threshold]
- **Key wins:** [2-3 notable logos or use cases]
- **Key losses:** [1-2 honest losses with root cause — boards respect honesty here]

---

### Slide 4: Product & Technology Update

**What boards want:** Are we shipping? Is the product differentiated? Are we building the right things?

Structure:
- **Key milestones since last meeting:** [What shipped, in plain English]
- **What didn't ship (and why):** [Honesty here builds credibility]
- **Current roadmap focus:** [Next 90 days — 3 items max]
- **Technical debt or infrastructure risk:** [Flag proactively — don't let the board discover it]
- **AI/platform shift relevance:** [How does macro technology shift affect our product roadmap?]

---

### Slide 5: Key Strategic Decisions

**This is the most important slide.** It is the reason the board meeting exists.

Structure per decision:
```
Decision: [Clear one-sentence statement of what you're deciding]
Context: [Why this matters now — what's driving the timing]
Options: Bold / Balanced / Conservative
Recommendation: [What management recommends, with rationale]
Board input needed: [Approve / Align / Inform only]
Kill criteria: [What would make this wrong]
```

**Rule:** If there is no key decision to bring, the board will make one up. Bring your own decisions — it keeps the agenda yours.

---

### Slide 6: Team & Organizational Update

**What boards want:** Is the management team intact? Any surprises? Key hires in critical gaps?

Structure:
- **Leadership changes:** [New hires / departures at VP+ level]
- **Headcount vs. plan:** [Current HC vs. plan]
- **Key open roles:** [What are you trying to hire that you haven't yet?]
- **Culture signal:** [One honest data point — eNPS, Glassdoor trend, attrition rate]
- **Succession risk:** [Flag proactively if any key-person dependency exists]

---

### Slide 7: Risk Register

**What boards want:** Are you aware of what could go wrong? Do you have a plan?

Format: Top 3 risks, assessed on likelihood × impact

| Risk | Likelihood | Impact | Mitigation | Owner |
|------|-----------|--------|------------|-------|
| [Risk 1] | H/M/L | H/M/L | [One sentence] | [Name] |
| [Risk 2] | | | | |
| [Risk 3] | | | | |

**Rule:** One of these risks should be uncomfortable to share. If all three are safe to say, you're not being honest about the risk register.

---

### Slide 8: Forward Plan — 90 Days

**What boards want:** What are you building toward? How will they know if you're on track at the next meeting?

Structure:
- **Top 3 milestones for next 90 days** (measurable, not vague)
- **Key dependencies or risks to hitting them**
- **What the board will see at the next meeting** (pre-commit to your own accountability)

---

### Slide 9: The Ask

**What boards want:** Be direct. Don't bury the ask in slide 8.

Options:
- **Consent item:** [Equity grant / option pool / major contract / debt] — needs formal vote
- **Strategic alignment:** [We're making a major move — we need the board to be aligned before we proceed]
- **Resource request:** [We need to hire / spend outside budget — here's why]
- **Advisory input:** [We're facing a decision where the board's experience is directly relevant]
- **Notification only:** [No action needed — we want the board to know X before they hear it elsewhere]

---

## Board-Specific Narrative Principles

### 1. Performance before strategy
Never open with vision when performance is below plan. Address the number first. Boards will not hear the strategy until they understand the numbers.

### 2. The "in control" signal
Every slide should signal that management is in control — of the narrative, the numbers, and the risks. If a slide undermines this signal, reframe or remove it.

### 3. The honest lowlight
Include at least one genuine lowlight. Boards who see only highlights stop trusting the materials. One credible lowlight makes everything else more believable.

### 4. Pre-answer the hard questions
For every slide, ask: what will [Lead VC / Financial VC / Independent Director] ask? If you know the question, put the answer in the slide. Board members who feel their questions are pre-answered feel heard and trust the management team.

### 5. Benchmark against your own plan, not the market
Boards care about performance vs. your own commitments, not industry benchmarks. Lead with plan variance. Use market benchmarks only as context for misses.

---

## Board Deck Narrative Spine

A board deck is a story, not a report. The narrative arc:

```
[1] Here is where we are vs. where we said we'd be.
[2] Here is what changed since last time (better + worse).
[3] Here is what we are focused on and why.
[4] Here is the one decision we need to make together.
[5] Here is the risk we want you to know about.
[6] Here is what you'll see at the next meeting.
```

If the deck doesn't follow this arc, it will feel like a data dump and the board will fill the silence with their own agenda.

---

## Context Framing for New Board Members

If a board member is new or the board composition recently changed, include a **Company Context Slide** before the Executive Summary:

```
Company: [Name]
Founded: [Year]
Stage: [Pre-PMF / Post-PMF / Growth / Scale]
Business model: [B2B SaaS / marketplace / platform / etc.]
Core product: [One sentence]
Key metric: [What we measure most]
Last raise: [$X] at [$Y] valuation, [date], [lead investor]
Runway: [X months]
Headcount: [N]
```

This prevents experienced board members from re-explaining context to new ones during the meeting.

---

## Output Format

After collecting context, generate:

1. **Board Deck Narrative Spine** — the story arc in plain English before slides
2. **Slide-by-slide structure** — what goes in each slide with board-specific framing
3. **Context gap analysis** — what's missing that the board will ask about
4. **Pre-emptive Q&A** — top 5 questions each archetype will ask, with recommended answers
5. **The Moment of Truth** — the one slide or exchange most likely to determine how the meeting goes

---

## Kill Criteria

- If financial performance vs. plan is unknown, cannot build a credible board presentation — this is the foundation
- If there is no clear ask, flag: a board presentation without an ask is a status report; boards disengage
- If this is a crisis board meeting (unexpected call, governance issue, CEO/executive departure), switch to `boardroom` mode for live simulation — `board-story` is for planned presentations
