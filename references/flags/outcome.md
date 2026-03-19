# `--outcome` Flag

**Trigger:** `/cpo --outcome [topic]` — e.g., `/cpo --outcome enterprise gtm`

**What it does:** Closes the loop on a prior decision. Surfaces the original verdict and kill criteria, records what actually happened, and — after 3+ outcomes — detects patterns across Bold/Balanced/Conservative path performance.

Before running: extract topic keyword (same logic as `--since`).

```bash
_DJ=~/.cpo/decisions
_KEYWORD="REPLACE_WITH_TOPIC_KEYWORD_OR_EMPTY"
if [ -n "$_KEYWORD" ]; then
  _MATCH=$(ls -t "$_DJ"/*.yaml 2>/dev/null | xargs grep -li "$_KEYWORD" 2>/dev/null | head -1)
else
  _MATCH=$(ls -t "$_DJ"/*.yaml 2>/dev/null | head -1)
fi
if [ -n "$_MATCH" ]; then
  echo "PRIOR_ENTRY_FOUND:"; cat "$_MATCH"
else
  echo "NO_PRIOR_ENTRY"
fi
```

**`PRIOR_ENTRY_FOUND`:** Surface the prior entry, then prompt:

> *"You chose [path] on [topic] ([date]). Verdict: [verdict]. Kill criteria: [criterion 1] · [criterion 2]. What happened?"*

Surface the prior entry, ask the user what happened and what the outcome was. Then record the outcome:

```bash
# Outcome recording — replace OUTCOME_STATUS, OUTCOME_DATE_VALUE, OUTCOME_NOTES_VALUE with actual values
# OUTCOME_STATUS: one of validated | invalidated | partial
# OUTCOME_NOTES_VALUE: one-line summary of what happened
_OUTCOME_FILE="REPLACE_WITH_MATCHED_FILE_PATH"
_OUTCOME_STATUS="REPLACE_WITH_validated_OR_invalidated_OR_partial"
_OUTCOME_DATE_VALUE=$(date +%Y-%m-%d)
_OUTCOME_NOTES_VALUE="REPLACE_WITH_ONE_LINE_SUMMARY"
sed -i.bak \
  -e "s|^outcome: .*|outcome: ${_OUTCOME_STATUS}|" \
  -e "s|^outcome_date: \".*\"|outcome_date: \"${_OUTCOME_DATE_VALUE}\"|" \
  -e "s|^outcome_notes: \".*\"|outcome_notes: \"${_OUTCOME_NOTES_VALUE}\"|" \
  "$_OUTCOME_FILE" && rm -f "${_OUTCOME_FILE}.bak"
echo "Outcome recorded: $_OUTCOME_FILE"
```

Replace every `REPLACE_WITH_*` value with actual content before running. `_OUTCOME_FILE` = the matched file path from the prior bash block. `_OUTCOME_STATUS` = the outcome as stated by the user. `_OUTCOME_NOTES_VALUE` = a one-line summary of what happened.

After recording, run pattern detection if 3+ entries for this decision_id or topic have `outcome:` set to `validated` or `invalidated`. Surface: *"Pattern: [N] of [total] decisions in this area were [validated/invalidated]. [One-line insight about the pattern]."*

**`NO_PRIOR_ENTRY`:** *"No prior decisions on [topic] logged yet."*
