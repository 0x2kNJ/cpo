# `--roadmap` Flag

**Trigger:** `/cpo --roadmap` with a list of bets, initiatives, or ideas — inline or from decision journal.

**What it does:** Comparative prioritization across N competing bets. Goes beyond `sequence` mode by running compressed Five Truths per bet, scoring learning velocity, mapping dependencies, checking capacity, and detecting bias patterns from the decision journal. Outputs a ranked stack with an ASCII timeline and kill criteria per bet.

**Input collection:**

If bets are listed inline: use them. If not, pull from decision journal entries tagged `outcome: pending`. If neither: ask once: *"List the 2–6 bets you're weighing. One line each."* Minimum 2, maximum 8. If >8, force the user to cut: *"You have [N] bets. A roadmap with >8 items is a wish list, not a strategy. Cut to your top 8, or tell me which to drop."*

**Strategy alignment check (runs before Step 1 when strategy context was loaded):**

If `STRATEGY_FILES_FOUND` fired in the session preamble, cross-reference the bet collection against the loaded strategy docs before running the per-bet assessment:

> *Strategy cross-reference — [N] bets vs. strategy docs:*
> *↑ Aligned: [bets directly supported or implied by strategy docs — cite file]*
> *↓ Conflicts: [bets that contradict stated strategy — cite file and the specific conflict]*
> *○ Silent: [bets the strategy docs don't address — may be exploratory or gaps in the docs]*

Omit any row with nothing to say. Output before Step 1.

**Reframe check:** If any ↓ Conflicts row changes what "good roadmap execution" means — e.g., strategy docs define success as developer adoption but the top bets are all built around TVL, or strategy names a sequencing dependency the bet list ignores — output one line and stop: *"Strategy conflict: [name] may change the prioritization criteria — correct it now, or say proceed."* Wait for response. One reframe check maximum per invocation.

If no strategy context was loaded (`NO_STRATEGY_FILES`): skip the alignment check and proceed directly to Step 1. Note inline if bets make strategy assumptions that aren't documented.

---

**Step 1 — Compressed Five Truths per bet:**

For each bet, produce a 5-line assessment — one sentence per Truth. Use context, integrations, and decision journal. Tag each sentence: *fact / assumption / inference*.

```
Bet: [name]
  Economic: [one sentence — revenue impact, cost, unit economics]
  User: [one sentence — user pain solved, retention effect, JTBD fit]
  Market: [one sentence — competitive dynamics, timing, window]
  Execution: [one sentence — team capability, technical risk, timeline]
  Strategic: [one sentence — alignment with thesis, compounding value, moat contribution]
```

**Step 2 — Scoring dimensions (per bet):**

**Stage-aware scoring.** Use the set appropriate to detected stage — don't burden a pre-PMF founder with a Series B spreadsheet exercise:

*Pre-PMF / seed (≤$1M ARR or pre-revenue):* 3 dimensions only:

| Dimension | Score 1–5 | What it measures |
|---|---|---|
| Learning velocity | 1=slow/expensive to learn, 5=fast/cheap | How quickly you discover if this bet is right or wrong |
| Reversibility | 1=one-way door, 5=easily unwound | Can you change course if the bet is wrong? |
| Dependencies | 1=blocks/blocked by many, 5=independent | How coupled is this to other bets or systems? |

*Post-PMF / growth / Series B+:* All 6 dimensions:

| Dimension | Score 1–5 | What it measures |
|---|---|---|
| Impact | 1=marginal, 5=transformative | Revenue/user/strategic upside at full execution |
| Confidence | 1=pure speculation, 5=proven | How much evidence supports the Five Truths assessment |
| Learning velocity | 1=slow/expensive to learn, 5=fast/cheap | How quickly you discover if this bet is right or wrong |
| Reversibility | 1=one-way door, 5=easily unwound | Can you change course if the bet is wrong? |
| Dependencies | 1=blocks/blocked by many, 5=independent | How coupled is this to other bets or systems? |
| Team fit | 1=requires hires/skills you lack, 5=existing team excels here | Can your current team execute this? |

**Step 3 — Sequencing heuristics (apply in order):**

1. **High learning + high reversibility first.** These are cheap experiments — do them before committing to expensive irreversible bets.
2. **Dependency-first.** If bet A unlocks bet B, A sequences before B regardless of B's impact score.
3. **One-way doors last.** Irreversible bets (reversibility ≤2) go to the back unless they have a closing window (market timing, competitive pressure).
4. **Capacity limit.** Given team size: ≤5 people = 1 bet in flight. 6–15 = 2 concurrent. 16–30 = 3 concurrent. >30 = ask the user. Never schedule more concurrent bets than the team can realistically execute. If the user tries: *"You have [N] people. Running [M] bets in parallel means none get full attention. Cut to [X] or tell me who runs each."*
5. **Regret minimization.** For the top 3 bets, surface: *"If you didn't do [bet] this quarter and a competitor did, how much would that hurt?"* — bets with high regret on inaction move up.

**Step 4 — Bias detection (if decision journal has 3+ entries):**

Check the journal for patterns:
- **Path bias:** Are you always choosing Bold paths and ignoring Balanced? Or always hedging?
- **Domain bias:** Are you over-indexing on revenue bets and under-indexing on retention? On technical bets vs GTM?
- **Recency bias:** Did a recent win or loss disproportionately shift your current bet selection?

If any pattern detected, surface it: *"Pattern: your last 4 decisions were all Bold-path revenue bets. This roadmap has the same skew. Intentional, or should we rebalance?"*

**Step 5 — Output format:**

```
ROADMAP — [company/product] — [date]

DO NOW (this quarter)
━━━━━━━━━━━━━━━━━━━
1. [Bet name] — [1-line rationale]
   Impact: ■■■■□  Confidence: ■■■□□  Learning: ■■■■■
   Kill if: [specific condition]

2. [Bet name] — [1-line rationale]
   Impact: ■■■□□  Confidence: ■■■■□  Learning: ■■■■□
   Kill if: [specific condition]

DO NEXT (next quarter, contingent on learnings)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
3. [Bet name] — [1-line rationale] — unlocked by: [bet #N]
   Kill if: [specific condition]

DEFER (real but not now)
━━━━━━━━━━━━━━━━━━━━━━━
4. [Bet name] — [why defer: dependency / low confidence / capacity]

KILL (considered and rejected)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
5. [Bet name] — [why kill: failed Five Truths / misaligned / negative learning velocity]

DEPENDENCY MAP
━━━━━━━━━━━━━
[bet 1] ──unlocks──→ [bet 3]
[bet 2] ──independent
[bet 4] ──blocked-by──→ [bet 1 outcome]

BIAS CHECK: [pattern or "none detected"]
```

**After output:**

1. Save each bet to the decision journal with `outcome: pending` and `roadmap_position: [DO NOW / DO NEXT / DEFER / KILL]`.
2. If `--export` is also passed, write the full roadmap to `~/.cpo/exports/`.
3. Offer once: *"Run `/cpo --roadmap` again next quarter to re-stack based on what you learned."*

**Relationship to `sequence` mode:** `--roadmap` supersedes `sequence` for multi-bet comparative analysis. `sequence` mode handles single-initiative phasing (what order to build features within one bet). `--roadmap` handles which bets to pursue at all.
