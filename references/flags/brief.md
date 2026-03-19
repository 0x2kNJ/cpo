# `--brief` Mode

**Trigger:** `/cpo --brief` with no other prompt.

**What it does:** Reads the decision journal + context + recent kill criteria. Produces a proactive intelligence brief — no new analysis, no advice. Just what the model knows, surfaced without being asked.

**Brief structure:**

```bash
# Load all decisions for brief
grep -l "status: active" ~/.cpo/decisions/*.yaml 2>/dev/null | xargs ls -t 2>/dev/null | while read -r _f; do cat "$_f"; echo "---"; done 2>/dev/null
```

Then output:

> **Strategic Brief — [date]**
>
> **Approaching kill criteria:**
> [List any kill criteria from journal entries where the user has signaled the threshold is close, or where > 30 days have passed since the decision without a follow-up. If none: "No criteria approaching — last check was [date]."]
>
> **Unresolved decisions (>30 days without follow-up):**
> [List decisions with no subsequent entry on the same topic. Format: "Feb 12 — [topic], verdict was X. No follow-up logged."]
>
> **Pattern alerts:**
> [Scan all loaded journal entries. Run three checks — surface each that fires:]
>
> *Confidence calibration (fires when ≥5 entries have `outcome: validated` or `outcome: invalidated` recorded):* Of those entries where `confidence: High` was stated at decision time, what fraction ended `invalidated`? If >50%: *"Calibration gap: [N] of your High-confidence decisions were later invalidated ([X]%). Your confidence may be running hot — consider defaulting to Medium until the rate improves."*
>
> *Thrashing (fires when ≥3 entries share a topic keyword, regardless of outcome count):* *"You've reviewed [topic] [N] times. Verdict changed [X] times. The underlying question may not be the one you're asking."*
>
> *Stuck decision (fires when a `decision_id` has ≥3 revisions all showing `outcome: pending`):* *"[decision_id] has been revised [N] times without resolution. Consider forcing a time-bound commitment or explicitly killing it."*
>
> If none of the three checks fire: omit **Pattern alerts** section entirely — do not write "No patterns."
>
> **Consequence checks due:**
> [Scan all loaded journal entries for `consequences:` where `check_date` is today or in the past AND `status: pending`. For each: "[decision_id/date] predicted: '[marker]' — check was due [check_date]. Was this prediction correct? Run `/cpo --outcome [topic]` to record."]
>
> If no consequence checks are due: omit this section entirely.
>
> **Decision decay:**
> [For each active decision, compute a decay score: `(days_since_decision / divisor) × (number_of_inferred_truths / 5) × confidence_multiplier` where divisor is the decision's `shelf_life` field if present, otherwise 30. Confidence_multiplier is 1.5 for Low, 1.0 for Medium, 0.5 for High. Classify: **Fresh** (<1.0) · **Aging** (1.0–2.0) · **Stale** (2.0–4.0) · **Degraded** (>4.0). Only surface Stale and Degraded decisions.]
>
> [List decisions classified as Stale or Degraded, ranked by score:]
> "[decision_id/date] — [topic] — **[Stale/Degraded]** (aged [N] days, [M] inferred truths, confidence: [level]). Original assumptions may no longer hold."
>
> [If any decision is Degraded: "⚠ [decision_id] assumptions are significantly degraded — revisit with `/cpo #[id]` or kill with `--invalidate [id]`."]
>
> If no decisions are Stale or Degraded: omit this section entirely.
>
> **Recent ships (last 14 days):**
> [Scan all loaded journal entries for `entry_type: ship_event` where `date:` is within 14 days of today. For each: "Mar 18 — v0.19.2.0 shipped (branch: feature/x) → [PR link]". If none in the last 14 days: omit this section entirely — do not write "none".]
>
> **Context:** [stage] — [doctrine] — context last updated [date].

**If NO_DECISIONS:** *"No decisions logged yet. Run a `/cpo` analysis and your decision journal will start building automatically."*

**`--brief --export` together:** Export the brief as a Markdown file using the same `--export` behavior (see SKILL.md `--export` section). Slug: `brief-[date]`.
