# `--kills` Flag — Kill Criterion Countdown

**Trigger:** `/cpo --kills`

**What it does:** Cross-cutting view of ALL active kill criteria across all decisions. Shows days remaining, status (safe/approaching/triggered), and groups by urgency. The single place to see everything that could force a course change.

## Data Collection

```bash
_DJ=~/.cpo/decisions
if [ -d "$_DJ" ] && ls "$_DJ"/*.yaml 2>/dev/null | head -1 | grep -q .; then
  echo "KILLS_DATA:"
  grep -l "status: active" "$_DJ"/*.yaml 2>/dev/null | while read -r _f; do
    echo "---"
    cat "$_f"
  done
else
  echo "NO_DECISIONS"
fi
```

## Kill Criteria Extraction (prose analysis)

For each active decision:
1. Extract all `kill_criteria:` entries
2. Parse each criterion for: **metric** (what's being measured), **threshold** (the number/direction), **timeframe** (deadline or window)
3. Compute days elapsed since the decision's `date:` field
4. If the criterion has an explicit timeframe (e.g., "within 60 days"), compute days remaining: `timeframe_days - days_elapsed`
5. Classify status:
   - **TRIGGERED** — days remaining ≤ 0 (past the window)
   - **APPROACHING** — days remaining ≤ 20% of original timeframe
   - **ACTIVE** — days remaining > 20% of original timeframe
   - **NO_TIMEFRAME** — criterion has no parseable timeframe (flag for user attention)

## Output Format

```
KILL CRITERIA DASHBOARD — [N] criteria across [M] decisions
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

🔴 TRIGGERED (past window):
  #[decision_id] — [criterion summary] — [X] days overdue
  Action needed: resolve via `/cpo --outcome [topic]` or kill the bet

🟡 APPROACHING (<20% remaining):
  #[decision_id] — [criterion summary] — [X] days remaining of [Y]
  #[decision_id] — [criterion summary] — [X] days remaining of [Y]

🟢 ACTIVE:
  #[decision_id] — [criterion summary] — [X] days remaining of [Y]
  #[decision_id] — [criterion summary] — [X] days remaining of [Y]

⚪ NO TIMEFRAME (needs attention):
  #[decision_id] — [criterion text] — no parseable deadline
  Consider adding a timeframe: `/cpo #[id]` to revisit

Summary: [triggered] triggered · [approaching] approaching · [active] active · [no_tf] undated
```

## Rules

- If no active decisions: *"No active decisions with kill criteria. Log decisions via `/cpo` to start tracking."*
- If all criteria lack timeframes: *"All [N] kill criteria lack explicit timeframes. Consider revisiting decisions to add deadlines — kill criteria without timeframes are intentions, not commitments."*
- Sort within each group by urgency (fewest days remaining first)
- After output, prompt: *"Act on a triggered criterion? Run `/cpo --outcome [topic]` to close the loop. Or `/cpo --status` for the executive view."*
- If any criteria are TRIGGERED, append a priority note: *"⚠ [N] kill criteria past their window — these decisions need resolution before new commitments."*
