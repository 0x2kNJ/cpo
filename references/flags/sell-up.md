# `--sell-up` Flag

**Trigger:** `/cpo --sell-up [audience]` where audience is one of: `ceo`, `board`, `eng-lead`, `cross-functional`, or a custom name.

**What it does:** Takes a strategic recommendation — from the current conversation, a recent `/cpo` analysis, or inline — and reframes it as a persuasive internal pitch tailored to a specific decision-maker. Produces the 1-minute version, the 5-minute version, objection pre-emption with responses, a concession strategy, and an explicit ask.

**The core principle:** PMs fail at internal pitches not because the analysis is wrong, but because they frame it from their own perspective. A CEO hears "user retention" differently than "revenue risk." A board member hears "tech debt" differently than "operational risk to our uptime SLA." Same facts, different frame. This mode reframes.

**Input collection:**

1. **Source material.** In order of preference:
   - Current conversation already contains a `/cpo` analysis → use it (most common path).
   - User provides a decision or recommendation inline → use it.
   - Neither → check last 3 decision journal entries. If a recent one exists: *"Use your latest analysis on [topic]? A) Yes · B) Different topic"*. If no journal: ask once for the recommendation in 2–3 sentences.

2. **Audience.** If not specified in the flag, ask once: *"Who are you pitching to? A) CEO / founder · B) Board · C) Engineering lead · D) Cross-functional peers · E) [Custom — tell me their role and what they care about]"*

**Strategy alignment check (runs before Step 1 when strategy context was loaded):**

If `STRATEGY_FILES_FOUND` fired in the session preamble, cross-reference the pitch recommendation against the loaded strategy docs before reframing:

> *Strategy cross-reference — pitch vs. strategy docs:*
> *↑ Supported by: [strategic commitments that reinforce this pitch narrative — cite file]*
> *↓ Contradicted by: [strategic commitments the pitch conflicts with or sidesteps — cite file]*
> *○ Not covered: [what the pitch claims that strategy docs are silent on]*

Omit any row with nothing to say.

If ↓ contradictions exist: **do not block** — the sell-up proceeds. Surface them as a note at the top of the output: *"Note: this pitch conflicts with [commitment in file]. Decide whether to address it directly or omit it from the pitch before presenting."* The user decides how to handle the conflict — this is a warning, not a gate.

If no strategy context was loaded: skip the alignment check entirely.

---

**Step 1 — Audience profile:**

Build a silent model of the decision-maker. Do not output this — it drives the reframing.

| Audience | Primary lens | Secondary lens | Fears | Responds to |
|---|---|---|---|---|
| CEO / founder | Strategic alignment, growth | Resource allocation, opportunity cost | Distraction from core thesis, over-commitment | Vision + numbers. "This gets us to [goal] faster because [mechanism]." |
| Board | Risk-adjusted returns, governance | Timeline to milestones, capital efficiency | Surprises, unbounded downside, lack of accountability | Frameworks + precedent. "Companies at our stage that did X saw Y." |
| Eng lead | Feasibility, technical debt | Team capacity, architecture fit | Scope creep, unmaintainable code, unrealistic timelines | Specificity + honesty about tradeoffs. "Here's what we're NOT building." |
| Cross-functional | Impact on their domain, coordination cost | Timeline, dependencies on their team | Unfunded mandates, being volunteered for work | Clarity on what you need from them and what you DON'T. |

**Custom audience:** If the user specifies a role not in this table (e.g., "regulator", "major customer", "legal", "finance"), do NOT infer generically. Before reframing, collect:

1. *"What does [audience] most care about in their day-to-day job?"*
2. *"What would make them say no immediately?"*
3. *"What evidence format do they trust most — data, precedent, or narrative?"*

Use the answers to build a custom row for this audience in the same structure as above, then proceed with reframing. If the user is in a hurry and says to skip the questions, infer from the role title and flag your assumptions at the top of the output using this format:

> **Audience assumptions (skipped intake):** Primary lens: [inferred focus]. Main fear: [inferred concern]. Evidence format: [data / narrative / precedent]. *Correct these before presenting if wrong.*

**Step 2 — Reframe the recommendation:**

Take the source material and restructure through the audience's primary lens. The same recommendation becomes different arguments:

- **For CEO:** *"[Recommendation] accelerates our path to [strategic goal] by [mechanism]. The alternative is [cost of inaction framed as strategic risk]."*
- **For board:** *"[Recommendation] addresses [risk/opportunity] with [bounded investment]. Comparable companies that [did/didn't] do this saw [outcome]. Kill criteria: [specific conditions for stopping]."*
- **For eng lead:** *"[Recommendation] requires [specific scope]. Here's what's NOT in scope: [explicit exclusions]. Technical approach: [1-2 sentences]. Timeline: [honest estimate with caveats]."*
- **For cross-functional:** *"[Recommendation] needs [specific thing] from your team over [timeframe]. It does NOT require [common fear]. The upside for your domain: [specific benefit]."*

**Step 3 — The 1-minute version:**

Exactly 4 sentences. Not a compressed version of the 5-minute version — a structurally different pitch optimized for an elevator or a Slack thread:

1. **The problem** — one sentence, framed through audience's lens.
2. **The recommendation** — one sentence, concrete and specific.
3. **The evidence** — one sentence, the single strongest data point or precedent.
4. **The ask** — one sentence, what you need from them and by when.

**Step 4 — The 5-minute version:**

Structured for a meeting or document:

```
HOOK (30 seconds)
[Problem framed as something the audience already cares about]

CONTEXT (60 seconds)
[What changed to make this urgent — market signal, data, user feedback]
[What happens if we DON'T act — cost of inaction through audience's lens]

RECOMMENDATION (60 seconds)
[What to do — specific, bounded, timelined]
[What we're NOT doing — explicit scope exclusion]
[What success looks like — measurable outcome, not vanity metric]

EVIDENCE (60 seconds)
[Strongest data point from Five Truths assessment]
[Second-strongest, from a different Truth]
[Precedent or analogy the audience would recognize]

THE ASK (30 seconds)
[Exact decision you need: approval / resources / support / alignment]
[Timeline for the decision]
[What the audience commits to if they say yes]
```

**Step 5 — Objection pre-emption:**

For the specified audience, generate the 3 most likely objections and pre-built responses. Structure:

```
OBJECTION 1: "[What they'll say]"
WHY THEY'LL SAY IT: [Underlying concern]
RESPONSE: "[Your answer — acknowledge the concern, redirect to evidence]"
IF THEY PUSH BACK: "[Escalation response — concede the point or offer a compromise]"
```

Objections must be specific to the audience type, not generic. A CEO won't object to "technical debt risk" — they'll object to "why now instead of after we close the enterprise deal?" An eng lead won't object to "strategic alignment" — they'll object to "this requires rewriting the auth layer."

**Step 6 — Concession strategy:**

What you're willing to give up to get the core ask approved. Structure:

```
MUST HAVE (non-negotiable for the recommendation to work):
- [Item 1]
- [Item 2]

WILLING TO TRADE (offer these as concessions if needed):
- [Item] — trade for: [what you'd accept instead]
- [Item] — trade for: [what you'd accept instead]

WALK-AWAY LINE (if they insist on removing these, the recommendation doesn't work):
- [Condition that would make you withdraw the recommendation]
```

**Step 7 — Output:**

Present in this order:
1. Audience profile (1 line: *"Pitching to [audience] — they care about [primary lens], fear [fear]."*)
2. The 1-minute version (labeled: "**Elevator pitch — use in Slack, hallway, or cold open:**")
3. The 5-minute version (labeled: "**Meeting version — use in 1:1, presentation, or written pitch:**")
4. Objection pre-emption (labeled: "**They'll push back on:**")
5. Concession strategy (labeled: "**If you need to negotiate:**")
6. The explicit ask (labeled: "**Your exact ask:**") — one sentence, unambiguous.

**Combinable with `--export`:** If `--export` is also passed, the sell-up output is export-ready — paste directly into a doc or Notion page. The 1-minute version works as a Slack message. The 5-minute version works as a meeting doc.

**Relationship to `upward-pitch` mode:** `--sell-up` supersedes `upward-pitch` for any internal persuasion task. `upward-pitch` remains as the internal routing mode name, but `--sell-up` is the user-facing interface with full audience-specific reframing, dual-version output, and negotiation strategy.
