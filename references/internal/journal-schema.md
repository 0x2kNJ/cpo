# Decision Journal — Schema & Write Rules

The decision journal is `/cpo`'s persistent memory layer. Every major analysis output (modes that produce a verdict, recommendation, or kill criteria) is logged as a YAML entry. This is the foundation for `--since`, `--brief`, `--trail`, `--history`, and Decision Objects.

## When to Write

After every output from: `ceo`, `blue-ocean`, `red-team`, `premortem`, `sequence`, `gtm`, `investor-story`, `discovery`, `narrative`, `launch-os`, `postmortem`, `org-design`, `board-memo`, `board-story`, `upward-pitch`.

Do NOT write for: `advisory-roundtable`, `boardroom`, `investor-roundtable` (simulations write transcripts instead). Do NOT write for `eng-translate` or `eng-brief` (execution artifacts, not strategic decisions).

**`--dry-run` gate:** If `--dry-run` is present, skip the entire journal write (YAML creation, coherence validator, revision supersession, and execution trace). The analysis runs identically — only persistence is suppressed. Exception: if `--deep` or `--go` is also active, the verification subagent still runs its inline output checks (it validates quality, not persistence), but its trace checkpoint is also suppressed. See `references/flags/dry-run.md`.

**Write the entry silently. Do not interrupt the conversation.** Do not announce the write unless the user has `--export` set or asks.

## YAML Schema

Before running: set `_CURRENT_MODE` to the actual mode used (e.g., `ceo`, `gtm`). Set `_PROMPT_SUMMARY` to a 3-5 word summary. Set `_DECISION_ID` to the `#name` tag if present, or empty if not.

```bash
mkdir -p ~/.cpo/decisions
_DATE=$(date +%Y-%m-%d)
_TS=$(date +%s | tail -c 5)
_MODE="${_CURRENT_MODE:-unknown}"
# For dry-run commits, _DRY_START is set at invocation time — use it as date if present
_ENTRY_DATE="${_DRY_START_DATE:-$_DATE}"
cat > ~/.cpo/decisions/${_DATE}-${_MODE}-${_TS}.yaml << EOF
schema_version: "1.6"
date: $_ENTRY_DATE
mode: $_MODE
entry_type: ${_ENTRY_TYPE:-decision}
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
path_chosen: REPLACE_WITH_A_B_OR_C
truth_fingerprint: "Dominant: REPLACE_WITH_DOMINANT_TRUTH . Grounded: REPLACE_WITH_GROUNDED_TRUTHS . Inferred: REPLACE_WITH_INFERRED_TRUTHS"
delta_from_prior: REPLACE_WITH_WHAT_CHANGED_OR_NA
shelf_life: ${_SHELF_LIFE:-}
open_questions:
  - REPLACE_WITH_OPEN_QUESTION
consequences:
  - marker: REPLACE_WITH_CONSEQUENCE_DESCRIPTION
    check_date: REPLACE_WITH_DATE_TO_CHECK
    status: pending
verified: REPLACE_WITH_yes_or_no
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

**`entry_type`:** Classifies the journal entry. Values: `decision` (default — standard analysis output), `ship_event` (logged when a decision results in a shipped artifact — version release, PR merge, launch). `--brief` scans for `entry_type: ship_event` to render the "Recent ships" section. If the mode output is a standard analysis, write `decision`. If the output records a ship/launch event (e.g., from L) Hand off → /ship), write `ship_event`. If omitted, scanners treat the entry as `decision`.

**`decision_id`:** If the user tagged the prompt with `#name`, write that name (without the `#`). If no tag, leave empty.

**`revision`:** For new decisions (or untagged decisions), write `1`. For returning decisions (`DECISION_OBJECT_LOADED`), count the existing entries for this `decision_id` and write `N+1`.

**Revision supersession:** When writing revision N+1 for a `decision_id`, mark ALL prior revisions of that `decision_id` as `status: superseded`. Do this immediately after the new entry is written:

```bash
# Supersede prior revisions (run after writing the new entry)
_PRIOR_FILES=$(grep -rl "decision_id: ${_DECISION_ID}" ~/.cpo/decisions/*.yaml 2>/dev/null | grep -v "${_DATE}-${_MODE}-${_TS}.yaml")
for _pf in $_PRIOR_FILES; do
  [ -n "$_pf" ] && sed -i '' 's/^status: active$/status: superseded/' "$_pf" 2>/dev/null
done
```

Superseded entries are excluded from `--verify`, `--assumptions`, `--brief`, `--score`, and coherence checks (all of which grep for `status: active`). They remain accessible via `--history` and `--replay` for full audit trail. The `sed` is safe here because it only targets the exact top-level `status: active` line — consequence `status:` fields are indented and won't match `^status:`.

**`path_chosen`:** The letter of the path the user selected (A, B, or C). This is the raw letter, not the situational label. Used by `--patterns` for path preference analysis. If the user didn't explicitly select a path (e.g., `--go` auto-selects the recommended path), write the recommended path letter.

**`delta_from_prior`:** For returning decisions, write a one-line summary of what changed. For new decisions, write `na`.

**`consequences`:** Auto-generated at decision time. Derive 2-3 consequences from the Verdict and kill criteria — each is a testable prediction about what should change if the recommended path succeeds. Each consequence must be falsifiable within the stated timeframe — if you can't tell whether it happened or not, it's not a valid consequence. Prefer measurable consequences over qualitative ones. `marker` is a one-sentence prediction (e.g., "If we ship the paywall, free-tier signups should drop 20-40%"). `check_date` is when to look for this effect. Derive from context: if the decision has kill criteria with explicit timeframes, align the consequence check_date to 50-75% of the shortest kill criterion timeframe (check before the kill deadline, not after). If no kill criteria timeframe exists, default to 60 days from the decision date. Format: YYYY-MM-DD. `status` is `pending`, `confirmed`, or `disconfirmed`. If no meaningful consequence can be inferred from the decision, write `consequences: []` — never write placeholder text. `--status` and `--brief` scan for consequences past their `check_date` that are still `pending` — surfacing them as "consequence check due."

**`verified`:** Whether the output passed the verification subagent's 8-check compliance pass. Write `yes` for outputs from `--deep` or `--go` (verification subagent fires). Write `no` for `--quick`, standard three-response flow, simulations, or entries committed via `N) Commit` from a `--dry-run` (dry-run commits never ran the interactive gate sequence). Scanners (`--score`, `--patterns`) can weight entries by this field: `verified: no` entries carry lower signal for calibration purposes. The field documents *how* the decision was produced, not whether it was good.

**`shelf_life`:** Optional. Number of days this decision is expected to remain valid (e.g., `180` for a 6-month strategic bet, `365` for an annual plan). If present, the Decision Decay Index uses this value instead of the default 30-day divisor: `(days_since_decision / shelf_life) × ...`. If absent or empty, the default 30-day divisor applies. Set this for long-horizon decisions (market positioning, annual strategy, platform bets) to prevent false decay alarms. Short-horizon decisions (sprint-level, launch timing) should omit this field. In the bash block, set `_SHELF_LIFE` to the number of days before running the write, or leave it unset to produce an empty value.

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
Parse the `truth_fingerprint:` of the new entry. For each Grounded Truth, check if any active entry's Grounded Truth on the same dimension makes a contradictory claim — but ONLY if the two entries share a related topic (overlapping keywords in `prompt:` or `recommendation:`) or the same `decision_id` lineage. Same-dimension grounded Truths across unrelated decisions are not contradictions — they're different scopes. Example of a real collision: new entry grounds Economic Truth on "CAC < $50" for SMB while an active entry on the same SMB segment grounds Economic Truth on "CAC is acceptable at $120." Example of NOT a collision: SMB entry grounds "CAC < $50" while enterprise entry grounds "CAC acceptable at $120" — different segments, not contradictory.

**Check 3 — Kill criteria overlap:**
If the new entry's kill criteria reference the same metric as an active entry's kill criteria but with a different threshold or timeframe, flag the discrepancy.

**Check 4 — Decision similarity:**
Scan active entries for structural similarity to the new entry — overlapping keywords in `prompt:` or `recommendation:`, same Dominant Truth, or same mode. If a similar past decision exists with at least one verified consequence (`confirmed` or `disconfirmed`), surface it. Cap at 1 match per write. Skip if the matched decision is the same `decision_id` (that's a revision, not a similarity). Output: *"Pattern match: this resembles #[decision-id or slug] ([date]) — that one [succeeded/failed] because [one-clause reason]. Run `/cpo --replay #[id]` to compare."*

**Truth-subagent conflict integration:** When `--deep` mode runs and truth-subagents surface a cross-Truth conflict (e.g., Economic Truth says "unit economics don't work" while Strategic Truth says "must enter now"), check whether the same conflict already appears as an active coherence contradiction from a prior decision (Check 1 or Check 2 of the coherence validator). If so, surface a single merged note instead of two separate warnings: *"⚠ Coherence + internal conflict: [topic] — [prior decision date] showed the same tension between [Truth A] and [Truth B]. This pattern is recurring — run `/cpo --drift` or `--replay #[id]` to investigate."* Do not emit both a coherence warning and a truth-subagent conflict for the same underlying tension.

**Output behavior:**
- If any check fires: append one non-blocking line after the Verdict, visible in ALL modes (standard, verbose, deep): *"⚠ Coherence: this [contradicts/tensions with] your active decision on [topic or decision_id] ([date]) — [one-sentence description]. Run `/cpo --drift` to investigate."*
- If `--verbose` or `--deep`: surface the full coherence findings inline after the Verdict.
- Never block the journal write — coherence issues are warnings, not errors.
- Cap at 3 coherence warnings per write — if more found, note: *"[N] additional coherence notes suppressed — run `/cpo --drift` for full view."*

**Bash helper (runs before the journal write):**
```bash
# Load active entries for coherence check (capped at 20 most recent)
_ACTIVE_FILES=$(grep -l "status: active" ~/.cpo/decisions/*.yaml 2>/dev/null | xargs ls -t 2>/dev/null | head -20)
_ACTIVE_ENTRIES=$(echo "$_ACTIVE_FILES" | while read -r _f; do [ -n "$_f" ] && cat "$_f"; echo "---"; done 2>/dev/null)
```

The coherence validator uses the loaded active entries in prose analysis — it does not require additional bash beyond loading them.
