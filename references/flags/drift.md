# `--drift` Flag

**Trigger:** `/cpo --drift` — on-demand logic drift detection across recent decisions.

**Execute immediately** — no four-action flow.

## What Is Logic Drift?

Logic drift is when a series of individually reasonable decisions moves you away from a prior strategic commitment — without ever explicitly deciding to change course. It's distinct from intentional pivots (which have `delta_from_prior` populated) and from re-evaluations (where L) New Evidence was used).

**Drift is unacknowledged directional change.** Intentional change is not drift.

## What Counts as Structural Drift

Detect ONLY these patterns — not semantic similarity in topic keywords:

1. **Truth fingerprint shift:** Dominant Truth shifted across ≥3 consecutive non-invalidated entries on the same topic or `decision_id`, AND `delta_from_prior` is `na` or empty in those entries. (If `delta_from_prior` is populated, the shift was acknowledged — not drift.)

2. **Verdict reversal without acknowledgment:** Verdict direction reversed on the same `decision_id` (Go → No-Go or vice versa), AND `delta_from_prior` is `na` or empty in the later entry.

3. **Kill criteria degradation:** Kill criteria went from all three fields (metric + threshold + timeframe) to vague/missing across the last 3 entries on the same topic. Signals deteriorating commitment discipline.

## What Does NOT Count as Drift

- Two different decisions in the same domain (different `decision_id`, different topic keywords)
- Any revision where `delta_from_prior` is populated and non-empty — that's acknowledged change
- Truth shift where the user explicitly provided new evidence (L) pick or elevation input in session)
- Invalidated entries — they're intentionally retired, not drifted

## Execution

```bash
# Load last 10 non-invalidated entries
ls -t ~/.cpo/decisions/*.yaml 2>/dev/null | head -20 | while read _f; do
  _STATUS=$(grep "^status:" "$_f" 2>/dev/null | awk '{print $2}')
  if [ "$_STATUS" != "invalidated" ]; then
    echo "---"
    cat "$_f"
  fi
done | head -200
```

Parse the output. Group entries by `decision_id` (exact) and by topic keyword (fuzzy — use `prompt` field). For each group with ≥3 entries, run the three structural checks above.

## Minimum Entries

Requires at least 3 non-invalidated journal entries. If fewer:
> *"Not enough decisions to detect drift yet. Make 3+ decisions to unlock drift detection."*

## Output Format

```
**Drift report — last [N] decisions**

[If structural contradictions found:]
Structural shifts detected:

· [Truth fingerprint shift example]
  Entries: [date-1], [date-2], [date-3]
  Dominant Truth: Economic → Execution → Execution (no delta_from_prior in last 2)
  → Intentional pivot or unacknowledged drift?
  Run `/cpo --invalidate [topic]` to retire the prior entry, or `/cpo [topic]` to explicitly revisit.

· [Verdict reversal example]
  Entry [#id]: Go (2026-01-10) → No-Go (2026-03-01) — no delta_from_prior on reversal.
  → Run `/cpo --outcome #[id]` to close the loop on what changed.

[If no structural contradictions:]
No drift detected across last [N] decisions.
Dominant Truth consistent: [name] across [N] entries.

---
Run `--invalidate [topic]` to retire a superseded decision, or `/cpo [topic]` to revisit it.
```

## Integration with Passive Drift Surface

The passive drift surface (fires automatically when writing a new journal entry) is a lightweight single-entry check. `--drift` is the comprehensive multi-entry scan. They complement each other:

- **Passive:** "This new entry's Truth fingerprint differs from your last one on this topic — heads up."
- **`--drift`:** "Across your last N decisions, here are structural contradictions worth resolving."
