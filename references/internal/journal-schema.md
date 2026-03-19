# Decision Journal — Schema & Write Rules

The decision journal is `/cpo`'s persistent memory layer. Every major analysis output (modes that produce a verdict, recommendation, or kill criteria) is logged as a YAML entry. This is the foundation for `--since`, `--brief`, `--trail`, `--history`, and Decision Objects.

## When to Write

After every output from: `ceo`, `blue-ocean`, `red-team`, `premortem`, `sequence`, `gtm`, `investor-story`, `discovery`, `narrative`, `launch-os`, `postmortem`, `org-design`, `board-memo`, `board-story`, `upward-pitch`.

Do NOT write for: `advisory-roundtable`, `boardroom`, `investor-roundtable` (simulations write transcripts instead). Do NOT write for `eng-translate` or `eng-brief` (execution artifacts, not strategic decisions).

**Write the entry silently. Do not interrupt the conversation.** Do not announce the write unless the user has `--export` set or asks.

## YAML Schema

Before running: set `_CURRENT_MODE` to the actual mode used (e.g., `ceo`, `gtm`). Set `_PROMPT_SUMMARY` to a 3-5 word summary. Set `_DECISION_ID` to the `#name` tag if present, or empty if not.

```bash
mkdir -p ~/.cpo/decisions
_DATE=$(date +%Y-%m-%d)
_TS=$(date +%s | tail -c 5)
_MODE="${_CURRENT_MODE:-unknown}"
cat > ~/.cpo/decisions/${_DATE}-${_MODE}-${_TS}.yaml << EOF
schema_version: "1.5"
date: $_DATE
mode: $_MODE
decision_id: ${_DECISION_ID:-}
prompt: REPLACE_WITH_PROMPT_SUMMARY
revision: REPLACE_WITH_REVISION_NUMBER
verdict: REPLACE_WITH_Go_NoGo_Conditional_NA
confidence: REPLACE_WITH_High_Medium_Low
recommendation: REPLACE_WITH_ONE_LINE_RECOMMENDATION
kill_criteria:
  - REPLACE_WITH_CRITERION_1
  - REPLACE_WITH_CRITERION_2
three_paths:
  path_a: REPLACE_WITH_PATH_A_LABEL_AND_SUMMARY
  path_b: REPLACE_WITH_PATH_B_LABEL_AND_SUMMARY
  path_c: REPLACE_WITH_PATH_C_LABEL_AND_SUMMARY
truth_fingerprint: "Dominant: REPLACE_WITH_DOMINANT_TRUTH . Grounded: REPLACE_WITH_GROUNDED_TRUTHS . Inferred: REPLACE_WITH_INFERRED_TRUTHS"
delta_from_prior: REPLACE_WITH_WHAT_CHANGED_OR_NA
open_questions:
  - REPLACE_WITH_OPEN_QUESTION
consequences:
  - marker: REPLACE_WITH_CONSEQUENCE_DESCRIPTION
    check_date: REPLACE_WITH_DATE_TO_CHECK
    status: pending
outcome: pending
outcome_date: ""
outcome_notes: ""
status: active
invalidated_date: ""
invalidated_reason: ""
EOF
```

Replace every `REPLACE_WITH_*` value with the actual content from the output just produced. **Never write a `REPLACE_WITH_*` value to disk literally.** If a field cannot be determined, write `unknown`.

## Field Rules

**`decision_id`:** If the user tagged the prompt with `#name`, write that name (without the `#`). If no tag, leave empty.

**`revision`:** For new decisions (or untagged decisions), write `1`. For returning decisions (`DECISION_OBJECT_LOADED`), count the existing entries for this `decision_id` and write `N+1`.

**`delta_from_prior`:** For returning decisions, write a one-line summary of what changed. For new decisions, write `na`.

**`consequences`:** Auto-generated at decision time. Each consequence is a predicted downstream effect of this decision — something that should become visible within a specific timeframe. `marker` is a one-sentence prediction (e.g., "If we ship the paywall, free-tier signups should drop 20-40%"). `check_date` is when to look for this effect (typically 30-90 days out). `status` is `pending`, `confirmed`, or `disconfirmed`. `--status` and `--brief` scan for consequences past their `check_date` that are still `pending` — surfacing them as "consequence check due."

**Consistency check:** If a new journal entry contradicts a recent one (same `decision_id` or same topic, opposite verdict), flag it inline: *"Note: last time we looked at [topic] ([date]), verdict was X. Today's context shifts that to Y because Z."*

**Passive drift surface:** When writing a new journal entry, check whether the incoming entry's Dominant Truth differs from the most recent non-invalidated entry on the same `decision_id` or topic. If it does, append one non-blocking line: *"Note: Dominant Truth shifted from [prior] -> [current] vs. your last entry on [topic]. Intentional? Run `/cpo --drift` for a full drift check, or `--invalidate [topic]` to retire the prior entry."*

**Decision object queries:** When `--since #name`, `--outcome #name`, or `--history #name` is passed, filter journal entries by `decision_id` instead of keyword search. This is exact — no fuzzy matching needed.

## Spec Coherence Validator (runs at write time)

Before persisting a new journal entry, run a silent coherence check against all active (non-invalidated) entries:

**Check 1 — Contradiction detection:**
For the new entry's `recommendation:` and `kill_criteria:`, scan all active entries. Flag if:
- The new recommendation directly contradicts an active recommendation on a related topic (e.g., "go all-in on enterprise" vs. an active "stay focused on SMB")
- A new kill criterion conflicts with an active entry's recommendation (e.g., new kill criterion is "abandon if enterprise revenue < $50k in 90 days" while an active entry recommends deprioritizing enterprise)

**Check 2 — Assumption collision:**
Parse the `truth_fingerprint:` of the new entry. For each Grounded Truth, check if any active entry's Grounded Truth on the same dimension makes a contradictory claim. Example: new entry grounds Economic Truth on "CAC < $50" while active entry grounds Economic Truth on "CAC is acceptable at $120."

**Check 3 — Kill criteria overlap:**
If the new entry's kill criteria reference the same metric as an active entry's kill criteria but with a different threshold or timeframe, flag the discrepancy.

**Output behavior:**
- If any check fires: append one non-blocking line after the journal write confirmation (internal, not user-facing in standard mode): *"⚠ Coherence: [new entry] [contradicts/tensions with] [active entry decision_id or date] — [one-sentence description]. Run `/cpo --drift` to investigate."*
- If `--verbose` or `--deep`: surface the full coherence findings inline after the Verdict.
- Never block the journal write — coherence issues are warnings, not errors.
- Cap at 3 coherence warnings per write — if more found, note: *"[N] additional coherence notes suppressed — run `/cpo --drift` for full view."*

**Bash helper (runs before the journal write):**
```bash
# Load active entries for coherence check
_ACTIVE_ENTRIES=$(grep -l "status: active" ~/.cpo/decisions/*.yaml 2>/dev/null | xargs -I{} cat {} 2>/dev/null)
```

The coherence validator uses the loaded active entries in prose analysis — it does not require additional bash beyond loading them.
