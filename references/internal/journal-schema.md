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

**Consistency check:** If a new journal entry contradicts a recent one (same `decision_id` or same topic, opposite verdict), flag it inline: *"Note: last time we looked at [topic] ([date]), verdict was X. Today's context shifts that to Y because Z."*

**Passive drift surface:** When writing a new journal entry, check whether the incoming entry's Dominant Truth differs from the most recent non-invalidated entry on the same `decision_id` or topic. If it does, append one non-blocking line: *"Note: Dominant Truth shifted from [prior] -> [current] vs. your last entry on [topic]. Intentional? Run `/cpo --drift` for a full drift check, or `--invalidate [topic]` to retire the prior entry."*

**Decision object queries:** When `--since #name`, `--outcome #name`, or `--history #name` is passed, filter journal entries by `decision_id` instead of keyword search. This is exact — no fuzzy matching needed.
