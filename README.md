# (C)hief (P)roduct (O)fficer

**The operating system for product decisions before execution.**

`/cpo` helps founders, PMs, and executives decide **what to build, whether to build it, how to communicate it, and when to kill it** — before the team commits time, headcount, or capital.

It sits between **"we have an idea"** and **"engineering is building it."**
Opportunity assessment, go / no-go decisions, roadmap prioritization, investor and board simulations, sell-up narratives, and a decision log that compounds context over time.

If this helps you make better product decisions, ⭐ the repo.

---

## Start here

New idea — where's the opportunity?
→ `/cpo what should we build`

Need a go / no-go?
→ `/cpo should we do this`

Heading into a raise?
→ `/cpo simulate an investor meeting`

Preparing for a board meeting?
→ `/cpo simulate the board`

Need to build the case upward?
→ `/cpo --sell-up CEO`

Want a weekly strategic digest?
→ `/cpo --brief`

First time here?
→ `/cpo ?`

---

> CPO is not a prompt. It's a system: structured reasoning, persistent memory, and live simulations working together across every major product decision.

---

## Why I built this

We are moving into a world where the ability to build is becoming widely accessible.

More people can now operate like engineers. Teams can prototype, ship, automate, and iterate with a speed that used to require much larger organizations. That changes the game.

But it also raises the stakes on a different question:

**What should we build in the first place?**

When execution becomes abundant, judgment becomes scarce.
When shipping gets easier, deciding well becomes more valuable.
When everyone can build, the teams that win are the ones that know **where to point that power**.

I built CPO because I believe that knowing what to build is now one of the most important skills in modern product work — and one of the most underserved.

Over many years leading strategy and building products across financial services and web3, I found that the hardest problems were rarely about execution alone. They were upstream decision problems:

- Which opportunity is actually worth pursuing?
- Which assumptions matter most?
- What is the right path for this stage of company?
- How do you communicate the decision clearly enough that others can align behind it?
- And how do you know when to stop?

I also noticed a persistent gap in the tooling.

Most AI skill sets today are weighted toward execution. They help teams write faster, code faster, ship faster, and automate faster once a direction has already been chosen. That matters. But far fewer tools help teams make the decision itself with rigor.

That gap matters more now, not less.

Because bad strategy plus great execution is still failure.
Because faster teams can still waste years.
Because more output does not automatically create more value.

CPO is my attempt to encode a way of working I developed under real product and strategy pressure: a way to think clearly, test tradeoffs honestly, pressure-test bets before they become expensive, communicate decisions in a way others can align behind, and iterate quickly without losing the plot.

This is not a replacement for execution-focused skills. It is the missing layer that should sit before them and alongside them.

My hope is that CPO helps push the ecosystem toward better product judgment as a first-class capability — not just better output. If more people now have the power to build, then more people also need better tools for deciding what deserves to be built.

That is the movement I want to contribute to.

If that resonates with you, you are exactly who I built this for.

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
- defines **kill criteria upfront**
- remembers company context across sessions
- logs major decisions in a searchable journal
- simulates investor and board conversations in real time

The sleeper feature is the decision log.

Every major decision is timestamped, reasoned, and searchable. Over time, `/cpo` becomes a living record of how your company thinks: what you believed, what you chose, what happened, and which patterns keep repeating.

Run `/cpo --brief` for a weekly intelligence digest:
- kill criteria approaching
- unresolved decisions needing follow-up
- patterns in decision quality
- strategic drift across bets

A year in, you do not just have outputs. You have **institutional memory**.

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

CPO will ask 5 quick questions and save the answers to `~/.cpo/context.md`. Every future session loads this automatically. Without it, CPO will ask one calibration question at the start of GTM, roadmap, and strategy prompts.

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

Before recommending a path, CPO defines what would make the bet fail. That means you know:
- what must be true
- what to monitor
- what would trigger re-scope
- when to stop

### 5) Persistent memory

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

---

## See it work

Here are four short examples that show how teams use CPO in practice: to develop a product idea, reach a go / no-go decision, run a weekly brief, and prepare for a board meeting.

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
- surfaced repeated patterns in decision-making, including overcommitting before validating demand
- flagged strategic drift between current roadmap work and stated company priorities
- turned the week's decision history into a concise executive brief

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
| ★ Weekly Brief (`--brief`) | `/cpo --brief` | Summarizes active decisions, kill criteria, risks, and strategic drift | Weekly leadership cadence |
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
| Eng Handoff (`eng-brief`) | `/cpo brief the engineering team` | PR/FAQ-style brief: Problem · Solution · Non-goals · Metrics · Open questions | Engineering handoff |
| Tech Decoder (`eng-translate`) | `/cpo decode what engineering said` | Translates technical constraints into product implications and decisions | Eng ↔ product alignment |
| Sell-Up Narrative (`upward-pitch`) | `/cpo help me convince my CEO` | Decision-maker profile, argument, evidence architecture, objection map, explicit ask | Internal advocacy |

---

### Flags

CPO supports 24 flags for shaping how it reasons and communicates.

| Flag | Example | What it does |
|:---|:---|:---|
| `--go` | `/cpo --go should we raise now` | Skip menus. Route directly. Get the answer immediately. Also skips simulation gate. |
| `--deep` | `/cpo --deep should we pivot` | Full 10-section structured output. All Five Truths assessed. |
| `--quick` | `/cpo --quick is freemium right for us` | One paragraph. Dominant Truth only. |
| `--memo` | `/cpo --memo should we enter healthcare` | Formats output as a printable decision memo. No headers. |
| `--silent` | `/cpo --silent should we kill this` | No calibration questions. Proceeds with stated assumptions. Flags every inference. |
| `--compare` | `/cpo --compare option-a vs option-b` | Runs two approaches side-by-side on the same input. |
| `--roadmap` | `/cpo --roadmap 4 bets` | Comparative prioritization across N bets — Five Truths per bet, ASCII timeline, kill criteria per bet. |
| `--sell-up` | `/cpo --sell-up CEO` | Reframes any decision as an internal pitch. 1-min + 5-min version, objection pre-emption, explicit ask. |
| `--save-context` | `/cpo --save-context` | Saves company context — stage, model, constraint, priorities. Loads automatically every session. |
| `--context` | `/cpo --context series-a` | Loads a named context from `~/.cpo/contexts/[name].md`. For consultants or multi-company use. |
| `--no-context` | `/cpo --no-context` | Ignores saved context. Reasons from the current prompt only. |
| `--focus` | `/cpo --focus burn-rate` | Simulation modes only — jumps directly to targeted pressure on one topic. |
| `--since` | `/cpo --since last-time` | Temporal delta — leads with what changed since a prior baseline. Pulls from decision journal. |
| `--brief` | `/cpo --brief` | Proactive weekly intelligence brief. Kill criteria alerts, unresolved decisions, pattern warnings. |
| `--trail` | `/cpo --trail` | 90-day strategic diary — all journal entries, one page. |
| `--history` | `/cpo --history enterprise` | Full decision journal. Optional keyword filter. |
| `--outcome` | `/cpo --outcome enterprise-pause` | Closes the loop on a prior decision. Records what happened. Detects path patterns over time. |
| `--export` | `/cpo --export` | Writes output to `~/.cpo/exports/YYYY-MM-DD-[slug].md` — shareable with co-founders, boards, investors. |
| `--schedule-brief` | `/cpo --schedule-brief` | Sets up a recurring weekly brief that runs automatically on a schedule you define — no manual trigger needed. Uses `anthropic-skills:schedule`. *(Claude Code only — Cursor users see manual reminder setup.)* |
| `--setup-integrations` | `/cpo --setup-integrations` | Detects MCP data sources and configures live data enrichment for Five Truths assessments. |
| `--import-context [path]` | `/cpo --import-context ./strategy.md` | Imports a context file from a specified path into `~/.cpo/context.md`. Useful for onboarding a new project or syncing context from a shared repo. |
| `--scan-strategy` | `/cpo --scan-strategy` | Alone: re-run strategic context scan and rebuild posture summary. With a question (`/cpo --scan-strategy [question]`): cross-reference strategy files against the question, surface tensions/alignments/silences, then run the four-action flow with strategy-anchored grounding options. |
| `--stack` | `/cpo --stack` | Shows the full product workflow with coverage status. Detects installed complementary skills. |
| `--update` | `/cpo --update` | Outputs update instructions: `cd ~/.claude/skills/cpo && git pull`. |

> Natural language should still be the default. Flags are best when you want format control, audience control, or repeatable workflows.

---

### Persistent layers

CPO uses shared local state so it can compound context over time. Everything lives in `~/.cpo/` — shared between Claude Code and Cursor.

| Path | What it stores | Why it matters |
|:---|:---|:---|
| `~/.cpo/context.md` | Company profile — stage, model, constraint, priorities | Keeps answers grounded in your actual company. Loads every session. |
| `~/.cpo/decisions/` | Decision journal — one YAML per major analysis | Builds a searchable strategic history. Foundation for `--brief`, `--trail`, `--since`. |
| `~/.cpo/simulations/` | Boardroom and investor roundtable transcripts | Preserves full simulation sessions |
| `~/.cpo/exports/` | Exported memos and analyses — dated and slugged | Makes outputs portable and shareable |
| `~/.cpo/integrations.md` | Live data source config — injected into Five Truths | Enriches assessments with real company data |
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
    └── flags/                  # Full flag behavior specs (15 files, loaded on demand)
        ├── brief.md
        ├── roadmap.md
        ├── sell-up.md
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

---

## Version · License

Current: **v1.4.4** — [view changelog](CHANGELOG.md)

MIT. Free forever. Go make better product decisions.
