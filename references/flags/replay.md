# Flag: `--replay #name` — Counterfactual Replay

**Purpose:** Re-run a past decision with new information you didn't have at the time. See what would have changed — different Dominant Truth, different paths, different verdict? This is retroactive structured analysis, impossible without a decision journal.

## Trigger

`/cpo --replay #decision-id` or `/cpo --replay #decision-id "new information here"`

## Behavior

**Step 1 — Load the original decision:**

Substitute the actual decision ID (from `_DECISION_ID` in prose context) into the bash before running:

```bash
# Replace DECISION_ID_VALUE with the actual ID extracted from the user's prompt
_DJ=~/.cpo/decisions
_ENTRIES=$(grep -rl "decision_id: DECISION_ID_VALUE" "$_DJ"/*.yaml 2>/dev/null | xargs ls -t 2>/dev/null)
_LATEST=$(echo "$_ENTRIES" | head -1)
if [ -n "$_LATEST" ]; then
  echo "REPLAY_LOADED:"
  cat "$_LATEST"
  # Also load Five Truths snapshot if it exists
  _TRUTHS=~/.cpo/.scratch/DECISION_ID_VALUE-truths.md
  [ -f "$_TRUTHS" ] && echo "TRUTHS_LOADED:" && cat "$_TRUTHS"
else
  echo "REPLAY_NOT_FOUND: no entries for decision_id DECISION_ID_VALUE"
fi
```

If not found: *"No decision found with ID #[name]. Run `/cpo --history` to see all decisions."*

**Step 2 — Prompt for new information (if not inline):**

If the user didn't provide new information in the command, ask:

*"Replaying #[name] from [date]. What do you know now that you didn't know then?"*

Wait for response.

**Step 3 — Single consolidated replay response:**

```
Counterfactual replay — #[name] ([original date])

**Original verdict:** [path chosen] — [one-line reason]
**Original confidence:** [level] — [key assumption]
**Original Dominant Truth:** [Truth name]

**New information:** [what the user provided]

---

**What shifts:**

[For each Truth affected by the new information:]
· [Truth name]: [original finding] → [updated finding with new data]

**Updated Dominant Truth:** [Truth name — same or changed]

**Updated paths:**
A) [original label] — [how this path looks with new information]
B) [original label] — [how this path looks with new information]
C) [original label] — [how this path looks with new information]

**Updated verdict:** [same or different path] — [one-line reason]
**Updated confidence:** [same or different level]

---

**Delta summary:**
· Verdict [changed/held]: [one sentence on why]
· [N] of 3 kill criteria would have [triggered/not triggered] with this data
· Consequence predictions: [which would have been more/less accurate]

**Lesson:** [What you would have seen earlier if you'd grounded [Truth name] at decision time. One sentence — the meta-learning for next time, not just what changed.]

*This replay is informational — no journal entry written. To act on this, start a new decision or run `/cpo #[name] [new context]`.*
```

## Rules

- **Read-only:** Replay never writes to the journal. It's a "what if" tool.
- **Five Truths snapshot:** If the original decision has a Five Truths snapshot in `~/.cpo/.scratch/`, use it for precise comparison. If not, reconstruct from the journal entry's `truth_fingerprint`.
- **Single response:** No interactive flow. One consolidated output comparing original vs. counterfactual.
- **Kill criteria check:** For each original kill criterion, evaluate whether the new information would have triggered it.

## No decision ID

If invoked without a `#name`: *"Which decision do you want to replay? Run `/cpo --history` to see recent decisions, or pass a decision ID: `/cpo --replay #pricing`."*
