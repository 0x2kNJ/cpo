# CPO — The Product Operating System

**The product framework that sits between "we have an idea" and "the engineering team is building it."**

Every decision a founder, PM, or senior executive needs to make across the product lifecycle before committing to execution. Opportunity assessment, go/no-go decision, roadmap prioritization, investor and board feedback simulations, pitch-building and a decision log that builds context over time.

---

## Start here

```
New idea — where's the opportunity?       → /cpo what should we build
Need a go/no-go?                          → /cpo should we do this
Heading into a raise?                     → /cpo simulate an investor meeting
Preparing for board?                      → /cpo simulate the board meeting
Building the case upward?                 → /cpo --sell-up CEO
Weekly kill-criteria check?               → /cpo --brief
First time here?                          → /cpo ?
```

If this helps you ship better decisions, ⭐ the repo.

---

## Why it exists

Generic AI gives you text. `/cpo` gives you structure.

You're about to make a $500K decision — a pivot, a raise, a new market, a platform bet — and you have no one to pressure-test it with. `/cpo` is that person.

It runs Five Truths against every decision (User · Strategic · Economic · Macro-Political · Execution), always gives you Three Paths before a recommendation (Bold · Balanced · Conservative), labels every claim as *fact / assumption / inference / judgment*, and names kill criteria upfront — before you commit. It remembers your company context across sessions, logs every major decision in a journal, and can simulate an investor panel or board meeting live.

The decision log is the sleeper feature. Every major decision is timestamped, reasoned, and searchable. Run `/cpo --brief` for a weekly intelligence digest — kills criteria approaching, patterns in your decision-making, unresolved decisions that need follow-up. One year in, you'll have a searchable library of every strategic decision your company made, the reasoning at the time, and what actually happened.

Twenty modes. Eighteen flags. One command.

---

## Install — takes 30 seconds

**Requirements:** Claude Code, Git

```bash
git clone https://github.com/0x2kNJ/CPO.git ~/.claude/skills/cpo
```

Then add to your `CLAUDE.md`:

```markdown
## cpo
Available skill: /cpo
```

Invoke with `/cpo` in any Claude Code session. No build step. No dependencies.

**For Cursor:** `@CURSOR.md` at the start of any conversation, or add to `.cursorrules` for always-on:

```
# .cursorrules
@~/.claude/skills/cpo/CURSOR.md
```

The same `~/.cpo/` state is shared between Claude Code and Cursor — decision journal, context, exports, all of it.

---

## Who this is for

- **Solo founders** deciding whether to pivot, raise, hire, or kill a bet with incomplete information
- **PMs and Heads of Product** building the case upward to leadership or lateral to eng
- **Scaling teams** preparing for investor meetings, board meetings, or post-mortems
- **Anyone** who needs a smart, opinionated co-thinker to pressure-test a $500K+ decision

---

## Works best when...

- You have a specific decision to make, not a vague question ("should we pivot?" vs "is our burn acceptable?")
- You can name your stage (seed / post-PMF / Series B / etc) — `/cpo` adapts doctrine to stage
- You're willing to debate all three paths, not just looking for validation of a path you've already chosen
- You're ready to be surprised — `/cpo` doesn't tell you what you want to hear

---

## See it work

```
/cpo simulate my Series B board meeting. Lead VC is skeptical of our burn rate.
```

> *Setting up the room. Lead VC: skeptical. Financial VC: neutral. Independent Director: neutral.*
>
> **Lead VC:** "Before we get to the deck — I pulled your bank statement this morning. You told this board in January that burn efficiency would improve in Q2. It's March. Walk me through what changed, and why we should believe the Q3 forecast."

You respond in real time. The Lead VC goes off-agenda. The Financial VC challenges a different number. The Independent Director raises a governance question. The simulation ends with a debrief: what you committed to, where you lost the room, what to fix before the real meeting.

---

```
/cpo should we kill our enterprise motion and go full PLG
```

> *Reading context: post-PMF B2B SaaS — optimizing for NRR and expansion.*
>
> **Dominant Truth:** Economic. Enterprise ACV is funding PLG runway — killing it now would cut 60% of revenue before PLG can fill the gap.
>
> **Three Paths:**
> **Bold** — Kill enterprise in Q3, move entire GTM to PLG. Requires 18 months runway. Burn rises before it falls.
> **Balanced** — Freeze new enterprise logos. Expand only from existing accounts. Build PLG lane in parallel, flip when it hits 40% of pipeline.
> **Conservative** — Keep enterprise motion unchanged. Build PLG as a separate SKU, validate conversion rate before any reallocation.
>
> **Recommendation:** Balanced. Kill criteria: if enterprise expansion ARR drops below $80K/month, stop the hold and re-evaluate.

One decision. Three paths. Kill criteria named before you commit.

---

```
/cpo --brief
```

> *Weekly brief — 3 items need your attention:*
>
> **Kill criterion approaching:** CAC/LTV ratio at 1:2.8 — target was 1:4 by Q2. [context, 2026-01-14]
> **Decision pending close:** Enterprise pause (2026-02-11) — outcome never logged. Run `/cpo --outcome enterprise` to close the loop.
> **Pattern warning:** 3 of your last 4 decisions chose the Balanced path. You may be systematically avoiding the Bold call.

---

## What it does

| Capability | Commands |
|---|---|
| **Opportunity analysis** | Where's the opportunity? What should we build? What are our riskiest assumptions? |
| **Go/no-go decisions** | Should we do this? Build vs buy? Red-team the plan. Pre-mortem before we commit. |
| **Roadmap & prioritization** | `--roadmap` — comparative scoring across bets, dependency map, ranked stack with kill criteria |
| **Investor & board simulations** | Live 5-round investor debate · Live board meeting — archetypes push back, disagree, surface governance dynamics |
| **Pitch-building** | `--sell-up [CEO/board/eng]` — narrative spine, objection map, explicit ask. For any audience. |
| **Decision journal** | `--brief` weekly intelligence · `--trail` 90-day diary · `--since` temporal delta · `--outcome` close the loop |

---

## The team

| Mode | Your specialist | What they do |
|---|---|---|
| `ceo` | Strategic Advisor | Go/no-go on any decision. Three Paths, kill criteria, confidence level. |
| `blue-ocean` | Opportunity Mapper | Map uncontested space, rank bets, sequence what to do first. |
| `sequence` | Prioritization Lead | Order the roadmap by leverage, reversibility, and strategic unlock. |
| `discovery` | Assumption Hunter | Surface the bets most likely to be wrong before you commit. |
| `gtm` | GTM Designer | ICP, channels, growth loop, retention. Full go-to-market. |
| `narrative` | Positioning Lead | Category design, story, differentiation. |
| `launch-os` | Launch Planner | Readiness score, sequencing, failure modes, launch protocol. |
| `investor-story` | Pitch Builder | Narrative spine, objection map, proof point gaps for your raise. |
| `red-team` | Attacker | Find the plan's fatal flaw before your competitors or investors do. |
| `premortem` | Risk Simulator | Assume it failed — reconstruct why, rank causes by probability. |
| `postmortem` | Retrospective Lead | What actually happened, what the pattern means, what to change. |
| `org-design` | Structure Advisor | Team shape, reporting lines, hiring sequence for your stage. |
| `board-memo` | Board Writer | Shareable written board update — principled, printable, complete. |
| `board-story` | Board Presenter | Board deck narrative, talking points, pre-read materials. |
| `eng-brief` | Eng Translator | Turn strategy into an engineering spec the team can build from. |
| `eng-translate` | Tech Decoder | Decode what engineering is actually telling you. |
| `advisory-roundtable` | Expert Panel | Structured expert debate — product, growth, market archetypes. |
| `boardroom` | Live Board Simulation | Multi-turn board meeting. Archetypes push back. You respond. Debrief at end. |
| `investor-roundtable` | Live Investor Debate | 5-round investor panel — 5 archetypes, distinct voices, per-archetype verdict. |
| `upward-pitch` | Internal Pitch Builder | Build the case to convince your CEO, board, or cross-functional peers. |

---

## All flags

| Flag | Effect |
|---|---|
| `--go` | Skip menus. Route directly. Get the answer. Also skips simulation gate. |
| `--deep` | Full 10-section structured output. All Five Truths assessed. |
| `--quick` | One paragraph. Dominant Truth only. |
| `--memo` | Output as a printable decision memo. No headers. |
| `--silent` | No calibration questions. Proceed with stated assumptions. Flags every inference. |
| `--compare` | Run two approaches side-by-side on the same input. |
| `--roadmap [N bets]` | Comparative prioritization — compressed Five Truths per bet, ASCII timeline, kill criteria per bet. |
| `--sell-up [audience]` | Reframe any decision as an internal pitch. 1-min + 5-min version, objection pre-emption, explicit ask. |
| `--save-context` | Save company context — loads automatically every session. |
| `--context [name]` | Load a named context. For consultants or multi-company use. |
| `--no-context` | Ignore saved context. Reason from prompt only. |
| `--focus [topic]` | Simulation modes only — jump directly to targeted pressure on one topic. |
| `--since [date]` | Temporal delta — what changed since a prior baseline. |
| `--brief` | Proactive weekly intelligence brief. Kill criteria alerts, unresolved decisions, pattern warnings. |
| `--trail` | 90-day strategic diary — all journal entries, one page. |
| `--history [keyword]` | Full decision journal. Optional keyword filter. |
| `--outcome [topic]` | Close the loop on a prior decision. Detect Bold/Balanced/Conservative patterns over time. |
| `--export` | Write output to `~/.cpo/exports/YYYY-MM-DD-[slug].md` — shareable with co-founders, boards, investors. |
| `--schedule-brief` | Set up a recurring weekly brief. Auto-runs `/cpo --brief` without you having to remember. |
| `--setup-integrations` | Detect MCP data sources and configure live data enrichment for Five Truths. |
| `--stack` | Show the full product workflow with coverage status. Detects installed complementary skills. |
| `--update` | Pull the latest version from GitHub. |

---

## Persistent layers

Everything lives in `~/.cpo/` — shared between Claude Code and Cursor.

| Path | What it stores |
|---|---|
| `~/.cpo/context.md` | Company profile — stage, model, constraint, priorities. Loads every session. |
| `~/.cpo/decisions/` | Decision journal — one YAML per major analysis. Foundation for `--brief`, `--trail`, `--since`. |
| `~/.cpo/simulations/` | Boardroom and investor roundtable transcripts. Full session saved. |
| `~/.cpo/exports/` | Shareable Markdown files — one per `--export`. Dated and slugged. |
| `~/.cpo/integrations.md` | Live data source config — injected into Five Truths assessments. |
| `~/.cpo/.version` | Version tracking — surfaces mismatches between skill and saved state. |
| `~/.cpo/contexts/` | Named contexts for multi-company use (`--context [name]`). |

---

## Know your stack

Run `/cpo --stack` to see what's installed alongside `/cpo`. It detects:
- `gstack` skills: `/plan-eng-review`, `/plan-ceo-review`, `/review`, `/ship`, `/qa`
- `pm-skills`: execution toolkit for PRDs, OKRs, roadmaps, retros
- Any other complementary skills

If a skill is missing, `--stack` surfaces the gap and suggests next steps. This is how you know your coverage is complete.

---

## Works with gstack

`/cpo` covers strategy and decision. [gstack](https://github.com/garrytan/gstack) covers implementation and QA. They hand off cleanly:

| After `/cpo`... | → Next step |
|---|---|
| Decision made → need implementation plan | `/plan-eng-review` |
| Decision made → need product / brand review | `/plan-ceo-review` |
| `eng-brief` written → need code review | `/review` |
| `postmortem` or `red-team` done → need QA plan | `/qa` |
| Decision shipped → need retrospective | `/retro` |

For execution artifacts — PRDs, OKRs, sprint plans, user stories, release notes → [pm-skills](https://github.com/phuryn/pm-skills).

---

## Works in Cursor

`CURSOR.md` is the full Cursor-compatible version of `/cpo`:
- Same bash blocks, same `~/.cpo/` state
- `cat`-based mode loading instead of `Read` tool
- Plain question format instead of `AskUserQuestion`
- Shared decision journal with Claude Code — decisions logged in one place, visible in both

Activate per-conversation: `@CURSOR.md`
Always-on: copy contents to `.cursorrules`

Command format: `cpo ?`, `cpo --help`, `cpo should we do this` (no `/` prefix in Cursor)

---

## Structure

```
cpo/
├── SKILL.md                    # Claude Code skill (~500 lines)
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
    └── modes/                  # Full mode templates (20 modes)
        ├── ceo.md
        ├── boardroom.md
        ├── investor-roundtable.md
        └── ...
```

---

## Version

Current: **v1.0.0** — [view changelog](CHANGELOG.md) for full release history.

---

## License

MIT. Free forever. Go make better product decisions.
