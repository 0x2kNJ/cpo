# `--invalidate` Flag

**Trigger:** `/cpo --invalidate [topic or #id]` — mark a past decision as intentionally retired.

**Execute immediately** — no four-action flow.

## Steps

1. **Find the entry.** Locate the most recent non-invalidated journal entry matching the topic or `decision_id`. If `#id` syntax was given (e.g., `--invalidate #pricing`), use exact `decision_id` lookup. Otherwise, keyword-match on the `prompt` field across all non-invalidated entries.

2. **Display the entry.** Show: date, prompt summary, verdict, confidence, recommendation. One paragraph, no headers.

3. **Confirm intent.** Ask via AskUserQuestion (or plain text in Cursor):
   - Re-ground: *"Found: [topic] ([date]) — verdict was [verdict] ([confidence])."*
   - Options: A) Invalidate — give a one-line reason · B) Cancel

4. **Write the invalidation.** If confirmed:
   ```bash
   # Find the file and update it
   _FILE=$(grep -rl "^decision_id: [topic_or_id]$\|^prompt:.*[keyword]" ~/.cpo/decisions/*.yaml 2>/dev/null | head -1)
   # Then write the three fields to the file using sed or a targeted rewrite
   ```
   Set in the YAML:
   ```yaml
   status: invalidated
   invalidated_date: YYYY-MM-DD
   invalidated_reason: [user's one sentence]
   ```

5. **Confirm.** Output: *"[topic] marked invalidated — [reason]. Skipped in future context loads. Visible in `--history`."*

## Rules

- **Never auto-invalidate.** Invalidation is always user-initiated and always annotated with a reason.
- **Audit trail is permanent.** `--history` always shows invalidated entries — mark them `[invalidated YYYY-MM-DD]` inline. `--trail` marks them `[invalidated]`.
- **Future context loads skip invalidated entries.** The `DECISIONS_FOUND` preamble load skips any entry with `status: invalidated`.
- **Multiple entries.** If multiple entries match the topic, show the most recent first. If the user wants to invalidate an older one, they can use `#id` for exact lookup.
- **If no entry found:** *"No active journal entry found for '[topic]'. Check `--history` for the full list — it includes invalidated entries."*
- **Invalidated entries do not count toward `--patterns` analysis** — they're excluded from trend detection.

## Effect on Other Flags

| Flag | Behavior with invalidated entries |
|------|----------------------------------|
| `--history` | Always shows them, marked `[invalidated YYYY-MM-DD]` |
| `--trail` | Shows them, marked `[invalidated]` |
| `--patterns` | Excludes them from analysis |
| `--brief` | Skips them — they are not active decisions with kill criteria to monitor |
| `--since` | Skips them — they are not active baselines to diff against |
| `--drift` | Skips them — they are intentionally retired, not drifted |
| Context load | Skips them silently |
