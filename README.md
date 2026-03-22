# CPO — the product decision layer

**v4.0.0** · The operating system for product decisions.

`/cpo` helps founders, PMs, and execs decide **what to build, whether to build it, how to communicate it, and when to kill it** — before the team commits time, headcount, or capital.

It sits between **"we have an idea"** and **"engineering is building it."**

---

## The vision

CPO's end goal is to **orchestrate an entire product organization** — not just advise on one decision, but manage the full surface area of product judgment across time.

Today, every AI tool in the product ecosystem accelerates execution. CPO is the missing layer that accelerates *judgment*.

**Where CPO is today:** A strategic advisor that frames decisions across Five Truths, explores Three Paths before recommending one, defines kill criteria upfront, and logs everything in a persistent journal that compounds context over time. Every session starts smarter than the last.

**Where CPO is going:** A full product operating system where decisions flow automatically from strategic framing to execution handoff to outcome tracking — where the system monitors kill criteria across all active bets, detects when your team's implicit strategy has drifted from your explicit one, and autonomously routes decisions to the right execution tools.

---

## Quick start

### Install

```bash
git clone https://github.com/0x2kNJ/cpo.git ~/.claude/skills/cpo
```

Then add to your `CLAUDE.md`:

```md
## cpo
Available skill: /cpo
```

No build step. No dependencies. Invoke with `/cpo` in any Claude Code session.

### Upgrade

```bash
cd ~/.claude/skills/cpo && git pull
```

CPO detects version mismatches automatically on every invocation. If the installed version (`~/.cpo/.version`) differs from the skill version, CPO updates it silently and tells you what changed. No auto-upgrade daemon — you pull when you want to update.

### Save your context (one-time)

```
/cpo --save-context
```

CPO asks 5 questions — stage, business model, core constraint, top priorities, biggest open question — and saves them to `~/.cpo/context.md`. Every future session loads this automatically. Without it, CPO infers from the prompt and flags all inferences.

### Run your first decision

```
/cpo should we launch a collaboration feature this quarter?
/cpo what should we build for onboarding activation?
/cpo --go should we add a free tier?
```

---

## Why I built this

When execution becomes abundant, judgment becomes scarce. Every AI tool today accelerates shipping — but bad strategy plus great execution is still failure.

Over many years leading strategy and building products across financial services and web3, I found that the hardest problems were never about execution. They were upstream: which opportunity is worth pursuing, which assumptions will kill you, what is the right path for this stage, and how do you know when to stop.

CPO encodes that way of working — think clearly, test tradeoffs honestly, pressure-test bets before they become expensive, and iterate without losing the plot.

If more people now have the power to build, they also need better tools for deciding what deserves to be built.

---

## How CPO works

### The flow

CPO structures every decision across three phases, each marked and gated:

**`[FRAME]`** — State the decision. Classify the door type. Surface the dominant Truth. Present premise checks. Gate enforced by AskUserQuestion: three structural grounding angles + D) Correct my framing.

**`[PATHS]`** — Three paths with situational verb-phrase labels. A recommendation with rationale. Pressure-test options (Stress test / Deep dive / Reality check). Gate enforced by AskUserQuestion: A/B/C to commit, 1/2/3 to pressure-test first.

**`[VERDICT]`** — Chosen path, confidence level, three kill criteria (metric + threshold + timeframe), Truth fingerprint. Offers D) Stress test · E) Deep dive · F) Reality check · K) Eng brief · L) Hand off.

The gates are structural, not suggestive. `[FRAME]` and `[PATHS]` cannot proceed until the user replies to the AskUserQuestion call. This is not a STOP comment — it's a mechanical gate.

### Premise checks

Every `[FRAME]` includes:

- **Right problem?** — Root cause or symptom?
- **Who benefits?** — Specific user + human outcome (grounds the User Truth)
- **Prove it** — Stage-specific forcing question: *Who specifically is paying for this today?* (pre-PMF) / *What's your churn/conversion rate on this segment?* (post-PMF) / *What's the payback period on this bet?* (Series B+)
- **Delay test** — Cost of delay: high or low, and why. If low: *"Low urgency — you could defer 90 days without material cost."*

### Door type classification

Every decision is classified on entry:

- **Two-way door** — reversible. Low-magnitude two-way doors auto-suggest `--quick`.
- **One-way door** — not reversible. One-way doors auto-suggest `--deep` and trigger a **market reality check** (two quick web searches for competitors and market trends, surfaced as a one-line finding in the Frame).

This is the Bezos framework applied mechanically — not as decoration.

### Five Truths

Every decision is assessed across five independent dimensions:

| Truth | What it asks |
|:---|:---|
| User | What does the user actually want, fear, and do? (behavior over stated preference) |
| Strategic | Where does this move us on the competitive board? |
| Economic | Does the unit economics work? CAC, LTV, payback, margin at scale? |
| Macro-Political | What regulatory, geopolitical, or ecosystem forces could override good execution? |
| Execution | Can we actually build this with our current team, runway, and tech stack? |

CPO identifies the **dominant Truth** (what this decision turns on), then tags every claim with an evidence level: `[fact / assumption / inference / judgment]`.

### Three Paths

CPO never jumps straight to one answer. It always explores three paths with situational labels derived from the decision — never "Bold / Balanced / Conservative."

Examples:
- *"Concentrate on [focus] / Sequence by [dependency] / Defer [conflict]"*
- *"Community-led / Product-led / Partner-led"*
- *"Resolve toward [X] / Resolve toward [Y] / Time-box [fork]"*

The recommendation appears before the path list as a dedicated block — always visible at the top.

### Kill criteria

Before recommending a path, CPO defines what would make the bet fail. Each kill criterion has:
- a named metric
- a specific threshold
- a timeframe

No recommendation without kill criteria. This is a hard rule.

### Decision journal

Every verdict is automatically logged to `~/.cpo/decisions/` as a YAML entry:

```yaml
decision_id: free-tier-add
date: 2026-03-22
decision: Whether to add a free tier to the SaaS product
verdict: Gated free tier with team-invite expansion
confidence: M
kill_criteria:
  - Free-to-paid conversion below 8% at 30 days
  - Paid ARR growth below 15% QoQ after 2 quarters
  - Support ticket volume increase above 40% in 90 days
status: active
```

The journal compounds context over time. CPO scans the last 5 decisions at session start and surfaces any related prior decisions before framing.

### Prior art scan

Every session starts with a scan of recent decisions. If keywords match a prior decision:

> *"Related prior decision: #free-tier — Gated expansion path (2026-01-15). Revisiting or new question?"*

Tag any decision with `#name` (e.g., `/cpo #pricing should we add a free tier?`) to create an addressable record. When returning to a named decision, CPO opens with a **delta frame** — what's changed since the last call — instead of starting from scratch.

### Signals integration (bidirectional)

**Inbound:** CPO reads red-flag signals from other skills (`~/.cpo/signals/*-latest.yaml`). If any signal shows `severity: red`, it surfaces in the Frame:

> *"Note: [skill] flagged [summary] ([N] days ago). This may affect your decision."*

**Outbound:** After every verdict (or `[GO]` response), CPO writes `~/.cpo/signals/cpo-latest.yaml` with decision summary, door type, confidence, and kill criteria count. Other skills (`/build`, `/review`, `/retro`) can read this to check if a decision exists before implementation.

---

## Flags

### `--go`

All three phases in one response. No grounding question, no premise checks, no gates. Use when you've already thought through the frame and want the fast path.

```
/cpo --go should we build a mobile app before web is profitable?
```

### `--quick`

Single response, ≤300 words, one kill criterion. No premise checks, no Truth fingerprint. Use for low-stakes decisions you need answered fast.

### `--deep`

Replaces the 1/2/3 pressure-test block with a full 10-section expansion:

1. Problem Definition
2. Five Truths Assessment (all five, independently)
3. Strategic Options
4. Recommendation + Kill Criteria
5. Sequencing & Dependencies
6. Risks & Mitigations
7. GTM Considerations
8. Organizational Implications
9. Open Questions
10. Decision Memo

Use for one-way doors, high-magnitude bets, or anything that warrants a full written record.

### `--journal`

Shows the 10 most recent decision journal entries. Read mode only — does not trigger a new decision flow.

```
/cpo --journal
```

### `--review`

Surfaces all active decisions and asks for current data against each kill criterion. CPO doesn't evaluate — it surfaces and asks. Use weekly or before major check-ins.

```
/cpo --review
```

### `--outcome`

Close the loop on a past decision. Reconstructs the information state at decision time, walks through each kill criterion asking for current data, and writes an outcome block to the journal entry.

```
/cpo --outcome #pricing
```

Three close modes:
- **Walk through** — evaluate each kill criterion one at a time (recommended for one-way doors)
- **Quick close** — one-line summary of what happened
- **Decision was wrong** — decision replay to understand why

After closing, CPO surfaces patterns across all closed decisions: *"This is your 8th closed decision. 5 succeeded, 2 pivoted, 1 failed. Most common failure mode: underestimating execution time."*

The preamble also nudges stale decisions — any active decision older than 30 days gets surfaced: *"You have 2 decisions older than 30 days that haven't been closed. Run `/cpo --outcome #[id]` to close the loop."*

### `--save-context`

Bootstrap or update `~/.cpo/context.md` with 5 questions asked one at a time:
1. Stage (pre-PMF / post-PMF / Series B+)
2. Business model (SaaS / marketplace / API / other)
3. Core constraint (time / money / people / tech)
4. Top 3 priorities right now
5. Biggest open question

### `--decide`

Inbound handoff from other skills. Any skill can route a decision fork to CPO via a structured `CPO Handoff Request` block. CPO receives the context, skips "Right problem?" / forcing question / delay test (the calling skill validated urgency), keeps "Who benefits?", and runs the standard flow. After verdict, it suggests returning to the calling skill.

See `references/handoff-contract.md` for the full contract format and implementation guide.

### `#name`

Tag any decision with a name to create an addressable journal entry:

```
/cpo #pricing should we add a free tier?
```

When you return to a named decision, CPO opens with a delta frame. `#name` takes precedence over the prior art rule — no double-surfacing.

---

## Stage doctrine

| Stage | CPO Doctrine |
|:---|:---|
| Pre-PMF / seed | Do things that don't scale. First 10 users. 90/10 product. |
| Post-PMF / growth | NRR, expansion motion, compounding loops. |
| Series B+ | Rule of 40, CAC payback, path to exit. |

CPO auto-detects stage from context. Without context, it infers from the prompt and flags all inferences.

---

## Works best when…

- you have a real decision, not a vague topic
- you can name your company stage
- you are willing to compare multiple paths, not just seek validation
- you want an opinionated sparring partner, not a note-taker
- you want a decision record, not just one-off outputs

---

## Who this is for

**Solo founders** — deciding what to build, whether to pivot, and when to kill a bet.

**PMs and Heads of Product** — building the case upward and laterally with structured reasoning.

**Product-minded execs** — pressure-testing strategy before it becomes roadmap, spend, or narrative.

**Teams before high-stakes conversations** — investor meetings, board meetings, roadmap reviews, strategic offsites.

---

## gstack integration

CPO is the decision layer for the [gstack](https://github.com/garrynsk/gstack) skill stack — the missing node between "we have an idea" and "engineering is building it."

### Where CPO fits in the ecosystem

```
THINKING              DECIDING              EXECUTING             FEEDBACK
─────────             ─────────             ─────────             ─────────
/office-hours    ──►  /cpo             ──►  /plan-eng-review  ──►  /retro
/plan-ceo-review ──►  (product          ──►  /plan-design-review──►  /review
/retro           ──►   decision layer)  ──►  /build            ──►  /qa
                                        ──►  /ship             ──►  /canary
                                        ──►  /land-and-deploy  ──►  /benchmark
```

CPO is the only skill in the stack that can say "don't build this." Every skill before it generates options. Every skill after it commits resources.

---

### Full gstack skill reference

#### THINKING — feeds into CPO

| Skill | Role relative to CPO |
|:---|:---|
| `/office-hours` | YC-style forcing questions that expose demand reality before a decision is framed. When a strategic fork emerges, `/office-hours` can hand off to CPO via `--decide`. |
| `/plan-ceo-review` | CEO/founder-mode plan review: challenges premises, expands scope, finds the 10-star product. Routes scope-level decisions to CPO when product prioritization is needed. |
| `/retro` | Weekly engineering retrospective. Surfaces recurring patterns and team signals — including historical CPO decisions — that inform the current bet. Writes `~/.cpo/signals/retro-latest.yaml` for CPO to read at session start. |

#### DECIDING — CPO's layer

| Skill | Role |
|:---|:---|
| `/cpo` | The product decision layer. Frames decisions, explores three paths, defines kill criteria, and logs outcomes. Reads signals from QA, retro, and review. Writes `~/.cpo/signals/cpo-latest.yaml` for downstream skills. |

#### PLANNING — after a decision is made

| Skill | Role relative to CPO |
|:---|:---|
| `/plan-eng-review` | Eng manager-mode plan review: architecture, data flow, edge cases, test coverage. CPO's K) Eng brief handoff saves a structured brief and routes here to lock in the implementation plan. |
| `/plan-design-review` | Designer's eye plan review: rates each design dimension, fixes issues iteratively. CPO routes here when the decision involves a significant UI/UX bet. |
| `/design-consultation` | Full design system proposal: aesthetic, typography, color, layout, spacing, motion. Use when the decision opens a new product surface that needs a design language from scratch. |

#### EXECUTING — commits resources, CPO decision already made

| Skill | Role relative to CPO |
|:---|:---|
| `/build` | Parallel implementation orchestrator. Decomposes plans into tasks, dispatches agents with worktrees. Reads `~/.cpo/signals/cpo-latest.yaml` to check a decision exists before starting. CPO's L) Hand off routes here when ready to build. |
| `/ship` | Release engineer mode: sync main, run tests, bump version, push branch, open PR. CPO routes here when a PR needs to be created. |
| `/land-and-deploy` | Merges the PR, waits for CI, verifies production health via canary. CPO routes here when the decision is to ship to production (vs. just create a PR). |
| `/setup-deploy` | Configures deployment settings for `/land-and-deploy`. Detects platform (Fly.io, Render, Vercel, Netlify, Heroku, GitHub Actions). Run once before the first `/land-and-deploy`. |

#### FEEDBACK — closes the loop, writes signals CPO reads

| Skill | Role relative to CPO |
|:---|:---|
| `/canary` | Post-deploy canary monitoring: watches the live app for console errors, performance regressions, and page failures. Writes `~/.cpo/signals/canary-latest.yaml`. CPO surfaces red canary signals in `[FRAME]`. |
| `/qa` | Systematic QA testing — runs tests, fixes bugs, commits atomically. Writes `~/.cpo/signals/qa-latest.yaml`. CPO surfaces red QA signals in `[FRAME]`. |
| `/qa-only` | Report-only QA: produces a health score and repro steps without touching code. Use when you want signal without automated fixes. |
| `/review` | Pre-landing PR review: SQL safety, LLM trust boundary violations, structural issues. Reads `~/.cpo/signals/cpo-latest.yaml` to check if a decision backs the implementation before approving. |
| `/benchmark` | Performance regression detection: baselines page load times and Core Web Vitals. Use after deploy to verify the build didn't regress. |
| `/retro` | Also closes the feedback loop: surfaces what went wrong, what recurred, what the team learned. CPO's `--outcome` mode cross-references retro findings with original kill criteria. |

#### QUALITY & SAFETY — available at any phase

| Skill | When to use |
|:---|:---|
| `/design-review` | Designer's eye QA: finds visual inconsistency, spacing issues, AI slop patterns. Use after build, before ship. |
| `/investigate` | Systematic debugging: four phases (investigate, analyze, hypothesize, implement). Iron Law: no fixes without root cause. |
| `/document-release` | Post-ship documentation update: reads all docs, cross-references the diff, updates README/ARCHITECTURE/CONTRIBUTING. Run after `/land-and-deploy`. |
| `/codex` | OpenAI Codex CLI wrapper: independent code review, adversarial mode, or test generation. Use for a second opinion on the implementation. |
| `/review` | (see Feedback above) |
| `/careful` | Warns before destructive commands (rm -rf, DROP TABLE, force-push). Lightweight safety layer. |
| `/guard` | Full safety mode: combines `/careful` with directory-scoped edits. Use during high-risk operations. |
| `/freeze` / `/unfreeze` | Restrict edits to a specific directory. Use when debugging to prevent scope creep. |

#### BROWSER & VISUAL — verification and QA support

| Skill | When to use |
|:---|:---|
| `/browse` | Fast headless browser for QA testing and dogfooding. Navigate, interact, diff before/after actions, take annotated screenshots. |
| `/setup-browser-cookies` | Import cookies from your real browser into the headless browse session for authenticated testing. |

---

### Signal flow

CPO participates in a bidirectional signal bus with other gstack skills:

**Inbound signals CPO reads** (`~/.cpo/signals/`):

| Signal file | Written by | CPO action |
|:---|:---|:---|
| `qa-latest.yaml` | `/qa`, `/qa-only` | If `severity: red`, surfaces in `[FRAME]` before the decision |
| `review-latest.yaml` | `/review` | If `severity: red`, surfaces in `[FRAME]` |
| `retro-latest.yaml` | `/retro` | If `severity: red`, surfaces in `[FRAME]` |
| `canary-latest.yaml` | `/canary` | If `severity: red`, surfaces in `[FRAME]` |

**Outbound signal CPO writes** (`~/.cpo/signals/cpo-latest.yaml`):

Written after every `[VERDICT]` or `[GO]`. Contains: decision summary, door type, confidence, kill criteria count, timestamp. Read by `/build`, `/review`, and `/retro` to verify a decision backs the implementation.

---

### Routing: CPO → gstack

After `[VERDICT]`, option **L) Hand off** routes to the best next skill:

| Situation | Routed to |
|:---|:---|
| Architecture/implementation needed | `/plan-eng-review` |
| Scope expansion warranted | `/plan-ceo-review` |
| New idea needs premise pressure-test | `/office-hours` |
| Design direction needed | `/plan-design-review` or `/design-consultation` |
| Ready to build | `/build` |
| Ready to ship (PR needed) | `/ship` |
| Ready to merge + deploy | `/land-and-deploy` |
| Post-launch monitoring | `/canary [url]` |
| Historical pattern check | `/retro` |

Option **K) Eng brief** saves a structured brief to `~/.cpo/briefs/YYYY-MM-DD-[slug].md` and suggests `/plan-eng-review`.

---

### Routing: gstack → CPO

Any gstack skill can route decision forks to CPO via the `--decide` flag and a `CPO Handoff Request` block:

```
**CPO Handoff Request**
From: [skill name]
Context: [1-3 sentences]
Decision: [the fork — one sentence]
Options considered: [optional]
```

CPO skips redundant premise checks (the calling skill validated urgency), keeps "Who benefits?", and runs the standard flow. After verdict, it suggests returning to the calling skill. See `references/handoff-contract.md` for the full contract.

---

### Works standalone

CPO does not require gstack. If a suggested skill isn't installed: *"I'd suggest [skill] for this — install gstack to get it."*

**Known limitation:** CPO discovery triggers in gstack skill files (e.g., suggesting `/cpo` from `/office-hours` or `/plan-ceo-review`) are local modifications that get overwritten by `gstack-upgrade` (which runs `git reset --hard`). After upgrading gstack, re-apply the triggers manually. Long-term fix: contribute CPO discovery upstream to gstack.

---

## Red flags CPO auto-escalates

- Strategy dependent on a competitor making a mistake
- Roadmap with no kill criteria
- GTM with no clear ICP
- Unit economics that only work at 10x current scale
- "We have no choice" (there is always a choice)
- Technical debt rationalized as "we'll fix it after launch"

---

## Reference files

Loaded on demand only (not pre-loaded):

| File | Contents |
|:---|:---|
| `references/examples.md` | Two worked examples: pricing decision flow, `--go` mobile/web decision |
| `references/frameworks.md` | Five Truths extended detail, kill criteria patterns, stage-specific frameworks |
| `references/handoff-contract.md` | CPO Handoff Contract spec for skill authors implementing `--decide` routing |

---

## State files

| Path | Contents |
|:---|:---|
| `~/.cpo/context.md` | Company context saved via `--save-context` |
| `~/.cpo/decisions/*.yaml` | Decision journal entries |
| `~/.cpo/briefs/*.md` | Eng briefs saved via K) option in `[VERDICT]` |
| `~/.cpo/signals/*-latest.yaml` | Red-flag signals from other skills |

---

> CPO is not a prompt. It's a system: structured reasoning, mechanical gates, persistent memory, and a compounding decision log — working together across every major product decision.
