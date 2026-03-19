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

## `--invalidate-all`

Bulk variant — marks all active journal entries as invalidated in one operation.

**Trigger:** `/cpo --invalidate-all` (all entries) or `/cpo --invalidate-all #name` (scoped to one decision object) or `/cpo --invalidate-all --hard` (permanent file deletion).

**Steps:**
1. Count active (non-invalidated) entries. If `#name` provided, count only that `decision_id`.
2. Display: *"Found N active journal entries [for #name]. This will mark all of them as invalidated."*
3. If `--hard` passed: *"⚠️ `--hard` will permanently delete the YAML files. `--history` will no longer show them. This cannot be undone."*
4. Require explicit YES confirmation — accept optional reason before YES.
5. Execute: write `status: invalidated`, `invalidated_date: YYYY-MM-DD`, `invalidated_reason: [reason or "bulk invalidation"]` to each matching file. If `--hard`: delete the YAML files instead.
6. Confirm: *"Done — N entries invalidated [and removed from disk]. Context loads start clean."*

**Rules:**
- Always show count before confirming — never silent execution.
- Standard (no `--hard`): audit trail preserved, `--history` always shows them.
- `--hard`: permanent deletion. Cannot be recovered. Use only for a true clean-slate reset.
- `#name` filter: scopes to that `decision_id` only — other entries untouched.
- If no active entries: *"No active journal entries found. Nothing to invalidate."*

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
