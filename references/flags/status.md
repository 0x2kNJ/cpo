# `--status` Flag

**Trigger:** `/cpo --status` with no other prompt.

**What it does:** Opinionated executive snapshot — under 100 words. Reads everything CPO knows (context, journal, strategy files, recent git activity) and force-ranks to ONE critical-path item. This is not a summary or a report — it's an opinion about what matters most right now.

**Difference from `--brief`:** `--brief` is a weekly backward-looking intelligence cadence (what happened, what's approaching kill criteria). `--status` is a right-now forward-looking read (what is THE thing blocking velocity).

**Data collection (run silently):**

```bash
# 1. Recent git activity (what's been built)
git log --oneline -15 2>/dev/null || echo "NO_GIT"

# 2. Open branches (what's in flight)
git branch --no-merged main 2>/dev/null | head -5 || echo "NO_BRANCHES"

# 3. Decision journal — active decisions
_DJ=~/.cpo/decisions
if [ -d "$_DJ" ] && ls "$_DJ"/*.yaml 2>/dev/null | head -1 | grep -q .; then
  echo "ACTIVE_DECISIONS:"
  grep -l "status: active" "$_DJ"/*.yaml 2>/dev/null | while read -r _f; do
    echo "---"
    cat "$_f"
  done
  echo "DECISION_COUNT:"
  grep -cl "status: active" "$_DJ"/*.yaml 2>/dev/null | wc -l
  echo "APPROACHING_STOP:"
  # Decisions older than 30 days with outcome: pending
  find "$_DJ" -name "*.yaml" -mtime +30 2>/dev/null | while read -r _f; do
    grep -q "outcome: pending" "$_f" 2>/dev/null && echo "$_f"
  done
else
  echo "NO_DECISIONS"
fi

# 4. Strategy files (what's the posture)
ls -t .claude/strategy/*.md 2>/dev/null | head -3
```

**After collecting data, produce this output — nothing more:**

```
**Status:** [one sentence — where the company/project stands right now, grounded in context + recent activity]

**Red line:** [THE one thing blocking velocity — force-rank, do not list multiple items. If two things are tied, name the dependency between them and pick the upstream one.]

**Action:** [what to do about the red line — specific, actionable, this week. Not "think about X" — "ship X" or "decide X" or "kill X".]

**Open decisions:** [N] active · [N] approaching stop conditions · [N] unresolved >30 days
```

**Rules:**
- Total output MUST be under 100 words (excluding the bold labels). Count before delivering.
- **Force-rank to ONE red line.** If you cannot pick one, the red line is: "No clear critical path — run `/cpo --roadmap` to force-rank competing priorities."
- The red line is forward-looking: what unblocks the most downstream work? Not what's most urgent, not what's most important — what creates the most velocity when resolved.
- **Action must be concrete.** "Decide whether to pursue enterprise" is concrete. "Think about enterprise strategy" is not. Prefer verbs: ship, decide, kill, test, commit, cut.
- Do not run Five Truths. Do not produce paths. Do not write a journal entry. This is a read, not an analysis.
- If strategy files exist, the red line should be consistent with the strategic posture. If it contradicts, flag: *"Note: this conflicts with [strategy file] — may need a `/cpo --scan-strategy` refresh."*

**If NO_CONTEXT and NO_DECISIONS and NO_GIT:**
> *"Nothing to read yet — no context, no decisions, no git history. Run `/cpo --save-context` to set up, then start making decisions."*

**Terminal prompt:** End with: *"Dig into the red line? Run `/cpo [red line topic]` to analyze it. Or `/cpo --brief` for the full weekly picture."*
