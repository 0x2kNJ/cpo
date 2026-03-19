# `--patterns` Flag — Decision DNA Analysis

## Trigger

`/cpo --patterns` — analyze the decision journal to surface personal decision-making tendencies.

## Data Source

Load all journal entries from `~/.cpo/decisions/*.yaml`. If fewer than 3 entries exist, output: *"Not enough decisions logged yet — run `/cpo --patterns` again after logging at least 3 decisions via `--outcome`."*

*Note: 3 entries is the technical gate; meaningful patterns emerge at 8–10 entries. Flag this to the user if N < 8: "Early signal — [N] decisions logged. Patterns strengthen at 8–10."*

## Analysis Dimensions

### 1. Truth Weighting Bias

Parse the `truth_fingerprint:` field across all entries. Tally how often each Truth appears as Dominant vs. Grounded vs. Inferred.

Output: Which Truth dominates your verdicts most often. Flag if one Truth is consistently Inferred (blind spot pattern) or if a single Truth dominates >60% of verdicts (single-axis risk).

### 2. Path Preference

Parse the `path_chosen:` field from each journal entry. Tally A/B/C frequency.

**Important:** Path letters are NOT fixed risk levels. A is not always "aggressive" and C is not always "conservative" — labels are situational per decision. The meaningful signal is: how often does the founder choose the recommended path (marked `← recommended` in the original output) vs. diverge from it?

Output: Recommended-path adherence rate (% of times the founder chose the `← recommended` path). Flag reversals (path chosen ≠ recommended) and, for entries with `outcome:` set, whether reversals correlated with better or worse outcomes. If the journal doesn't record which path was recommended, fall back to raw A/B/C tallies with the caveat: *"Letter tallies only — no recommended-path tracking available in these entries."*

### 3. Kill Criteria Hit Rate

Cross-reference `kill_criteria:` with `outcome:` fields. Count: decisions where a kill criterion triggered and execution was paused or reversed (hit rate) vs. decisions where kill criteria passed without action (miss rate).

Output: Kill criteria hit rate as a percentage. Flag if hit rate is <20% (criteria may be set too conservatively) or >60% (criteria may be too aggressive or decisions are in high-risk domains).

### 4. Decision Reversal Rate

Count decisions reopened within 30 days of logging — identified by multiple journal entries with the same `decision_id:` within a 30-day window, or explicit `outcome: reopened` entries.

Output: Reversal rate as a percentage. Flag if >30% (may indicate insufficient grounding at decision time) or <5% (may indicate decisions aren't being revisited when they should be).

### 5. Confidence Calibration

Compare `confidence:` field values at decision time against `outcome:` results. High confidence should correlate with better outcomes; Low confidence with harder outcomes or reversals.

Output: Calibration score — whether your confidence levels predict outcome quality. Flag systematic overconfidence (High confidence, poor outcomes) or underconfidence (Low confidence, strong outcomes).

## Output Format

```
DECISION DNA — [N] decisions analyzed

Truth bias:       [Dominant Truth] leads [X]% of verdicts · [blind spot Truth] consistently inferred
Path preference:  [A/B/C]% · [X] reversals from recommended ([Y]% led to better outcomes)
Kill criteria:    Hit rate [X]% · [flag if notable]
Reversal rate:    [X]% reopened within 30 days · [flag if notable]
Confidence:       [calibrated/overconfident/underconfident] — [one-sentence finding]

Pattern summary: [2–3 sentence plain-language summary of what this means for how this person makes decisions]

Watch for: [one specific behavioral pattern to watch for in the next 3 decisions]
```

## Rules

- Never surface raw counts without context — always interpret what the number means
- If a dimension has insufficient data (<3 data points), skip it and note: *"[dimension]: insufficient data"*
- Do not make normative judgments — describe patterns, not character
- If `truth_fingerprint:` is missing from older journal entries (pre-v1.4), skip Truth bias dimension and note: *"Truth bias: requires journal entries from v1.4+ — run more decisions to populate"*
- Pattern summary is always plain language, no jargon
- "Watch for" is always one concrete, observable behavior — not a meta-instruction

## Rolling Window Analysis

When 10+ journal entries exist, compute patterns across two windows:
- **Recent window:** Last 10 decisions
- **Prior window:** Decisions 11-20 (or all remaining if fewer than 20 total)

For each analysis dimension, compare the two windows and surface drift:

```
PATTERN DRIFT — comparing recent 10 vs prior [N] decisions
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Truth bias shift:    [Prior dominant] → [Recent dominant] · [stable / shifting / volatile]
Path preference:     [Prior A/B/C%] → [Recent A/B/C%] · [stable / shifting]
Confidence trend:    [Prior avg] → [Recent avg] · [calibrating up / calibrating down / stable]
Reversal rate:       [Prior %] → [Recent %] · [improving / worsening / stable]
Kill criteria usage: [Prior avg per decision] → [Recent avg] · [tightening / loosening / stable]

[If any dimension shows >25% shift: "Notable drift in [dimension] — your decision-making style is evolving. This may be intentional growth or unconscious habit change."]
[If all stable: "Decision patterns stable across both windows — consistent operating style."]
```

**Rolling window rules:**
- **Sort by decision date, not filesystem time:** Sort entries by their `date:` field (from the YAML content), not by filesystem modification timestamp. Edits via `--outcome` change the file's mtime but don't change when the decision was made — sorting by mtime would contaminate the "recent" window with old decisions that were merely updated.
- Only compute when ≥10 entries exist (need enough for a meaningful "recent" window)
- If 10-19 entries: prior window is smaller — flag: *"Prior window has only [N] entries — drift signals are early."*
- Always show the rolling window analysis AFTER the standard pattern output, separated by a `---`
- The rolling window is behavioral (how you decide), not topical (what you decide about)
