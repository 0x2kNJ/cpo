# `--since` Flag

**Trigger:** `/cpo --since [date or "last time"]` — typically combined with a topic, e.g., `/cpo --since last time should we pursue enterprise?`

**What it does:** Temporal delta mode. Finds the most recent prior journal entry on the same topic, surfaces the baseline, then leads the analysis with what has changed.

Before running: extract the topic keyword from the non-flag words in the user's prompt (e.g., `/cpo --since last time enterprise gtm` → `enterprise`). If no topic, leave empty to use the most recent entry.

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

Then:

- **`PRIOR_ENTRY_FOUND`:** Lead with: *"Last time we looked at [topic] ([date]), verdict was [verdict] — [one-line recommendation]. Here's what's changed..."* Then output a structured temporal cross-reference before the Frame:

> *Temporal cross-reference — [topic in 3–5 words]:*
> *↑ Reinforces: [what changed since last time that supports the prior verdict or current direction]*
> *↓ Challenges: [what changed that contradicts the prior verdict or complicates the current question]*
> *○ Still open: [what was an identified gap last time that remains unanswered]*

  Omit any row with nothing to say. Then run the normal four-action flow.

  **Reframe check:** After the temporal cross-reference, check the ↓ Challenges row. If it changes the **success metric**, **primary user**, or **time horizon** of the current question — output one line and stop: *"Temporal shift: [name] may change the question itself — correct it now before I lock in grounding options, or say proceed."* Wait for response. One reframe check maximum per invocation (do not also fire the base-flow reframe check if both paths are active).

- **`NO_PRIOR_ENTRY`:** Run normal analysis. Note inline: *"No prior baseline on [topic] — running fresh analysis."*

**`--since last time` without a topic:** Use the most recent journal entry regardless of topic.
