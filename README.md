# /cpo — Strategic Product Advisor

> The hard conversation you need before you commit.

**Why it exists:** You're about to make a $500K decision — a pivot, a raise, a hire, a launch — and you have no one to pressure-test it with at 11pm. `/cpo` is that person. It gives you CPO-grade strategic advice on demand: go/no-go decisions, risk assault, and live investor and board meeting simulations you can't get anywhere else.

---

## What it looks like

```
/cpo simulate my Series B board meeting. Lead VC is skeptical of our burn rate.
```

> *Setting up the room. Lead VC: skeptical. Financial VC: neutral. Independent Director: neutral.*
>
> **Lead VC:** "Before we get to the deck — I pulled your bank statement this morning. You told this board in January that burn efficiency would improve in Q2. It's March. Walk me through what changed, and why we should believe the Q3 forecast."

That's the boardroom simulation. The Lead VC goes off-agenda. You respond in real time. It ends with a debrief: what you committed to, where you lost the room, what to fix before the real meeting.

---

## What it does

| Type | Example |
|------|---------|
| **Go/no-go** | "Should we kill our enterprise motion?" → instant verdict with Three Paths and kill criteria |
| **Risk assault** | "What could go wrong with this launch?" → top failure vectors ranked by probability × impact |
| **Investor simulation** | Live multi-investor debate — 5 archetypes, distinct voices, verdict per investor |
| **Board simulation** | Live board meeting — archetypes push back, disagree, surface governance dynamics |
| **GTM design** | ICP → channel → growth loop → retention |
| **Upward pitch** | Building the case to convince your CPO/CEO/leadership |
| **20 total modes** | Covering the full product strategy surface area |

---

## Install

```bash
git clone https://github.com/YOUR_USERNAME/cpo ~/.claude/skills/cpo
```

Then invoke with `/cpo` in any Claude Code session.

---

## Usage

```bash
# Ask a direct question — gets an immediate verdict, no menu
/cpo should we launch in Europe before we fix retention

# Open-ended — shows options menu
/cpo we're thinking about building a paid API

# Skip the menu on anything
/cpo --go should we acquire this company for $8M

# Live investor simulation
/cpo --go simulate an investor meeting for our B2B SaaS

# Live board simulation with pre-loaded skepticism
/cpo simulate my Series B board meeting. Lead VC is skeptical of our burn rate.

# Full deep-dive with all Five Truths
/cpo --deep our freemium conversion is at 1.2% and we need to decide whether to kill it

# One paragraph, fast
/cpo --quick is freemium a good idea for our B2B tool

# Save your company context (loads automatically next session)
/cpo --save-context
```

---

## The simulations

The investor and board meeting simulations are the thing you can't get anywhere else.

**Investor roundtable** — a live 5-round structured debate between investor archetypes (Company Builder, Category Maker, Numbers Analyst, User Validator, Contrarian) who disagree with each other, push on your wedge, and give you a verdict per archetype. Add domain specialists for crypto, enterprise, consumer, deep tech, or AI.

**Board simulation** — the actual board meeting, not prep for it. The Lead VC goes off-agenda, the Financial VC challenges your burn assumptions, the Independent Director raises a governance question. You respond in real time. Ends with a post-meeting debrief: what you committed to, where you lost the room, what to fix before the next meeting.

---

## Context persistence

`/cpo` remembers your company across sessions:

```bash
/cpo --save-context   # saves stage, model, constraint, priorities
/cpo                  # automatically loads saved context next session
```

Context is stored at `~/.cpo/context.md` — edit directly to update. Staleness is checked automatically; you won't be re-asked unless your context is >90 days old or you signal a strategic shift.

---

## Flags

| Flag | Effect |
|------|--------|
| `--go` | Skip menus, route directly, get the answer |
| `--deep` | Full 10-section structured output |
| `--quick` | One paragraph, Dominant Truth only |
| `--memo` | Output as a printable decision memo |
| `--silent` | No calibration questions, proceed with stated assumptions |
| `--save-context` | Save company context for future sessions |
| `--context [name]` | Load a named context (for consultants / multi-company) |
| `--no-context` | Ignore saved context, reason from prompt only |
| `--focus [topic]` | Jump directly to targeted pressure on one topic |
| `--brief` | Proactive intelligence brief — what's changed, what's approaching kill criteria, what's unresolved |
| `--export` | Write output to `~/.cpo/exports/YYYY-MM-DD-[slug].md` — shareable with co-founders, boards, investors |
| `--trail` | One-page decision diary — all journal entries for the last 90 days |
| `--history [keyword]` | Full decision journal browser; optional keyword filter |

---

## What `/cpo` does NOT do

PRDs, OKRs, sprint plans, user stories, backlogs, retros. For execution artifacts → **[pm-skills](https://github.com/phuryn/pm-skills)** — the complementary execution toolkit.

---

## Who it's for

- **Solo founders** making irreversible decisions with incomplete information
- **PMs and Heads of Product** who need to build the case upward or laterally
- **Scaling teams** preparing for investor or board meetings
- **Anyone** who needs a smart, opinionated co-thinker before committing

---

## Structure

```
/cpo
├── SKILL.md                    # Main skill (~500 lines — the execution contract)
├── CHANGELOG.md                # Full version history
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

## Complementary skills

| Skill | When to use it |
|-------|---------------|
| `/plan-eng-review` | After `/cpo` → need an implementation plan |
| `/plan-ceo-review` | After `/cpo` → need a product / brand review |
| `/review` | After `eng-brief` → need code review |
| `/qa` | After `postmortem` or `red-team` → need a QA plan |
| [pm-skills](https://github.com/phuryn/pm-skills) | After strategy is set → execution artifacts |

---

## Version

Current: **v4.0.0** — see [CHANGELOG.md](CHANGELOG.md) for full history.
