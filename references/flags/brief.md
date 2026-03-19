# `--brief` Mode

**Trigger:** `/cpo --brief` with no other prompt.

**What it does:** Reads the decision journal + context + recent kill criteria. Produces a proactive intelligence brief — no new analysis, no advice. Just what the model knows, surfaced without being asked.

**Brief structure:**

```bash
# Load all decisions for brief
ls -t ~/.cpo/decisions/*.yaml 2>/dev/null | xargs -I{} cat {} 2>/dev/null
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
> **Recent ships (last 14 days):**
> [Scan all loaded journal entries for `entry_type: ship_event` where `date:` is within 14 days of today. For each: "Mar 18 — v0.19.2.0 shipped (branch: feature/x) → [PR link]". If none in the last 14 days: omit this section entirely — do not write "none".]
>
> **Context:** [stage] — [doctrine] — context last updated [date].

**If NO_DECISIONS:** *"No decisions logged yet. Run a `/cpo` analysis and your decision journal will start building automatically."*

**`--brief --export` together:** Export the brief as a Markdown file using the same `--export` behavior (see SKILL.md `--export` section). Slug: `brief-[date]`.
