# CPO — the product decision layer

Every AI tool today makes you faster at building. CPO is the one that makes sure you're building the right thing.

`/cpo` structures the decision before the team commits time, headcount, or capital — with kill criteria, three concrete paths, and a persistent journal that gets smarter with every decision you log.

**Standalone. No dependencies.** Works in any Claude Code session. Optionally integrates with [gstack](https://github.com/garrynsk/gstack) for full lifecycle automation.

---

## What it produces

```
/cpo should we add a free tier?
```

```
[FRAME]

I'm reading this as: whether to add a free tier to accelerate top-of-funnel growth.
Inferring post-PMF SaaS — correct me if wrong.
Door type: one-way. Pricing architecture is hard to reverse once users anchor to it.

The Economic Truth is what this turns on: free tier changes your CAC math, support
load, and conversion denominator simultaneously. [judgment]

Market scan: Notion, Linear, and Vercel all run freemium — but each anchors free to
a hard limit (pages, seats, bandwidth) that creates natural upgrade pressure. [fact]

Premise checks:
· Right problem? Top-of-funnel or activation? Free tier solves discovery, not
  engagement. If churn is the problem, a free tier won't fix it.
· Who benefits? Early-stage teams who won't pay upfront but convert if they see value.
· Prove it: What's your current free-trial-to-paid conversion rate, and have you
  measured it by acquisition channel?
· Delay test: High. Competitors are already freemium. Every month you're not is
  a month of compounding CAC disadvantage.
```

*Then it gates. You correct the frame or confirm it. Then paths. Then verdict with kill criteria. All logged automatically.*

---

## Install

```bash
git clone https://github.com/0x2kNJ/cpo.git ~/.claude/skills/cpo
```

Add to your `CLAUDE.md`:

```md
## cpo
Available skill: /cpo
Invoke when the user is making a product or strategic decision — what to build,
whether to build it, when to kill it, or how to sequence competing priorities.
```

No build step. No dependencies. Works immediately.

### First run

```bash
/cpo --save-context   # one-time: saves your stage, model, constraints
/cpo should we launch a collaboration feature this quarter?
/cpo --go should we add a free tier?          # fast path, no gates
/cpo --quick should we use Postgres or Mongo?  # ≤300 words, one criterion
```

### Upgrade

```bash
cd ~/.claude/skills/cpo && git pull
```

CPO detects version mismatches on every invocation. No daemon, no auto-upgrade — you pull when you want to update.

---

## Why not just ask Claude directly?

Claude gives you an answer. CPO makes you face the real question.

The difference: Claude will tell you whether a free tier is a good idea. CPO will ask you what your current conversion rate is, classify this as a one-way door, run a market scan, define three structurally distinct paths, and refuse to give a verdict without three kill criteria attached to it. Then it logs everything, so next time you revisit pricing, you open with what changed — not from scratch.

The value isn't the answer. It's the structure that forces better questions, and the journal that makes decisions compounding assets instead of forgotten conversations.

---

## How it works

### Three-phase flow

Every decision moves through three marked, gated phases:

**`[FRAME]`** — Classify the decision. Is it reversible? What Truth does it turn on? Four premise checks: right problem, who benefits, stage-specific forcing question, cost of delay. Ends with a structural grounding AskUserQuestion — the gate cannot be skipped.

**`[PATHS]`** — Three situationally-labeled paths (never Bold/Balanced/Conservative). Recommendation appears first. Pressure-test options before committing. Ends with another gate: commit to a path or pressure-test first.

**`[VERDICT]`** — Chosen path, confidence, three kill criteria (metric + threshold + timeframe), Truth fingerprint. Routes to K) Eng brief or L) Hand off to the next skill.

The gates are mechanical, not suggestive. `[FRAME]` and `[PATHS]` cannot proceed until you reply.

### Five Truths

Every decision is assessed across five independent dimensions:

| Truth | What it asks |
|:---|:---|
| **User** | What does the user actually want, fear, and do? (behavior > stated preference) |
| **Strategic** | Where does this move us on the competitive board? |
| **Economic** | Does the unit economics work? CAC, LTV, payback, margin at scale? |
| **Macro-Political** | What regulatory, geopolitical, or ecosystem forces could override good execution? |
| **Execution** | Can we actually build this with our current team, runway, and tech stack? |

CPO identifies the **dominant Truth** — what this decision actually turns on — then tags every claim: `[fact / assumption / inference / judgment]`.

### One-way vs two-way doors

Every decision is classified on entry.

- **Two-way door** — reversible. Low-magnitude two-way doors auto-suggest `--quick`.
- **One-way door** — not reversible. Triggers `--deep` suggestion and a **market reality check**: two web searches for competitors and market trends, surfaced as a one-line finding in the Frame.

Bezos framework, applied mechanically — not as decoration.

### Kill criteria

No recommendation without kill criteria. Every verdict includes three, each with:
- a named metric
- a specific threshold
- a timeframe

*"Stop if: free-to-paid conversion stays below 8% at 30 days."*

### Decision journal

Every verdict is automatically logged to `~/.cpo/decisions/`:

```yaml
decision_id: free-tier-add
date: 2026-03-22
decision: Whether to add a free tier to the SaaS product
verdict: Gated free tier with team-invite expansion loop
confidence: M
kill_criteria:
  - Free-to-paid conversion below 8% at 30 days
  - Paid ARR growth below 15% QoQ after 2 quarters
  - Support ticket volume up more than 40% in 90 days
status: active
```

The journal compounds. CPO scans the last 5 decisions at session start and surfaces any related prior decisions before framing. Every session starts smarter than the last.

---

## Flags

### `--go`
All three phases in one response. No gates, no premise checks. Use when you've already thought through the frame.

```
/cpo --go should we build mobile before web is profitable?
```

### `--quick`
Single response, ≤300 words, one kill criterion. Use for low-stakes decisions.

### `--deep`
Full 10-section expansion after paths: Problem Definition → Five Truths → Strategic Options → Recommendation → Sequencing → Risks → GTM → Org implications → Open Questions → Decision Memo. Auto-triggered for one-way doors.

### `--journal`
Shows the 10 most recent decisions. Read-only — does not trigger a new flow.

### `--review`
Surfaces all active decisions and asks for current data against each kill criterion. Use weekly or before major check-ins. CPO surfaces, doesn't evaluate — it asks you for the data.

### `--outcome #name`
Close the loop on a past decision. Reconstructs the information state at decision time, walks through kill criteria, writes an outcome block. Three close modes:
- **Walk through** — criterion by criterion (recommended for one-way doors)
- **Quick close** — one-line summary of what happened
- **Decision was wrong** — decision replay to understand why

After closing: *"This is your 8th closed decision. 5 succeeded, 2 pivoted, 1 failed. Most common failure mode: underestimating execution time."*

Active decisions older than 30 days are surfaced automatically: *"You have 2 decisions that haven't been closed — run `/cpo --outcome #[id]`."*

### `--save-context`
Bootstrap `~/.cpo/context.md` with 5 questions — stage, business model, core constraint, top priorities, biggest open question. Every future session loads this automatically.

### `--decide`
Inbound handoff from other tools. Any skill can route a decision fork to CPO via a structured handoff block. CPO skips redundant premise checks and routes back after verdict. See `references/handoff-contract.md`.

### `#name`
Tag any decision for addressability:
```
/cpo #pricing should we add a free tier?
```
When you return to a named decision, CPO opens with a **delta frame** — what's changed since the last call — instead of starting from scratch.

---

## Stage doctrine

CPO auto-detects your stage from context. Without context, it infers and flags every inference.

| Stage | Doctrine |
|:---|:---|
| Pre-PMF / seed | Do things that don't scale. First 10 users. 90/10 product. |
| Post-PMF / growth | NRR, expansion motion, compounding loops. |
| Series B+ | Rule of 40, CAC payback, path to exit. |

---

## Who this is for

**Solo founders** — deciding what to build, whether to pivot, and when to kill a bet before it bleeds the runway.

**PMs and Heads of Product** — building the case upward and laterally with structured reasoning instead of slide decks.

**Product-minded execs** — pressure-testing strategy before it becomes roadmap, headcount, or board narrative.

**Teams before high-stakes conversations** — investor meetings, board reviews, strategic offsites, roadmap planning.

---

## Red flags CPO auto-escalates

- Strategy dependent on a competitor making a mistake
- Roadmap with no kill criteria
- GTM with no clear ICP
- Unit economics that only work at 10x current scale
- "We have no choice" (there is always a choice)
- Technical debt rationalized as "we'll fix it after launch"

---

## gstack integration

CPO works standalone. If you also use [gstack](https://github.com/garrynsk/gstack), it becomes the decision gate for the full dev lifecycle.

### Where CPO fits

```
THINKING              DECIDING              EXECUTING             FEEDBACK
─────────             ─────────             ─────────             ─────────
/office-hours    ──►  /cpo             ──►  /plan-eng-review  ──►  /retro
/plan-ceo-review ──►  (the only skill  ──►  /build            ──►  /review
/retro           ──►   that can say    ──►  /ship             ──►  /qa
                        don't build)   ──►  /land-and-deploy  ──►  /canary
```

Every skill before CPO generates options. Every skill after it commits resources. CPO is the gate.

### Outbound: CPO → gstack

After `[VERDICT]`, option **L) Hand off** routes to the right next skill:

| Situation | Routes to |
|:---|:---|
| Architecture / implementation | `/plan-eng-review` |
| Scope expansion | `/plan-ceo-review` |
| New idea, unvalidated premise | `/office-hours` |
| Design direction | `/plan-design-review` |
| Ready to build | `/build` |
| Ready to ship | `/ship` or `/land-and-deploy` |
| Post-launch monitoring | `/canary` |
| Historical pattern check | `/retro` |

**K) Eng brief** saves a structured brief to `~/.cpo/briefs/` and routes directly to `/plan-eng-review`.

### Inbound: gstack → CPO

Any gstack skill can route a decision fork to CPO via the `--decide` flag and a handoff block. CPO handles the decision and routes back. See `references/handoff-contract.md`.

### Signal bus (bidirectional)

CPO reads red-flag signals from other skills at session start. If `severity: red`, the signal surfaces in `[FRAME]` before the decision is framed.

| Signal | Written by | CPO surfaces when |
|:---|:---|:---|
| `qa-latest.yaml` | `/qa` | QA found critical failures |
| `review-latest.yaml` | `/review` | Code review flagged blockers |
| `retro-latest.yaml` | `/retro` | Recurring team failure pattern |
| `canary-latest.yaml` | `/canary` | Production regressions detected |

After every verdict, CPO writes `~/.cpo/signals/cpo-latest.yaml` — decision summary, door type, confidence, kill criteria count. Downstream skills (`/build`, `/review`, `/retro`) read this to verify a decision exists before committing work.

> **Note:** Signal integration requires skills to write to `~/.cpo/signals/`. Native gstack signals require local configuration — see `references/handoff-contract.md`. CPO degrades gracefully when no signals are present.

### Known limitation

CPO discovery triggers in gstack skill files — e.g., suggesting `/cpo` from `/office-hours` — are local modifications that `gstack-upgrade` overwrites. After upgrading gstack, re-apply triggers manually or contribute CPO discovery upstream.

### Full skill reference

Complete documentation of all 25+ gstack skills and their relationship to CPO: [`docs/gstack-skills.md`](docs/gstack-skills.md)

---

## State files

| Path | Contents |
|:---|:---|
| `~/.cpo/context.md` | Company context saved via `--save-context` |
| `~/.cpo/decisions/*.yaml` | Decision journal entries |
| `~/.cpo/briefs/*.md` | Eng briefs saved via K) option |
| `~/.cpo/signals/*-latest.yaml` | Signals from other skills |

## Reference files

Loaded on demand — not pre-loaded:

| File | Contents |
|:---|:---|
| `references/examples.md` | Worked examples: pricing flow, `--go` mobile/web decision |
| `references/frameworks.md` | Five Truths detail, kill criteria patterns, stage frameworks |
| `references/handoff-contract.md` | CPO Handoff Contract for skill authors |

---

## Why I built this

When execution becomes abundant, judgment becomes scarce. Every AI tool today accelerates shipping — but bad strategy plus great execution is still failure.

The hardest problems I've encountered were never about execution. They were upstream: which opportunity is worth pursuing, which assumptions will kill you, what is the right path for this stage, and how do you know when to stop.

CPO encodes that way of working — think clearly, test tradeoffs honestly, pressure-test bets before they become expensive, and build a record that makes every future decision better informed than the last.

If more people now have the power to build, they also need better tools for deciding what deserves to be built.

---

> CPO is not a prompt. It's a system: structured reasoning, mechanical gates, persistent memory, and a compounding decision log — working together across every major product decision.
