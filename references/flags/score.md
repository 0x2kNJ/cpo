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
| **Prediction accuracy** | confirmed / (confirmed + disconfirmed) | How often your consequence predictions come true |
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

## Minimum data requirement

If fewer than 3 decisions have verified outcomes: *"Not enough verified decisions yet ([N]/3 minimum). Run `/cpo --verify` to close the loop on past predictions, then re-run `--score`."*

## Integration

`--score` is read-only — never writes to the journal. Only reads existing entries and computes.
