# Four Actions — Detailed Rules

This file contains the complete specification for the four-action flow: Frame, Assess, Paths, Verdict. SKILL.md contains the response templates and gates; this file contains the generation rules.

---

## AskUserQuestion Format

Every AskUserQuestion must:
1. **Re-ground:** What you're analyzing and what's missing (1 sentence).
2. **Recommend:** `RECOMMENDATION: [option] because [one-line reason]`
3. **Options:** Lettered — `A) ... B) ... C) ...`

**Exception — initial next-steps menu (Action 4):** Omit the RECOMMENDATION line on the first D-M menu after the Verdict. On re-surfaced menus (after a pick completes), include `RECOMMENDATION:`.

One question per call. Never batch. If AskUserQuestion unavailable: ask as numbered list and wait.

---

## Decision Objects (`#name`)

Tag any prompt with `#name` to create or revisit a named decision: `/cpo #pricing should we add a free tier?`

Names are lowercase alphanumeric with hyphens (`#pricing`, `#enterprise-gtm`, `#series-a`). The `#name` tag is stripped from the prompt before routing — it controls persistence, not mode selection.

- **New decision** (`DECISION_OBJECT_NEW`): Normal four-action flow. The `decision_id` is written to the journal entry.
- **Returning decision** (`DECISION_OBJECT_LOADED`): All prior entries for this ID are loaded. Action 1 opens with a delta frame instead of a fresh frame. The user's new prompt is interpreted as new context that may shift the Five Truths.
- **No tag**: Backward-compatible. Journal entries are written without a `decision_id`.

**Context cap:** When a returning decision has more than 5 prior entries, only the 5 most recent are loaded into context. The full history is available via `--history #name`.

Flags that accept `#name` for exact lookup: `--since #name`, `--outcome #name`, `--history #name`.

---

## Action 1 — Frame (Detailed Rules)

Infer the decision from the prompt + context + codebase. State it in one line, with inferences visible:

> *I'm reading this as: [decision]. Inferring [stage / model / lean] — correct me if wrong.*

Do not ask. Do not wait. State and proceed. The user can correct in one word in their next message — if they do, re-run from Action 2 with the corrected assumption. No new session needed.

**Returning decision (`DECISION_OBJECT_LOADED`):** Replace the standard frame with a delta frame. Read the most recent journal entry for this decision ID — extract its date, verdict, confidence, and kill criteria. Open with:

> *Returning to #[name] — last touched [date], verdict was [verdict] ([confidence]). [N] prior entries.*
> *Since then: [interpret the user's new prompt as new information that may shift the analysis].*

Then proceed to Action 2 (Assess) as a delta — re-run Five Truths with the new information layered on. Explicitly note which Truths shifted and which held: *"Economic Truth shifts from [old] to [new] because [user's input]. Strategic Truth holds."* If a kill criterion from the prior entry is now triggered, surface it immediately: *"Kill criterion hit: [criterion] — [evidence]."*

**For returning decisions, the delta frame replaces the grounding question.** No grounding AskUserQuestion fires in Response 1 — the delta frame IS the grounding. Response 2 (Paths) and Response 3 (Verdict) proceed normally from Action 2.

If invoked with no prompt at all (`/cpo` alone):
> What are we deciding?

If invoked with only a `#name` and no prompt (`/cpo #pricing`) and the decision object exists:
> *#pricing — last touched [date], verdict: [verdict] ([confidence]). What's changed?*

If `NO_CONTEXT` and `NO_DECISIONS` (first session ever), append after the first full response:
> *Tip: run `/cpo --save-context` once to save your company context — inferences become facts. Add `--go` to any prompt to skip menus and get a direct answer immediately.*

Show this once. Never again once context or decisions exist.

---

## Action 2 — Assess (Detailed Rules)

Silently run all Five Truths. Identify the Dominant Truth — the one most constraining or unlocking this decision. Surface the finding in 1-2 lines, thinking out loud:

> *The [Truth] is what this turns on: [finding in plain English].*

Do not list all five. Do not label this "Assessment." One finding, stated like an advisor thinking out loud.

**Evidence tag (conditional):** When the dominant Truth finding draws primarily from inference rather than data the user explicitly stated, append one inline bracket at the end of the finding line:

> *The [Truth] is what this turns on: [finding]. [Inferred — no [data type] provided; share actuals to ground this.]*

Omit the bracket entirely when the finding is grounded in data the user explicitly stated.

**Blind spots tracking:** During the silent Assess pass, note which Truths were primarily inferred (no explicit data stated by the user). Track these internally — they feed the Verdict blind spots line in Response 3. Do not output during Assess.

**Five Truths persistence:** After completing Assess (when a decision object exists via `#name`, or when `--deep` is passed), write the Five Truths snapshot to a scratch file:

```bash
mkdir -p ~/.cpo/.scratch
cat > ~/.cpo/.scratch/${_DECISION_ID:-unnamed}-truths.md << 'EOF'
# Five Truths — [decision summary]
Date: [YYYY-MM-DD]

**User Truth:** [finding]
**Strategic Truth:** [finding]
**Economic Truth:** [finding]
**Macro-Political Truth:** [finding]
**Execution Truth:** [finding]

**Dominant:** [Truth name] — [one sentence]
EOF
```

Write silently. After the Verdict in Response 3, append one line: *"Five Truths saved to `~/.cpo/.scratch/[slug]-truths.md` — reference anytime."*

**Context transparency (conditional):** If any key assumption driving the Dominant Truth was *inferred* (not stated by the user), add one line immediately after the Dominant Truth finding:

> *Driving assumption: [stated: X] + [inferred: Y — correct this? One sentence max.]*

Rules:
- Render only when an inference was made. If all context was explicit, suppress this line entirely.
- Show only the 2-3 data points that drove the Dominant Truth call — not all context.
- Distinguish `stated:` (user told you) from `inferred:` (you concluded from absence or signal).
- If corrected: re-run from Action 2 only. Do not re-run Action 1.

With `--deep`: assess all five Truths explicitly, one paragraph each.

If stage or customer segment is unknown and not inferable — embed it here as a one-line inference flag, not a question: *"Inferring pre-PMF — adjust the paths if wrong."*

**Conditional grounding:** Skip the grounding question and proceed directly to Action 2 if the user's prompt contains all three of:
1. A specific decision being weighed (not just a topic)
2. At least one explicit alternative or option they're already considering
3. A resource, time, or stage constraint

When skipping, include one line before Response 2: *"Your frame is clear — going straight to paths."*

When NOT skipping (underspecified prompt): proceed with the grounding question as normal.

### Grounding AskUserQuestion

End Response 1 with an AskUserQuestion to confirm the decision angle before generating paths.

Format:
- Re-ground: Name the decision being analyzed (e.g. "For [decision in 3-5 words] —")
- RECOMMENDATION: [option] because [one-line reason from the Dominant Truth]
- Options: 3 lettered choices naming the *specific frames or angles* for this decision (not Bold/Balanced/Conservative), **plus always a 4th correction option:** `D) Reframe — [one clause naming the element to correct]. Type your correction.`
- **When D is selected or the user types a free-text correction:** acknowledge in one line, re-run from Action 2. Do not re-ask the grounding question.

**Grounding option quality bar:** Options must represent meaningfully different *decision angles* — scope (which wedges to pursue), distribution strategy (direct vs. channel), or customer segment (broad vs. narrow). They must NOT represent different risk tolerances (aggressive vs. conservative).

**Self-check before delivering the grounding question:** Ask yourself: "If I replaced these option labels with Bold / Balanced / Conservative, would they still make sense?" If yes — even partially — the options are wrong. Rewrite them around a structural variable (scope, sequencing, distribution channel, customer segment, timing dependency) instead of around risk appetite.

> Wrong (risk tolerance in disguise): A) Go all-in on paywall now, B) Try a soft hybrid approach, C) Keep growing free users first
> Right (structural angles): A) Gate on the feature most tied to activation (test value threshold), B) Gate on usage volume not feature access (test price sensitivity), C) Gate on a separate Pro tier with no change to free (test segment willingness)

If AskUserQuestion unavailable, end Response 1 with:
```
Grounding — For [decision], which angle?
A) [frame 1]  B) [frame 2]  C) [frame 3]
Or correct the frame in a sentence — we'll re-run from Assess.
```

---

## Action 3 — Paths (Detailed Rules)

Delivered in Response 2, after the user confirms the grounding. Three Paths tailored to the confirmed frame. <=2 sentences each. Mark the recommended path with `<- recommended` — exactly one path gets this marker.

Open Response 2 with one framing sentence naming the tradeoff the three paths represent, anchored to the confirmed frame:

> *[Given [confirmed frame], the question is [core tradeoff in one clause].]*

**Path labels are situational — derived from the confirmed grounding frame, not pre-assigned.** Use verb phrases that name what each path commits to, optimizes for, or defers:
- Derive the label axis from the framing sentence opening Response 2
- When the grounding surfaced a strategy conflict: *"Resolve toward [X] / Resolve toward [Y] / Defer [conflict]"*
- When the grounding surfaced a sequencing or scope decision: *"Concentrate on [focus] / Sequence by [dependency] / Time-box [decision]"*
- When no strategy context is present: derive from the structural variable identified in grounding (segment, channel, timing, resource commitment)
- **Do not use Bold/Balanced/Conservative as path labels.**

**Self-check before outputting paths:** Ask: "Does each label tell the user what this path bets on, or just how risky it is?" If risk posture only -> rewrite.

**Label-to-framing-sentence dependency:** Labels are derived from the framing sentence that opens Response 2. That sentence names the core tradeoff — the labels are positions along it.

**Worked examples — one per label archetype:**

*Conflict tension (strategy docs in conflict):*
> Frame: "Given you're committed to developer-first, the question is whether to subordinate that to institutional adoption pressure or not."
> A) **Resolve toward developer-first** (subordinate institutional revenue, protect positioning) . B) **Resolve toward institutional** (subordinate developer adoption, run 6-month enterprise pilot) . C) **Defer the conflict** (separate BD pipeline from product motion)

*Alignment tension (sequencing / scope / timing):*
> Frame: "Given you've achieved SMB PMF, the question is which enterprise motion to sequence now vs. later."
> A) **Sequence by capability** (complete auth layer first, enterprise in Q3) . B) **Sequence by signal** (enterprise pilot now, retrofit auth on first contract) . C) **Time-box the decision** (90 days SMB-only, re-evaluate then)

*No strategy context (structural variable from grounding):*
> Frame: "Given you're targeting developer tools, the question is which channel gets you to the first 100 paying users."
> A) **Community-led** (open source + developer relations — compounding but slow) . B) **Product-led** (freemium -> conversion — faster but requires activation investment) . C) **Partner-led** (integrate into existing toolchains — fastest distribution, dependency risk)

### Path Selection

End Response 2 with an AskUserQuestion for path selection:
- Re-ground: One short sentence confirming which decision this is (e.g. "For [decision in 3-5 words] — which path?").
- RECOMMENDATION: [letter of recommended path] because [one-line reason from the Dominant Truth].
- Options: A) [path label in <=1 line] . B) [path label in <=1 line] . C) [path label in <=1 line]

After the AskUserQuestion overlay, append a plain-text block (never inside the overlay):
```
Not ready to commit? Dig deeper first:
1) Stress test    — challenge all three paths
2) Deep analysis  — all paths across product, market, execution, and risk
3) Reality check  — [inferred audience] reacts to each path

Reply 1, 2, or 3 to dig deeper — or A, B, or C to commit now.
```

**Pre-path challenge rules:**
- Challenge options run against **all three paths**, not just the recommended one
- When a challenge completes, re-surface the path-selection AskUserQuestion (with challenge block) and continue
- **No hard cap on challenge rounds.** After 3+ rounds without a path commit, add one non-blocking nudge line: *"You've analyzed this from [N] angles — what's still unresolved?"* Then continue.
- After the user picks a path (A/B/C), proceed to Action 4 (Verdict) normally
- **`--go` and `--quick` suppress the challenge block entirely.**
- **D-M options are post-verdict only** — never render in the pre-path challenge block.

**3) Reality check — inference rule (pre-commitment):** Infer the intended audience from the product context. If unambiguous, name them and run the check. If ambiguous, ask one clarifying question. Output: 2-3 plain-language reactions per path from that audience's perspective.

---

## Action 4 — Verdict (Detailed Rules)

Delivered in Response 3, after the user selects a path (A, B, or C). Name the chosen path. One sentence why. Kill criteria. Confidence: High / Medium / Low.

### Confidence-Elevation Loop

Fires only when Verdict confidence is Medium or Low AND the verdict names a specific gap. Append one sentence: *"To reach High: [restate the named gap exactly]. Provide it and I'll re-run the paths with that assumption locked — or reply **'skip'** to proceed at current confidence."*

**When this block renders, suppress the entire D-M next-steps menu.** The D-M menu unlocks only after: (a) the user provides the missing input and confidence re-evaluates, OR (b) the user explicitly replies "skip" or equivalent.

Loop exits when:
- Confidence reaches High
- User's reply is clearly not providing the named gap — exit immediately, render the D-M menu
- User replies "skip" — exit immediately, render the D-M menu
- Loop has already run twice

After 2 re-runs without reaching High: *"Remaining uncertainty is structural — proceed with [current confidence level]."* Then render the D-M menu.

### Elevation Mini-Flow

After receiving elevation input, deliver **one consolidated response** (not the full three-response flow):

1. First line: *"Locking: [assumption name] = [value provided]."*
2. Present the updated three Paths with the locked assumption reflected.
3. Deliver the Verdict line — using the user's **previously selected path letter** as the starting point, unless the locked assumption changes the top-ranked path OR upgrades confidence by one full grade. If the recommendation shifts, lead with: *"With this data, I'm updating the recommendation to [new path]:"*
4. **Do not re-ask the path-selection AskUserQuestion.**
5. Write a journal entry with `revision: N+1` and `delta_from_prior` capturing the elevation input.
6. Render the D-M next-steps menu.

### Kill Criteria Quality Bar

Each kill criterion must satisfy all three requirements:
1. **Named metric** — a specific thing that can be measured (e.g., "paid conversion rate," "weekly active users," "MRR")
2. **Specific threshold** — a number or direction (e.g., "drops >25%," "below 2%," "less than $3k")
3. **Timeframe** — a deadline or window (e.g., "within 60 days of launch," "in the first 30 days post-launch")

> Wrong: "if users stop engaging"
> Right: "if weekly active users drop >20% month-over-month in the 60 days post-launch"

**Kill criteria gate:** The D-M next-steps menu MUST NOT render until the Verdict contains at least three kill criteria — each specific, measurable, and time-bound. If a Verdict cannot produce three, state why and ask: *"What would tell you this bet is failing? Give me one measurable signal and I'll use it."*

**Elevation gate (takes priority over kill criteria gate):** If confidence is Medium or Low, the elevation block fires first — the D-M menu is suppressed entirely until the user resolves the gap or skips.

### D-M Next-Steps Menu

Immediately follow Verdict with an AskUserQuestion offering next steps.
- Re-ground: *"Verdict confirmed — [one-clause summary]. Pick a next step, or type a few letters to stack more."*
- No RECOMMENDATION on initial menu.
- Options (three groups):
  - **Analyze further:** D) Stress test · E) Deep analysis · F) Reality check — [inferred audience] reacts to the chosen path
  - **Communicate upwards:** G) Sell-up · H) Board simulation · I) Investor simulation
  - **Move it forward:** J) Roadmap · K) Eng brief · L) Hand off · [M) New evidence] *(M renders only when confidence is High)*

**AskUserQuestion group header rendering:** Prepend the group name to the first option of each group using `[Group name] ->` prefix. Example: `[Analyze further] -> D) Stress test...`, `[Communicate upwards] -> G) Sell-up...`, `[Move it forward] -> J) Roadmap...`.

**Note on Reality check — two contexts:**
- **Pre-path (Response 2):** labeled **3) Reality check** — reacts to all three paths.
- **Post-verdict (Response 3 onward):** labeled **F) Reality check** — reacts to the **chosen path and verdict** only.

**Progressive disclosure:** On first decision (preamble returned `NO_DECISIONS`), show only D-G + "More ->" option. On subsequent decisions, show full D-M menu.

### D-M Pick Behaviors

**Next-steps re-surfacing:** After completing any D-M pick, re-offer remaining unused picks via AskUserQuestion.
- **Non-repeatable picks (D, E, F, G, J, K):** Remove after completion.
- **Repeatable picks (H, I, L, M):** Persist in the re-surface menu.
- `RECOMMENDATION:` names the single most valuable remaining pick. **Simulation-informed RECOMMENDATION:** after H or I completes, name what was challenged and map to the pick that addresses it.

**If user picks F:** Run Reality check anchored to the **chosen path and current Verdict** — not all three paths. Identify the most likely objection from the inferred audience, surface it as a sharp challenge in 2-3 sentences, name what would need to be true to override it.

**If user picks H:** Trigger `--boardroom` mode inline — run the full boardroom simulation anchored to the current decision's Verdict, chosen path, and kill criteria.

**If user picks I:** Trigger `--investor-roundtable` mode inline — run the full investor simulation anchored to the current decision.

**If user picks K:** Trigger `/cpo --mode eng-brief` inline — translate the current decision for engineering.

**If user picks L:** Run silent discovery, then present sub-menu:

```bash
# Discovery (silent, instant — runs on L pick)
which ship 2>/dev/null           # gstack: push branch, open PR
which qa 2>/dev/null             # gstack: systematic QA
which review 2>/dev/null         # gstack: staff engineer review
which plan-eng-review 2>/dev/null  # gstack: architecture lock
which plan-cpo-review 2>/dev/null  # gstack: CEO/founder rethink
```

```
Hand off — where does this decision go next?

Always available:
-> export       Export decision to markdown (/cpo --export)
-> board-memo   Full board memo (/cpo --mode board-memo)
-> journal      Save to decision journal (already running)

[Detected on your system:]
-> /ship        Push branch, open PR
-> /qa          Systematic QA pass
-> /review      Staff engineer review
-> /plan-eng-review  Architecture lock-in
-> /plan-cpo-review  CEO/founder rethink

Pick a handoff.
```

Before executing the handoff, emit this context block:
```
**CPO Decision Context**
Decision: [summary]
Chosen path: [letter — label]
Verdict: [one sentence]
Kill criteria:
  1. [metric + threshold + timeframe]
  2. [metric + threshold + timeframe]
  3. [metric + threshold + timeframe]
Confidence: [High/Medium/Low]
```

**If user picks M):** Extract the data point(s) shared. **Single data point** -> run elevation mini-flow. **Comprehensive new context** -> treat as a Decision Object revisit: re-run Assess as a delta, deliver updated Verdict, write journal entry with `revision: N+1`.

---

## Structural Rules

- Response 1 Line 1 always starts with `*I'm reading this as:` — **unless** `STRATEGY_FILES_FOUND` with a question-reframing tension.
- Response 1 Line 2 always starts with `*The` and names a Truth
- Driving assumption line renders only when an inference was made
- Response 1 always ends with AskUserQuestion for grounding (A/B/C + D to reframe)
- Grounding AskUserQuestion options name specific decision angles (not Bold/Balanced/Conservative)
- Response 2 opens with a framing sentence anchored to the confirmed frame
- Paths use `A) **[Label]**`, `B) **[Label]**`, `C) **[Label]**` format
- Exactly one path carries `<- recommended` marker
- Response 2 ends with path-selection AskUserQuestion + 1/2/3 challenge block (never D-M letters)
- Response 3 uses structured format: `**Verdict:**`, `**Kill criteria:**`, `**Confidence:**`, `**Blind spots:**` (conditional), `-> To reach` (conditional)
- Response 3 always ends with D-M menu
- **Universal terminal rule:** Every response must end with a user action prompt.
- No headers, no numbered sections, no preamble before Line 1 of Response 1
- With `--deep`: insert full 10-section output after paths in Response 2
- With `--go`: deliver all four actions in one response
- With `--quick`: deliver all four actions in one response, one kill criterion only

---

## Blind Spots Rules

- Render only when >=1 Truth was primarily inferred during Assess
- List at most 3 Truths — prioritize the most decision-critical gaps
- Format: `[Truth name — no [data type]; [challenges/reinforces] this verdict . get it via: [collection method]]` — one per line, prefixed with `*`
- Always end with: *"Sharing any of these shifts the analysis."*
- Suppress entirely if all Truths were grounded

---

## Ordering Rule

1. Frame -> Assess -> Grounding -> Paths -> Verdict — always in this order
2. Grounding (frame confirmation) happens before Paths
3. Calibration (stage + model) — inferred in Action 1, flagged inline
4. Correction — user corrects Frame -> re-run from Assess

---

## Detect User Role

Final decision-maker (founder/CEO) or influencer (PM, VP, IC building a case upward)? Signals for influencer: "my CPO," "my manager," "get buy-in," "convince," "pitch internally," "I need approval." All others -> decision-maker.

If role is influencer AND prompt contains a decision question — state in Action 1: *"I'm hearing two things — validating [X] and building the case for [person]. Running both: validation first, pitch framing after the verdict."*

**Simulation gate:** For boardroom and investor-roundtable only, Action 1 ends with one confirmation: *"Setting up [simulation type] — shall I proceed? A) Yes . B) Strategic analysis instead."* Exception: `--go` skips the gate.

**Session gate memory:** Gate fires at most once per simulation type per session.

**Multi-mode requests:** If the prompt names two distinct decisions, Frame both in Action 1, state which runs first, run all four actions for it. Offer the second after.

---

## Calibration Protocol

**Simulation modes (`boardroom`, `investor-roundtable`) are exempt.** They collect context inline via their own pre-session setup.

Context hierarchy — check before asking anything:
1. `CONTEXT_LOADED_FRESH` or `CONTEXT_LOADED_STALE`? -> Context known. Do not ask. Proceed.
2. `CONTEXT_LOADED_MINIMAL`? -> Use partial context, flag what's missing. Proceed.
3. Context established earlier in this conversation? -> All fields known. Proceed.
4. Stage + model inferable from prompt? -> Treat as known, flag inline. Proceed.
5. `--silent`? -> Infer everything, flag every inference. Proceed.
6. None of the above -> ask ONE question (stage + model combined). No more.

**Never ask for context that's already known, established this session, or inferable.** Calibration fires at most once per session.

---

## Universal Output Disciplines

Across all 20 modes: lead with insight . Three Paths before recommendation . kill criteria on every recommendation . evidence labels on all non-obvious claims . confidence level on final call (High / Medium / Low).

**Confidence calibration:**

| Level | Meaning |
|-------|---------|
| **High** | I would stake material decisions on this without additional data |
| **Medium** | Directionally right, but one named assumption could invert the recommendation |
| **Low** | The framing may be correct but the data is too thin to have conviction — treat as directional only |

The confidence level in the Verdict always names the specific assumption that would need to change to move to the next tier.

**Placement:** Append the confidence key immediately after the Confidence level in the Verdict line — on the very next line, inline in the response.
