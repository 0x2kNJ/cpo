# Flag: `--assumptions` — Assumption Tracker

**Purpose:** Surface ungrounded assumptions across active decisions, grouped by age. Every decision has inferred Truths — some were never validated. This flag prevents assumption rot by making invisible inferences visible on a timeline.

## Trigger

`/cpo --assumptions` or `/cpo --assumptions #decision-id`

## Behavior

**Step 1 — Scan active decisions for inferred Truths:**

```bash
# Load all active decisions
_ACTIVE=$(grep -rl "status: active" ~/.cpo/decisions/*.yaml 2>/dev/null | xargs ls -t 2>/dev/null)
echo "ASSUMPTIONS_SCAN:"
for _f in $_ACTIVE; do
  echo "---"
  cat "$_f"
done
```

**Step 2 — Extract assumptions (prose analysis):**

For each active decision, parse the `truth_fingerprint` field. Inferred Truths are ungrounded assumptions. Cross-reference with the decision date to compute age.

Categorize each assumption:
- **Fresh** (< 30 days) — recently made, not yet stale
- **Aging** (30–60 days) — should have been validated by now
- **Stale** (60–90 days) — actively risky if still driving decisions
- **Critical** (> 90 days) — this assumption has been unvalidated for a quarter

**Validation specificity rule:** The "Validate via:" line must be derived from the actual decision context — name the specific action, audience, or data source. Never use generic placeholders like "check analytics." If the Economic Truth was inferred for a pricing decision, write "run a pricing survey with 20 existing users" or "pull cohort LTV from Stripe." If the User Truth was inferred, write "5 interviews with [specific segment from the decision]."

**Step 3 — Output format:**

```
Assumption Tracker — [N] active decisions, [M] ungrounded assumptions

[CRITICAL — unvalidated > 90 days]
· [Truth name] in #[decision-id or slug] ([date]): [what was inferred]
  → Validate via: [specific method — e.g., "5 customer interviews," "check Mixpanel cohort data"]

[STALE — 60-90 days]
· [Truth name] in #[decision-id or slug] ([date]): [what was inferred]
  → Validate via: [method]

[AGING — 30-60 days]
· [Truth name] in #[decision-id or slug] ([date]): [what was inferred]
  → Validate via: [method]

[FRESH — < 30 days]
· [count] assumptions under 30 days — not yet due for validation.

[If any CRITICAL or STALE assumptions exist]:
*"[N] assumptions have been unvalidated for [max age] days. The longer an inference drives active decisions, the more expensive it is when it turns out to be wrong. Start with the oldest."*
```

If `#decision-id` is passed, show only assumptions for that decision with full context.

## Integration with `--brief`

When `--brief` runs and STALE or CRITICAL assumptions exist, add one line:

*"[N] assumptions aging past 60 days — run `/cpo --assumptions` to review."*

## No output if clean

*"All active assumptions are under 30 days old. No validation urgency."*
