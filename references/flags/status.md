# `--status` Flag

**Trigger:** `/cpo --status` or `/cpo --status --verbose`

**What it does:** Opinionated executive snapshot — 120–150 words. Reads everything CPO knows (context, journal, strategy files, recent git activity) and force-ranks to ONE critical-path item. This is not a summary or a report — it's an opinion about what matters most right now, grounded in the actual decision history of THIS founder.

**Difference from `--brief`:** `--brief` is a weekly backward-looking intelligence cadence (what happened, what's approaching kill criteria). `--status` is a right-now forward-looking read (what is THE one thing blocking velocity).

---

## Data collection (run silently)

```bash
# 1. Recent git activity (what's been built and shipped)
git log --oneline -15 2>/dev/null || echo "NO_GIT"

# 2. Open branches (what's in flight)
git branch --no-merged main 2>/dev/null | head -5 || echo "NO_BRANCHES"

# 3. Decision journal — cap at 10 most recent active decisions
_DJ=~/.cpo/decisions
if [ -d "$_DJ" ] && ls "$_DJ"/*.yaml 2>/dev/null | head -1 | grep -q .; then
  _ACTIVE_COUNT=$(grep -l "status: active" "$_DJ"/*.yaml 2>/dev/null | wc -l | tr -d ' ')
  echo "ACTIVE_DECISION_COUNT: $_ACTIVE_COUNT"
  if [ "$_ACTIVE_COUNT" -gt 10 ]; then
    echo "DECISIONS_CAPPED: loading 10 most recent of $_ACTIVE_COUNT active"
  fi
  echo "ACTIVE_DECISIONS:"
  grep -l "status: active" "$_DJ"/*.yaml 2>/dev/null \
    | xargs ls -t 2>/dev/null \
    | head -10 \
    | while read -r _f; do echo "---"; cat "$_f"; done
  echo "APPROACHING_STOP:"
  find "$_DJ" -name "*.yaml" -mtime +30 2>/dev/null | while read -r _f; do
    grep -q "outcome: pending" "$_f" 2>/dev/null && echo "$_f"
  done
else
  echo "NO_DECISIONS"
fi

# 4. Strategy files (what's the posture)
ls -t .claude/strategy/*.md 2>/dev/null | head -3

# 5. Load last status for delta detection
if [ -f ~/.cpo/last-status.yaml ]; then
  echo "LAST_STATUS:"
  cat ~/.cpo/last-status.yaml
else
  echo "NO_PRIOR_STATUS"
fi
```

---

## Synthesis (run silently — never show to user unless `--verbose`)

**Thin-journal check (runs first):** Count active decisions.
- **0 decisions:** Skip synthesis. Go directly to NO_DECISIONS output.
- **1–2 decisions:** Flag thin journal. Run abbreviated synthesis: anchor red line on the single decision with lowest confidence or nearest kill criterion. Append to output: *"Limited journal — make more `/cpo` decisions to improve status accuracy."*
- **3+ decisions:** Run full synthesis below.

**Full synthesis — 7 steps:**

**Step 1 — Intent trajectory**
What is the founder trying to achieve across all decisions? Not each decision in isolation — the arc. Extract the theory of the case: what are they building, for whom, and by what mechanism?
- *If clear arc:* name it in one clause (e.g., "wallet-native automation for DeFi power users, via partner embed")
- *If no clear arc:* the red line is strategic clarity itself

**Step 2 — Confidence map**
For each active decision, read `confidence:` and `truth_fingerprint:`. Rank decisions by confidence (Low first). Low-confidence decisions with heavily inferred Truths are structural risks — not just uncertain, but potentially wrong in ways that cascade.
- *Output:* ordered list `[decision_id] → [confidence] → [inferred Truths]`
- *Tiebreaker:* when confidence is equal, pick the decision with more downstream dependents

**Step 3 — Kill criteria proximity**
For each active decision, read `kill_criteria:` and `date:`. Compute days elapsed since the decision date. If a kill criterion has an explicit timeframe (e.g., "60 days"), calculate remaining days. Flag any decision within 20% of a kill threshold.
- *Output:* `[decision_id] — [criterion] — [N days remaining]` or `NO_APPROACHING_CRITERIA`
- *If a criterion is already past its window:* that IS the red line — the decision is already in kill territory and hasn't been resolved

**Step 4 — Decision dependencies**
Scan `recommendation:`, `open_questions:`, and `three_paths:` across all entries. Identify which decisions are blocked by other decisions. The upstream blocker is the red line, not the downstream aspiration.
- *If no explicit dependencies found:* note this — decisions may be operating independently, which is itself a signal
- *Tiebreaker:* when two decisions both have dependents, pick the one whose resolution unblocks more paths

**Step 5 — Ship-to-decision alignment**
Cross-reference verdicts and `recommendation:` fields against the git log. For each recent verdict with a build-first or ship-first path chosen:
- Does the git log show corresponding activity in the same area?
- *Gap detected* (verdict = build X, no git commits for X in 14+ days): this gap is a red line candidate — the founder decided to ship something and hasn't. Name the specific verdict and the missing commits.
- *Alignment confirmed* (verdict = build X, git shows recent X commits): note as green signal

**Step 6 — Operating style**
Read `operating_bias:` from `~/.cpo/context.md` if present. If not present, infer from the last 5+ path choices: which path type did the founder choose most (build-first, research-first, relationship-first)?
- *Declared bias wins over inferred.* If `operating_bias` is set, use it.
- *Inferred from <5 entries:* flag as low confidence — `[inferred, low signal — only N path choices available]`
- This determines whether the Action is a build move, a research move, or a relationship move

**Step 7 — Open question recurrence**
Scan `open_questions:` across all entries. If the same question (or semantically equivalent question) appears in 2+ entries, it is a chronic unresolved blocker — likely the real red line even if confidence on individual decisions looks ok.
- *If recurring question found:* surface it as a red line candidate
- *Tiebreaker with other candidates:* recurring open questions lose to triggered kill criteria, win over general confidence gaps

**Force-rank synthesis:**
From Steps 1–7, produce one ranked list of red line candidates. Apply this priority order:
1. Kill criterion already past its window (Step 3 — triggered)
2. Kill criterion within 20% of its window (Step 3 — approaching)
3. Ship-to-decision gap (Step 5 — decided but not shipped)
4. Recurring open question (Step 7)
5. Lowest-confidence upstream decision (Step 2 + Step 4)

**If two candidates tie at the same priority level:** name the dependency between them and pick the upstream one. The red line is the unresolved dependency itself: *"Cannot rank [A] vs [B] because [dependency] is unresolved — resolving [dependency] unblocks both."* This is still grounded and still force-ranks to one thing.

**Delta check (runs last in synthesis):**
If `LAST_STATUS` was loaded, compare:
- Did the prior red line resolve (no longer in top candidates)? → note the shift
- Is the red line the same as last time? → note "unchanged"
- Did a NEW red line emerge that wasn't in prior candidates? → flag as a new development

---

## Output

**Standard (`--status`):**

```
**Status [YYYY-MM-DD]:** [one sentence — where things stand, grounded in the intent trajectory and recent git. Reference what's been built.]

**Red line:** [THE one thing blocking velocity — name the specific decision_id or kill criterion. Not generic — anchored to this founder's actual data. If kill criterion is approaching, include days remaining.]

**Action:** [what to do — specific verb, this week, calibrated to operating_bias. "Demo [what was built] to [specific target]" not "book discovery calls." Include a time horizon if a kill criterion is counting down.]

**At risk:** [decision_id] — [one sentence: why it's at risk and what resolves it]

[If delta detected: *"Red line [unchanged / shifted from #prior-id] since [prior date]."*]
[If DECISIONS_CAPPED: *"[N] active decisions — loaded 10 most recent. Run `/cpo --brief` for full view."*]
```

**Verbose (`--status --verbose`):**
Produce the standard output first, then append:

```
---
**Synthesis trace:**
1. Intent trajectory: [one clause]
2. Confidence map: [ranked list]
3. Kill criteria proximity: [findings or NO_APPROACHING_CRITERIA]
4. Decision dependencies: [findings or INDEPENDENT]
5. Ship-to-decision alignment: [gaps found or ALL_ALIGNED]
6. Operating style: [declared/inferred bias + signal strength]
7. Open question recurrence: [recurring questions or NONE]

Red line candidates (ranked): [ordered list with rationale]
Selected: [winning candidate + tiebreaker used if applicable]
```

---

## Post-output: persist last status

After producing output, silently write:

```bash
mkdir -p ~/.cpo
cat > ~/.cpo/last-status.yaml << EOF
date: $(date +%Y-%m-%d)
red_line_decision_id: REPLACE_WITH_DECISION_ID_OR_NA
red_line_summary: REPLACE_WITH_ONE_LINE_RED_LINE
action: REPLACE_WITH_ACTION
at_risk: REPLACE_WITH_AT_RISK_DECISION
EOF
```

Replace every `REPLACE_WITH_*` with actual values from the output. Never write literal placeholder text.

---

## Rules

- **Word budget: 120–150 words** (excluding bold labels and the verbose trace). Specificity requires room — don't compress to vagueness to hit an arbitrary lower limit.
- **Red line must cite a specific `decision_id`, kill criterion, or ship gap.** A red line that could apply to any startup is wrong. If you cannot cite a specific grounded anchor, the red line is: *"Journal data is too sparse to force-rank — make 3+ `/cpo` decisions to unlock this."*
- **Force-rank to ONE red line.** If two candidates tie at the same priority level, name the dependency and pick the upstream one (see Force-rank synthesis above). The "run --roadmap" escape is a last resort — use it only when there are genuinely no decision entries to rank against.
- **Action must match `operating_bias`.** If the founder is build-first, the action is a build or demo move. If declared bias conflicts with the red line (e.g., build-first founder with a relationship-gated kill criterion), name the tension in one clause.
- Do not run Five Truths. Do not produce paths. Do not write a decision journal entry. This is a read, not an analysis.
- If strategy files exist, the red line must be consistent with strategic posture. If it contradicts: *"Note: conflicts with [strategy file] — run `/cpo --scan-strategy` to refresh."*

---

## Edge cases

**NO_CONTEXT and NO_DECISIONS and NO_GIT:**
> *"Nothing to read yet — no context, no decisions, no git history. Run `/cpo --save-context` to set up, then start making decisions."*

**1–2 active decisions (thin journal):**
> Use abbreviated synthesis. Anchor on confidence + kill criteria of available entries. Append: *"Limited journal — make more `/cpo` decisions to improve status accuracy."*

**DECISIONS_CAPPED (>10 active):**
> Load 10 most recent. Flag at bottom of output: *"[N] active decisions — loaded 10 most recent. Run `/cpo --brief` for full view."*

---

**Terminal prompt:** End with: *"Dig into the red line? Run `/cpo [red line topic]` to analyze it. Or `/cpo --brief` for the full weekly picture."*
