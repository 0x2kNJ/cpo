# Flag: `--score` — Decision Quality Score

**Purpose:** Compute a personal batting average across all closed decisions. Track how well-calibrated your judgment is over time. Meta-cognition about your decision-making — impossible without structured history.

## Trigger

`/cpo --score` or `/cpo --score --since [date]`

## Behavior

**Step 1 — Load all decisions:**

```bash
_ALL_FILES=$(ls -t ~/.cpo/decisions/*.yaml 2>/dev/null)
echo "SCORE_SCAN:"
for _f in $_ALL_FILES; do echo "---"; cat "$_f"; done
```

All computation happens in prose analysis on the loaded data.

**Step 2 — Compute scores:**

| Metric | Calculation | What it measures |
|--------|-------------|-----------------|
| **Prediction accuracy** | confirmed / (confirmed + disconfirmed), weighted by `verified:` field | How often your consequence predictions come true |
| **Confidence calibration** | % of High-confidence decisions where all consequences confirmed vs. % of Low where ≥1 disconfirmed | Whether your confidence levels mean what they should |
| **Blind spot rate** | Disconfirmed consequences where the relevant Truth was Inferred / total disconfirmed | Whether your weak spots are where you think they are |
| **Truth accuracy** | Per-Truth accuracy when that Truth was Dominant | Which Truths you read well vs. poorly |

**Step 3 — Output format:**

```
Decision Quality Score — [N] decisions analyzed ([M] with verified outcomes)
[If --since: "Period: [date] to today"]

Prediction accuracy:  [X]% ([confirmed]/[total verified])
Confidence calibration:
  · High confidence → [X]% accurate (expected: >80%)
  · Medium confidence → [X]% accurate (expected: 50-80%)
  · Low confidence → [X]% accurate (expected: <50%)
Blind spot rate: [X]% of misses were in inferred Truths

Your strongest Truth: [Truth name] — [X]% accuracy when Dominant
Your weakest Truth: [Truth name] — [X]% accuracy when Dominant

[One-paragraph interpretation: what this means for how you should approach decisions going forward. Be specific — name the pattern, not just the number.]
```

## Verification weighting

When computing Prediction accuracy, Confidence calibration, and Truth accuracy, apply verification weighting:
- Entries with `verified: yes` (from `--deep` or `--go` — passed the 8-check verification subagent) count as **1.0 weight**.
- Entries with `verified: no` (`--quick`, standard flow, dry-run commits) count as **0.75 weight**.
- Entries with no `verified:` field (pre-v1.6 schema) count as **1.0 weight** — do not penalize legacy entries; treat as pre-verification era.

Display note in output: *"[N] entries verified (full weight), [M] unverified (0.75× weight), [P] pre-v1.6 (full weight)."*

## Minimum data requirement

If fewer than 3 decisions have verified outcomes: *"Not enough verified decisions yet ([N]/3 minimum). Run `/cpo --verify` to close the loop on past predictions, then re-run `--score`."*

## Step 4 — Write score profile

**Gate:** Only write the profile when `--since` is NOT present in the invocation. If `--since [date]` was passed, the analysis is scoped to a period — writing it as a permanent profile would corrupt the full-history baseline. If `--since` is present, skip Step 4 entirely and note: *"Score profile not updated — `--since` produces a partial-period view. Run `/cpo --score` (no date filter) to update your profile."*

After computing scores with ≥3 verified decisions (no `--since`), write a compact profile to `~/.cpo/score-profile.md`. This profile loads at session start and informs Assess, Blind spots, and Confidence for every future decision.

```bash
mkdir -p ~/.cpo
cat > ~/.cpo/score-profile.md << 'EOF'
# CPO Score Profile
Generated: REPLACE_DATE
Decisions analyzed: REPLACE_N
Verified outcomes: REPLACE_M

Weakest Truth: REPLACE_WEAK_TRUTH — REPLACE_WEAK_ACCURACY% accuracy when Dominant
Strongest Truth: REPLACE_STRONG_TRUTH — REPLACE_STRONG_ACCURACY% accuracy when Dominant
Confidence calibration: REPLACE_CALIBRATION
  (hot = High confidence decisions <70% accurate · calibrated = 70-90% · cold = >90%)
EOF
```

Replace all `REPLACE_*` values with the actual computed values. Never write placeholders to disk.

**Weak Truth threshold:** Weakest Truth = Truth with the lowest accuracy percentage when Dominant. If the lowest accuracy is < 70%, set it as `_WEAK_TRUTH` in the profile. If all Truths are >= 70%, do not set a weak Truth — write `Weakest Truth: none (all ≥70%)` in the profile and suppress preamble-handler weak Truth behaviors.

After writing: *"Score profile saved to `~/.cpo/score-profile.md` — CPO will use this to surface your [Weak Truth] blind spots more aggressively in future sessions."*

## Integration

`--score` reads existing entries and computes. It does not write to the decision journal — only to `score-profile.md`.
