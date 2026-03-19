# (C)hief (P)roduct (O)fficer

**A human PM can hold a handful of decisions in working memory. CPO holds all of them** — with full context, cross-referenced assumptions, and mechanical decay tracking — and surfaces the one thing that matters most right now.

`/cpo` is the operating system for product decisions before execution. It helps founders, PMs, and executives decide **what to build, whether to build it, how to communicate it, and when to kill it** — before the team commits time, headcount, or capital.

It sits between **"we have an idea"** and **"engineering is building it."**

If this helps you make better product decisions, ⭐ the repo.

---

## The vision

CPO's end goal is to **orchestrate an entire product organization** — not just advise on one decision, but manage the full surface area of product judgment across time.

Today, every AI tool in the product ecosystem accelerates execution. CPO is the missing layer that accelerates *judgment*.

**Where CPO is today:** A strategic advisor that frames decisions across five dimensions, explores three paths before recommending one, defines kill criteria upfront, and logs everything in a persistent journal that compounds context over time. Every session starts smarter than the last.

**Where CPO is going:** A full product operating system where decisions flow automatically from strategic framing to execution handoff to outcome tracking — where the system monitors kill criteria countdowns across all active bets, detects when your team's implicit strategy has drifted from your explicit one, autonomously routes decisions to the right execution tools, and maintains a living model of your entire product portfolio's health. Not a better prompt — a product organization's nervous system.

---

## What makes CPO different

These capabilities require persistent memory, cross-decision computation, and automated integrity checks. They don't exist in traditional PM workflows — not because PMs lack skill, but because the mechanics are structurally impossible to maintain manually at scale.

### Your new strategy contradicts a decision you made 3 months ago — and CPO catches it before you ship
**Spec Coherence Validator.** Every time a decision is logged, CPO automatically checks it against all active decisions for contradictions, assumption collisions, and kill criteria conflicts — scoped to related topics, not false alarms across unrelated domains.

### The one decision blocking everything else — surfaced automatically
**Decision Dependency Graph (`--graph`).** CPO maps which decisions depend on which others, scores bottlenecks by fan-out, staleness, and confidence, and identifies the single decision whose resolution unblocks the most downstream work. Computed live from actual decision content — no spreadsheet to maintain.

### Every active kill criterion across every bet, with a countdown timer
**Kill Criterion Countdown (`--kills`).** Cross-cutting dashboard with days remaining, urgency classification (triggered / approaching / active / undated / external), and cross-reference with the dependency graph. When a bottleneck decision also has an approaching kill criterion, CPO surfaces the compounding risk.

### Your decisions generate predictions — and CPO checks them against reality
**Consequence Tracking.** At decision time, CPO auto-generates 2–3 falsifiable predictions about what should change if the recommended path succeeds. Each has a check date. When the date arrives, `--brief` surfaces the prediction for verification. Over time, this becomes a personal prediction market — a track record of whether your decision framework actually predicts outcomes.

### Before you judge a past decision, CPO shows you what you actually knew at the time
**Session Replay.** When you close the loop with `--outcome`, CPO reconstructs your information state at decision time — Truth fingerprint, open questions, paths considered, predictions made — before asking what happened. This prevents hindsight bias: the single most common failure mode in retrospectives.

### Assumptions rot silently. CPO makes the rot visible.
**Decision Decay Index.** Every active decision gets a mechanical decay score based on age, inferred assumptions, and confidence level. `--brief` and `--status` surface Stale and Degraded decisions automatically. Long-horizon decisions (annual strategy, platform bets) can set a `shelf_life` to prevent false alarms.

### How your decision-making style is evolving — and whether you've noticed
**Founder Pattern Drift.** With 10+ logged decisions, CPO computes rolling-window behavioral analysis across 5 dimensions: Truth weighting bias, path preference, confidence calibration, reversal rate, and kill criteria usage. It tells you whether shifts in your decision patterns are intentional growth or unconscious drift.

---

## Start here

**New idea — where's the opportunity?** Three paths, kill criteria, and a clear verdict.
→ `/cpo what should we build`

**Need a go / no-go?** Five Truths assessment, three options, and what to kill it on.
→ `/cpo should we do this`

**Heading into a raise?** Five investor archetypes debate your pitch live.
→ `/cpo simulate an investor meeting`

**Preparing for a board meeting?** Multi-turn simulation with archetypes who go off-agenda.
→ `/cpo simulate the board`

**Need to build the case upward?** 1-min and 5-min pitch with objection pre-emption.
→ `/cpo --sell-up CEO`

**Right-now executive snapshot?** Force-ranks to ONE red line blocking velocity.
→ `/cpo --status`

**Weekly strategic digest?** Kill criteria, decay scores, consequence checks, drift alerts.
→ `/cpo --brief`

**See your decision-making DNA.** Truth bias, path preference, confidence calibration.
→ `/cpo --patterns`

**All active kill criteria with countdown timers.** Grouped by urgency.
→ `/cpo --kills`

**Decision dependencies and bottlenecks.** Which decision unblocks the most?
→ `/cpo --graph`

**Revisiting a prior decision?** Session replay guards against hindsight bias.
→ `/cpo --outcome [topic]` or `/cpo #[decision-name]`

First time here? → `/cpo ?`

---

> CPO is not a prompt. It's a system: structured reasoning, persistent memory, and live simulations working together across every major product decision.

---

## Why I built this

When execution becomes abundant, judgment becomes scarce. Every AI tool today accelerates shipping — but bad strategy plus great execution is still failure.

Over many years leading strategy and building products across financial services and web3, I found that the hardest problems were never about execution. They were upstream: which opportunity is worth pursuing, which assumptions will kill you, what is the right path for this stage, and how do you know when to stop.

CPO encodes that way of working — think clearly, test tradeoffs honestly, pressure-test bets before they become expensive, and iterate without losing the plot. It is the missing layer that sits before execution and alongside it.

If more people now have the power to build, they also need better tools for deciding what deserves to be built.

---

## The core model

`/cpo` gives you **decision structure** and turns strategic thinking into **clear, defensible communication**.

Most product decisions are expensive long before they reach code:
- entering a new market
- funding a new bet
- hiring against a roadmap
- raising capital around a narrative
- choosing whether to pivot, double down, or kill something

`/cpo` is built for those moments.

It pressure-tests every decision across **Five Truths**:
- **User**
- **Strategic**
- **Economic**
- **Macro-Political**
- **Execution**

Before making a recommendation, it explores **Three Paths** — situational labels derived from the decision at hand, not fixed risk categories. Each path names what it bets on, optimizes for, or defers.

It also:
- labels claims as **fact / assumption / inference / judgment**
- defines **kill criteria upfront** — each with a named metric, specific threshold, and timeframe
- records a **Truth fingerprint** on every decision (Dominant · Grounded · Inferred)
- auto-generates **falsifiable predictions** at decision time and tracks them to verification
- checks every new decision against all active ones for **coherence** — contradictions, assumption collisions, kill criteria conflicts
- remembers company context across sessions
- logs major decisions in a searchable, addressable journal
- computes **dependency graphs** across decisions to find bottlenecks
- tracks **kill criterion countdowns** with urgency classification
- measures **decision decay** — surfacing stale assumptions before they become expensive
- lets you retire superseded decisions with `--invalidate` and detect **logic drift** with `--drift`
- simulates investor and board conversations in real time
- acts as a **decision layer for your full skill stack** via `--decide` handoffs

The sleeper feature is the decision log.

Every major decision is timestamped, reasoned, and searchable. Tag any decision with `#name` (e.g., `/cpo #pricing should we add a free tier?`) to create an addressable record you can revisit, update, and track over time. CPO tracks a **Truth fingerprint** — Dominant, Grounded, and Inferred Truths — on every decision, so the reasoning behind each call is never lost.

When a decision is superseded, use `--invalidate` to retire it cleanly — with an annotation — so it stops polluting future context. When a series of small decisions has quietly moved you away from a prior commitment, use `--drift` to surface the contradiction before it becomes expensive.

Run `/cpo --brief` for a weekly intelligence digest:
- kill criteria approaching
- unresolved decisions needing follow-up
- patterns in decision quality
- logic drift across bets
- consequence predictions due for verification
- decision decay scores on aging assumptions

Run `/cpo --graph` to see the dependency graph across all active decisions — bottleneck identification, transitive chains, orphaned decisions. Run `/cpo --kills` for a cross-cutting dashboard of every active kill criterion with countdown timers grouped by urgency.

Run `/cpo --patterns` to see your decision-making DNA: which Truths you consistently over- or under-weight, how often your kill criteria actually trigger, and whether your confidence calibration is accurate. With 10+ decisions, CPO computes rolling-window drift — comparing your recent 10 decisions against prior ones to surface how your decision style is evolving.

A year in, you do not just have outputs. You have **institutional memory** — and a mirror on how you actually make decisions.

---

## Quick start: your first 5 minutes

### 1) Install for Claude Code

Requirements: Claude Code, Git

```bash
git clone https://github.com/0x2kNJ/CPO.git ~/.claude/skills/cpo
```

Then add this to your `CLAUDE.md`:

```md
## cpo
Available skill: /cpo
```

Invoke with `/cpo` in any Claude Code session. No build step. No dependencies.

### 2) Install for Cursor

Use `@CURSOR.md` at the start of a conversation, or add this to `.cursorrules` for always-on usage:

```
@~/.claude/skills/cpo/CURSOR.md
```

CPO uses the same shared `~/.cpo/` state across Claude Code and Cursor.

### 3) Run your first decision

Try one of these:

```
/cpo what should we build for onboarding activation in our B2B SaaS?
```

```
/cpo should we do this
```

```
/cpo --brief
```

### 4) Save your company context (one-time)

Run this once so CPO knows your stage and business model — it will never ask again:

```
/cpo --save-context
```

CPO will ask 6 quick questions and save the answers to `~/.cpo/context.md`. Every future session loads this automatically. The 6th question captures your **operating bias** (build-first, research-first, or relationship-first) — this powers `--status` so action recommendations match how you actually operate, not generic startup advice. Without it, CPO will ask one calibration question at the start of GTM, roadmap, and strategy prompts.

### 5) Stop there

If the output feels sharper than your usual brainstorming doc, this is probably for you.

---

## Who this is for

### Solo founders
Deciding what to build, how to build it, whether to pivot or kill a bet under uncertainty, and when — and how — to raise or hire.

### PMs and Heads of Product
Building the case upward to leadership and laterally across engineering, design, GTM, and finance.

### Product-minded execs
Using structured reasoning to pressure-test strategy before it becomes roadmap, spend, or narrative.

### Teams preparing for high-stakes conversations
Investor meetings, board meetings, roadmap reviews, strategic offsites, and post-mortems.

---

## Works best when…

- you have a real decision, not a vague topic
- you can name your company stage: pre-seed, seed, post-PMF, Series A/B, scale-up
- you are willing to compare multiple paths, not just seek validation
- you want an opinionated sparring partner, not a note-taker
- you want a decision record, not just one-off outputs

---

## How CPO works

### 1) Five Truths

Every decision is tested across five dimensions:

| Truth | What it asks |
|:---|:---|
| User | Does this solve a real pain or unlock real demand? |
| Strategic | Does this fit where the company is actually going? |
| Economic | Can this become a meaningful return on time, capital, or focus? |
| Macro-Political | What external conditions make this easier or harder now? |
| Execution | Can this team actually pull it off from here? |

### 2) Three Paths

CPO does not jump straight to one answer. It explores multiple paths first.

Each path is labeled situationally — naming what it commits to, not how risky it is:
- *"Resolve toward [X] / Resolve toward [Y] / Defer [conflict]"* — for strategy conflicts
- *"Concentrate on [focus] / Sequence by [dependency] / Time-box [decision]"* — for scope/sequencing
- *"Community-led / Product-led / Partner-led"* — for channel decisions

### 3) Decision hygiene

Every output distinguishes between:
- **Fact**
- **Assumption**
- **Inference**
- **Judgment**

That makes the reasoning auditable instead of persuasive-by-default.

### 4) Kill criteria

Before recommending a path, CPO defines what would make the bet fail — each with a named metric, specific threshold, and timeframe. That means you know:
- what must be true
- what to monitor
- what would trigger re-scope
- when to stop

### 5) Structural integrity

CPO doesn't just log decisions — it validates them:
- **Coherence checks** catch contradictions between new and prior decisions at write time
- **Dependency graphs** map which decisions block which others
- **Decay tracking** surfaces assumptions that are aging past their validity window
- **Consequence tracking** auto-generates predictions and checks them against reality
- **Pattern analysis** reveals your decision-making tendencies over time

### 6) Persistent memory

CPO maintains shared state across sessions, including:
- company context
- decision journal
- simulations
- exports

So each decision starts with more context than the last one.

---

## What CPO helps you do

| Use case | What CPO does |
|:---|:---|
| New product bets | Sizes the opportunity, surfaces assumptions, frames paths |
| Go / no-go decisions | Tests whether to proceed, pause, re-scope, or kill |
| Roadmap prioritization | Compares strategic and economic tradeoffs |
| Investor prep | Simulates pushback, weak spots, and narrative gaps |
| Board prep | Stress-tests the plan and likely questions |
| Executive alignment | Builds the case upward with structured reasoning |
| Decision follow-up | Tracks outcomes, verifies predictions, detects pattern drift |
| Kill monitoring | Countdown timers on every active kill criterion |
| Bottleneck identification | Dependency graph showing what's blocking velocity |

---

## See it work

Five examples showing how teams use CPO in practice: developing a product idea, reaching a go / no-go, running a weekly brief, preparing for a board meeting, and using structural intelligence across active bets.

### 1) Developing a product idea

**Prompt**
```
/cpo what should we build to improve activation for new users in our B2B SaaS product?
```

**What CPO returned**
- framed the activation problem across the **Five Truths**
- identified the most likely opportunity as reducing time-to-value in the first session
- proposed **Three Paths**:
  - **Redesign the first session** — guided setup flow, maximum time-to-value reduction
  - **Layer progressive nudges** — templates, checklists, and milestone triggers on existing flow
  - **Instrument before rebuilding** — improve copy and tracking first, let data decide the rebuild
- surfaced key assumptions, risks, and kill criteria for each path
- recommended **Layer progressive nudges** to increase activation without a full rebuild

**Why it mattered**
The team did not just get ideas. They got a structured product direction, a clear recommendation, and a sharper view of what had to be true before investing more.

---

### 2) Reaching a go / no-go decision

**Prompt**
```
/cpo should we launch a collaboration feature for mid-market teams this quarter?
```

**What CPO returned**
- pressure-tested the opportunity across user demand, strategic fit, economics, timing, and execution risk
- showed **Three Paths**:
  - **Launch broadly this quarter** — maximum signal, maximum risk
  - **Constrained beta with 5–10 accounts** — real data, bounded blast radius
  - **Defer and shore up retention first** — protect the base, revisit next quarter
- labeled major claims as **fact / assumption / inference / judgment**
- identified kill criteria, including weak pull from current customers and high execution drag on the roadmap
- recommended **go, but only as a constrained beta**, not a full launch

**Why it mattered**
Instead of arguing in circles, the team got to a decision they could defend: proceed, but with clear scope, clear risks, and clear conditions for stopping.

---

### 3) Weekly brief

**Prompt**
```
/cpo --brief
```

**What CPO returned**
- summarized active decisions and unresolved follow-ups
- highlighted kill criteria that were getting close
- surfaced consequence predictions due for verification
- flagged decision decay on aging assumptions
- surfaced repeated patterns in decision-making, including overcommitting before validating demand
- flagged strategic drift between current roadmap work and stated company priorities

**Why it mattered**
The weekly brief helped leadership step back from day-to-day motion and see where focus was slipping, where bets were aging, and which decisions needed attention before they became expensive.

---

### 4) Board meeting simulation

**Prompt**
```
/cpo simulate the board
```

**What CPO returned**
- ran a live multi-turn board simulation with speaking archetypes (Lead VC, Financial VC, Independent Director)
- went off-agenda — the Lead VC challenged burn assumptions not on the slide
- stress-tested the company's current narrative and exposed weak spots in the Q3 forecast
- ended with a post-meeting debrief: what was committed to, where the room was lost, what to fix before the real meeting

**Why it mattered**
The team went into the board meeting better prepared for pushback, with stronger answers, clearer logic, and a more coherent story about where the company was going and why.

---

### 5) Structural intelligence across active bets

**Prompt**
```
/cpo --kills
```

**What CPO returned**
- scanned 8 active decisions and extracted 14 kill criteria
- classified each: 1 TRIGGERED (enterprise pilot past 90-day window), 3 APPROACHING, 8 ACTIVE, 2 with no timeframe
- cross-referenced with the dependency graph — the triggered criterion belonged to a decision that also blocked 3 downstream bets
- surfaced: *"⚠ #enterprise-pilot is both past its kill window AND blocking 3 other decisions — resolve this first."*

**Then running `/cpo --graph`:**
- mapped all decision dependencies, identified `#enterprise-pilot` as the bottleneck (fan-out score: 4.7)
- showed the transitive chain: `#enterprise-pilot → #pricing-model → #series-a-narrative` (3 hops)
- recommended resolving the bottleneck before committing to the Series A narrative

**Why it mattered**
No PM manually maintains countdown timers across 14 kill criteria, cross-references them against a dependency graph, and identifies compounding risk where a bottleneck also has a triggered kill criterion. This is structural intelligence that only exists with persistent memory and automated cross-referencing.

---

## Install

### Claude Code

```bash
git clone https://github.com/0x2kNJ/CPO.git ~/.claude/skills/cpo
```

Add to `CLAUDE.md`:

```md
## cpo
Available skill: /cpo
```

### Cursor

Use `@CURSOR.md` at the start of a conversation, or add this to `.cursorrules`:

```
@~/.claude/skills/cpo/CURSOR.md
```

### Shared state

CPO shares the same `~/.cpo/` state across Claude Code and Cursor:
- decision journal
- company context
- simulation transcripts
- exports

---

## Command model

CPO is designed around one command:

```
/cpo
```

You invoke it with natural language, mode selectors, or flags.

```
/cpo what should we build
/cpo should we do this
/cpo simulate an investor meeting
/cpo simulate the board
/cpo --sell-up CEO
/cpo --brief
/cpo ?
```

---

## Advanced

### Modes

CPO supports 20 core modes. Invoke them with natural language — CPO routes automatically. The internal mode name is shown in parentheses for direct invocation.

★ = most-used starting points

| Mode | Invoke with | What it does | Best for |
|:---|:---|:---|:---|
| ★ Opportunity Scan (`blue-ocean`) | `/cpo what should we build` | Maps uncontested opportunity spaces, ranks bets, sequences what to do first | New bets, growth ideas, product direction |
| ★ Go / No-Go (`ceo`) | `/cpo should we do this` | Verdict: proceed, pause, re-scope, or kill — with Three Paths and kill criteria | High-stakes decisions |
| ★ Investor Simulation (`investor-roundtable`) | `/cpo simulate an investor meeting` | Live 5-round investor debate, 5 archetypes, distinct voices, verdict per archetype | Fundraising meetings |
| ★ Board Simulation (`boardroom`) | `/cpo simulate the board` | Live multi-turn board meeting with speaking archetypes + post-meeting debrief | Board prep |
| ★ Weekly Brief (`--brief`) | `/cpo --brief` | Kill criteria, consequence checks, decision decay, pattern alerts, strategic drift | Weekly leadership cadence |
| Prioritization (`sequence`) | `/cpo what should we prioritize` | Orders the roadmap by leverage, reversibility, and strategic unlock | Roadmaps, portfolio choices |
| Assumption Audit (`discovery`) | `/cpo what are our riskiest assumptions` | Surfaces bets most likely to be wrong before you commit | Discovery planning, diligence |
| GTM Design (`gtm`) | `/cpo how do we go to market` | ICP, channels, growth loop, retention — full go-to-market design | Launch planning, GTM motion |
| Positioning (`narrative`) | `/cpo how do we position this` | Category design, 1-sentence claim, narrative spine, objection map | Positioning, category creation |
| Launch Planning (`launch-os`) | `/cpo plan our launch` | 8-phase launch plan, owner map, launch narrative, success metrics | Launch sequencing |
| Raise Prep (`investor-story`) | `/cpo help me prep for a raise` | Narrative spine, objection map, proof point gaps for the raise | Fundraising prep |
| Risk Review (`red-team`) | `/cpo what could go wrong` | Top 5 failure vectors ranked by probability × impact, mitigation per vector | Pre-commit risk review |
| Pre-mortem (`premortem`) | `/cpo assume this failed — why` | Failure scenario, root cause chain, pre-emptive actions ranked | Pre-commitment risk sim |
| Retrospective (`postmortem`) | `/cpo why did this fail` | Timeline reconstruction, root cause, what to change, system fix | Post-mortems |
| Team Structure (`org-design`) | `/cpo how should we structure the team` | Team shape, reporting lines, transition plan, failure modes | Org planning |
| Board Update (`board-memo`) | `/cpo write a board update` | Board memo: Context → Highlights → Lowlights → Key Decisions → Ask | Board communication |
| Board Deck (`board-story`) | `/cpo help me prepare board materials` | Board deck narrative spine, 9-slide structure, pre-emptive Q&A by archetype | Board deck prep |
| Eng Handoff (`eng-brief`) | `/cpo brief the engineering team` | PR/FAQ-style brief: Problem · Solution · Non-goals · Metrics · Open questions — saved to `~/.cpo/briefs/` | Engineering handoff |
| Tech Decoder (`eng-translate`) | `/cpo decode what engineering said` | Translates technical constraints into product implications and decisions | Eng ↔ product alignment |
| Sell-Up Narrative (`upward-pitch`) | `/cpo help me convince my CEO` | Decision-maker profile, argument, evidence architecture, objection map, explicit ask | Internal advocacy |

---

### Flags

CPO supports 30 flags for shaping how it reasons and communicates.

| Flag | Example | What it does |
|:---|:---|:---|
| `--go` | `/cpo --go should we raise now` | Skip menus. Route directly. Get the answer immediately. Also skips simulation gate and strategy confirmation. |
| `--deep` | `/cpo --deep should we pivot` | Full 10-section structured output. All Five Truths assessed explicitly. |
| `--quick` | `/cpo --quick is freemium right for us` | Single-response mode. Dominant Truth + immediate recommendation + one kill criterion. No grounding questions. |
| `--memo` | `/cpo --memo should we enter healthcare` | Formats output as a printable decision memo. No headers. |
| `--silent` | `/cpo --silent should we kill this` | No calibration questions. Proceeds with stated assumptions. Flags every inference. |
| `--compare` | `/cpo --compare option-a vs option-b` | Runs two approaches side-by-side on the same input. |
| `--roadmap` | `/cpo --roadmap 4 bets` | Comparative prioritization across N bets — Five Truths per bet, ASCII timeline, kill criteria per bet. |
| `--sell-up` | `/cpo --sell-up CEO` | Reframes any decision as an internal pitch. 1-min + 5-min version, objection pre-emption, explicit ask. |
| `--status` | `/cpo --status` | Executive snapshot — force-ranks to ONE red line (the thing blocking velocity). Grounded in decision journal history, kill criteria proximity, and your operating style. Add `--verbose` to see the full synthesis trace. |
| `--save-context` | `/cpo --save-context` | Saves company context — stage, model, constraint, priorities, and operating bias. Loads automatically every session. |
| `--context` | `/cpo --context series-a` | Loads a named context from `~/.cpo/contexts/[name].md`. For consultants or multi-company use. |
| `--no-context` | `/cpo --no-context` | Ignores saved context. Reasons from the current prompt only. |
| `--focus` | `/cpo --focus burn-rate` | Simulation modes only — jumps directly to targeted pressure on one topic. |
| `--since` | `/cpo --since last-time` | Temporal delta — leads with what changed since a prior baseline. Pulls from decision journal. |
| `--brief` | `/cpo --brief` | Weekly intelligence brief. Kill criteria, consequence checks, decision decay, pattern alerts. |
| `--trail` | `/cpo --trail` | 90-day strategic diary — all journal entries, one page. |
| `--history` | `/cpo --history enterprise` | Full decision journal. Optional keyword filter or exact `#name` lookup. |
| `--outcome` | `/cpo --outcome enterprise-pause` | Closes the loop with Session Replay (hindsight bias guard). Records what happened. Verifies predictions. Detects path patterns over time. |
| `--patterns` | `/cpo --patterns` | Decision DNA — Truth weighting bias, path preference, kill criteria hit rate, reversal rate, confidence calibration. Rolling-window drift with 10+ decisions. |
| `--invalidate` | `/cpo --invalidate #pricing` | Marks a past decision as retired. Annotates with date and reason. Future context loads skip it; `--history` always shows it. |
| `--invalidate-all` | `/cpo --invalidate-all` | Bulk invalidation — mark all active journal entries as invalidated. Optional `#name` filter to scope to one decision. Requires YES confirmation. Add `--hard` to permanently delete YAML files (irreversible). |
| `--drift` | `/cpo --drift` | Logic drift detection — scans the last 10 decisions for structural contradictions: unacknowledged Truth fingerprint shifts, verdict reversals, or kill criteria degradation. |
| `--graph` | `/cpo --graph` | Decision dependency graph — bottleneck identification, fan-out scoring, transitive chains, orphaned decisions. Cross-references with `--kills` for compounding risk. |
| `--kills` | `/cpo --kills` | Kill criteria countdown dashboard — ALL active kill criteria with days remaining, grouped by urgency (triggered / approaching / active / undated / external). Cross-references with `--graph`. |
| `--decide` | `/cpo --decide` | Inbound handoff from another skill. CPO reads the situation, scans your installed toolchain, and routes to the best next action — with install suggestion + fallback if the ideal skill isn't present. |
| `--export` | `/cpo --export` | Writes output to `~/.cpo/exports/YYYY-MM-DD-[slug].md` — shareable with co-founders, boards, investors. |
| `--schedule-brief` | `/cpo --schedule-brief` | Sets up a recurring weekly brief that runs automatically. Uses `anthropic-skills:schedule`. *(Claude Code only.)* |
| `--setup-integrations` | `/cpo --setup-integrations` | Detects MCP data sources and configures live data enrichment for Five Truths assessments. |
| `--import-context [path]` | `/cpo --import-context ./strategy.md` | Imports a context file into `~/.cpo/`. Useful for syncing context from a shared repo. |
| `--scan-strategy` | `/cpo --scan-strategy` | Alone: re-run strategic context scan and rebuild posture summary. With a question: cross-reference strategy files, surface tensions/alignments, then run the four-action flow with strategy-anchored grounding. |
| `--stack` | `/cpo --stack` | Shows the full product workflow with coverage status. Detects installed complementary skills and marks CPO-aware ones with `[✓ CPO-aware]`. |
| `--update` | `/cpo --update` | Outputs update instructions: `cd ~/.claude/skills/cpo && git pull`. |

> Natural language should still be the default. Flags are best when you want format control, audience control, or repeatable workflows.

### Decision Objects

Tag any decision with `#name` to make it addressable across sessions:

```
/cpo #pricing should we add a free tier?
```

CPO creates a named journal entry and links all future analyses on the same decision. Revisit it anytime — CPO opens with a delta frame (what's changed since last time) instead of starting fresh. Use `--history #pricing`, `--outcome #pricing`, or `--since #pricing` for exact lookup by name.

---

### Persistent layers

CPO uses shared local state so it can compound context over time. Everything lives in `~/.cpo/` — shared between Claude Code and Cursor.

| Path | What it stores | Why it matters |
|:---|:---|:---|
| `~/.cpo/context.md` | Company profile — stage, model, constraint, priorities, operating bias | Keeps answers grounded in your actual company. Loads every session. The `operating_bias` field powers `--status` action recommendations. |
| `~/.cpo/last-status.yaml` | Last `--status` run — red line, action, at-risk decision | Enables delta detection: next `--status` run shows whether the red line changed and why. |
| `~/.cpo/decisions/` | Decision journal — one YAML per major analysis (schema v1.5: includes Truth fingerprint, consequences, path chosen, shelf life, status, kill criteria) | Builds a searchable strategic history. Foundation for `--brief`, `--trail`, `--since`, `--drift`, `--graph`, `--kills`, `--patterns`, `--invalidate`. |
| `~/.cpo/simulations/` | Boardroom and investor roundtable transcripts | Preserves full simulation sessions |
| `~/.cpo/exports/` | Exported memos and analyses — dated and slugged | Makes outputs portable and shareable |
| `~/.cpo/integrations.md` | Live data source config — injected into Five Truths | Enriches assessments with real company data |
| `~/.cpo/signals/` | Cross-skill signals — structured YAML files written by other skills (retro, QA, review) | Enriches `--status` with real-time project data: velocity trends, quality regressions, review findings. See `references/signals-contract.md`. |
| `~/.cpo/.scratch/` | Five Truths snapshots — written after Assess on named decisions (`#name`) or `--deep` runs | Quick reference for the full reasoning behind any decision mid-conversation |
| `~/.cpo/.version` | Version tracking | Surfaces mismatches between skill and saved state |
| `~/.cpo/contexts/` | Named contexts for multi-company use | Supports `--context [name]` switching |
| `.claude/strategy/` | Project-level strategy docs — read by CPO on each session for strategic posture synthesis | Keeps CPO grounded in the project's current direction; populated by `--save-context` or manually |

---

### Structure

```
cpo/
├── SKILL.md                    # Claude Code skill
├── CURSOR.md                   # Cursor-compatible version
├── CHANGELOG.md                # Version history
├── README.md                   # This file
├── tests/
│   └── expected-outputs.md     # Behavioral test cases
└── references/
    ├── examples.md             # Full example library
    ├── frameworks.md           # 14 strategic frameworks
    ├── personas.md             # Advisory persona cards
    ├── investor-panels.md      # Investor panel templates
    ├── domains/                # Domain-specific Five Truths overlays
    │   ├── ai-products.md
    │   ├── saas-b2b.md
    │   ├── crypto-web3.md
    │   ├── identity-auth.md
    │   └── developer-platforms.md
    ├── modes/                  # Full mode templates (20 modes)
    │   ├── ceo.md
    │   ├── boardroom.md
    │   ├── investor-roundtable.md
    │   └── ...
    ├── handoff-contract.md     # Skill Handoff Contract for CPO-aware skill authors
    ├── signals-contract.md     # Cross-skill Signals Contract for --status enrichment
    └── flags/                  # Full flag behavior specs (19 files, loaded on demand)
        ├── brief.md
        ├── roadmap.md
        ├── sell-up.md
        ├── drift.md            # Logic drift detection spec
        ├── graph.md            # Decision dependency graph spec
        ├── kills.md            # Kill criteria countdown dashboard spec
        ├── invalidate.md       # Journal invalidation spec
        ├── patterns.md         # Decision DNA analysis spec
        ├── combinations.md     # Flag stacking and combination rules
        └── ...

# Project-level strategy context (auto-read on session start)
.claude/strategy/               # Strategy docs scanned by CPO preamble
    ├── strategy.md             # (example) company strategy
    ├── roadmap.md              # (example) product roadmap
    └── ...
```

---

## Works with other skills

CPO is best used as the **decision layer before execution**.

It helps you decide:
- what to build
- why it matters
- which path to take
- how to defend the choice
- what would make you stop

Then it hands off cleanly into the rest of your skill stack.

### Dynamic handoff model

The pattern is simple:

1. **CPO frames the decision** — opportunity · tradeoffs · assumptions · recommendation · kill criteria
2. **CPO sharpens the narrative** — board-ready · investor-ready · CEO / leadership framing · rationale that can travel across teams
3. **Other skills operationalize the decision** — discovery · research · PRDs · planning · execution · launch · measurement
4. **CPO returns to evaluate** — weekly brief · follow-up go / no-go · board review · decision journal review

**CPO decides. Other skills operationalize. CPO returns to evaluate.**

### CPO as a decision layer (`--decide`)

Any installed skill can invoke CPO at a decision fork using the `--decide` flag. CPO reads the handoff context, scans your installed toolchain, and recommends the best next action — including install suggestions if the ideal skill isn't present.

Skills that implement this contract are marked `[✓ CPO-aware]` in `/cpo --stack` output.

**Example:** `/qa` finds a critical regression and doesn't know whether to block the release or escalate. It emits a `CPO Handoff Request` block and calls `/cpo --decide`. CPO reads the context, scans the available skills, and routes to the right next action.

For skill authors: see `references/handoff-contract.md` for the full contract spec.

### Example handoffs

| If CPO helps you decide… | Then hand off to… |
|:---|:---|
| what opportunity to pursue | discovery, market research, user research skills |
| whether to proceed with a bet | planning, scoping, PRD, roadmap, execution skills |
| how to position the bet | strategy, GTM, launch, messaging skills |
| what success looks like | metrics, analytics, experimentation skills |
| how to defend the choice | board, investor, CEO, and leadership communication workflows |
| when to stop | review, retro, and decision follow-up workflows |

### Recommended handoff stack

| Tool | When | What it does |
|:---|:---|:---|
| [gstack](https://github.com/garrytan/gstack) `/plan-eng-review` | After `/cpo` → need implementation plan | Architecture, data flow, edge cases |
| [gstack](https://github.com/garrytan/gstack) `/plan-ceo-review` | After `/cpo` recommendation → pressure-test the experience and vision | 10-star product thinking, narrative reframe, challenge premises |
| [gstack](https://github.com/garrytan/gstack) `/review` | After `eng-brief` → need code review | Bugs that pass CI, completeness gaps |
| [gstack](https://github.com/garrytan/gstack) `/qa` | After `postmortem` or `red-team` → need QA plan | Real browser, real clicks, regression tests |
| [gstack](https://github.com/garrytan/gstack) `/retro` | After decision shipped → team retrospective | Per-person breakdown, shipping streaks |
| [pm-skills](https://github.com/phuryn/pm-skills) | After strategy is set → execution artifacts | PRDs, OKRs, sprint plans, user stories |
| [notebooklm-py](https://github.com/teng-lin/notebooklm-py) | Before `/cpo` → enrich with research docs | Inject competitor intel, interviews, and market data into Five Truths |
| [notebooklm-py](https://github.com/teng-lin/notebooklm-py) | After `--export` → distribute the decision | Generate a podcast brief, board slide deck, or team study guide from the memo |

---

## Complementary skill ecosystems

### PM Skills

[pm-skills](https://github.com/phuryn/pm-skills) covers a wide PM workflow surface area — discovery, strategy, execution, launch, and growth.

Use **CPO** when you need:
- a high-stakes product decision
- strategic framing before execution
- a go / no-go call
- board or investor pressure-testing
- a decision journal and persistent reasoning

Use **pm-skills** when you need:
- product discovery workflows
- assumption testing
- PRDs
- launch planning
- market research
- analytics and growth workflows
- broader PM execution support

A strong combined motion: use **CPO** to decide whether the bet is worth making → use **pm-skills** to discover, define, validate, scope, and launch it → return to **CPO** to review the bet over time and decide whether to double down, adapt, or kill it.

### gstack

[gstack](https://github.com/garrytan/gstack) covers execution with an opinionated set of role-based skills: CEO review, engineering review, QA, release, and delivery workflows.

Use **CPO** when the question is: what should we do? should we do this now? how do we defend the decision? what are the risks and kill criteria?

Use **gstack** when the question becomes: how do we execute this with discipline? how do we review, document, QA, and ship?

A strong combined motion: use **CPO** to frame the strategic decision → use **gstack** to move the chosen plan through execution, review, and delivery → return to **CPO** for weekly briefings and decision follow-up.

---

## Design principles

CPO is opinionated. It assumes:
- strategy should be explicit
- tradeoffs should be named
- claims should be typed
- recommendations should be compared against alternatives
- kill criteria should exist before commitment
- decisions should leave a record
- assumptions should be checked against reality, not forgotten

That is the whole point.

---

## FAQ

**Is this for founders only?**
No. It is especially useful for solo founders, PMs, Heads of Product, chiefs of staff, and product-minded execs making high-stakes product and company decisions.

**Is this a PRD generator?**
No. CPO sits upstream of PRDs. It helps you decide whether the thing is worth doing, how to frame it, and what must be true before execution.

**Does it replace discovery or execution workflows?**
No. It complements them. CPO is the strategic decision layer before execution, and the review layer after execution starts.

**Does it work in Cursor?**
Yes. Use `@CURSOR.md` at the start of a conversation, or add it to `.cursorrules`. It shares the same `~/.cpo/` state as Claude Code.

**Does it remember my company context?**
Yes. CPO compounds context over time through its shared local state and decision journal.

**Can I retire old decisions that no longer reflect my thinking?**
Yes. `/cpo --invalidate [topic]` marks any journal entry as retired — with a reason you provide. Future context loads skip it silently; `--history` always shows it for audit. This prevents old superseded decisions from polluting new analysis.

**What is logic drift?**
Logic drift is when a series of individually reasonable decisions quietly moves you away from a prior strategic commitment — without ever explicitly deciding to change course. `/cpo --drift` scans your last 10 decisions for structural contradictions: unacknowledged Truth fingerprint shifts, verdict reversals without explanation, and kill criteria degradation. The passive drift surface also fires automatically whenever you write a new journal entry that shifts Dominant Truth from the last one on the same topic.

**What are consequences and why do they matter?**
At decision time, CPO auto-generates 2–3 falsifiable predictions about what should happen if the recommended path succeeds. Each prediction has a check date. When that date arrives, `--brief` surfaces it for verification. Over time, this creates a track record of whether your decision framework actually predicts outcomes — a personal prediction market.

**What is decision decay?**
Every active decision has a mechanical decay score based on age, inferred assumptions, and confidence level. As decisions age and their assumptions go unchecked, the score increases. `--brief` and `--status` surface Stale and Degraded decisions before the founder has to remember to check. Long-horizon decisions (annual strategy, platform bets) can set a `shelf_life` field to prevent false decay alarms.

---

## Version · License

Current: **v2.3.0** — [view changelog](CHANGELOG.md)

MIT. Free forever. Go make better product decisions.
