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

# 3. Decision journal — ALL active decisions (full content, not just counts)
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

**After collecting data — BEFORE writing output, run this synthesis silently (never show to user):**

Read every active decision entry. For each, extract:
- `prompt:` — what question was being asked
- `verdict:` — what was decided
- `recommendation:` — the specific recommendation
- `confidence:` — how confident
- `kill_criteria:` — what would make this wrong
- `three_paths:` — what options were considered
- `truth_fingerprint:` — which Truths were grounded vs. inferred
- `delta_from_prior:` — what shifted in revisits
- `open_questions:` — what's still unresolved

From this, build a silent mental model:
1. **Intent trajectory** — what is the founder trying to achieve across decisions? Not each decision in isolation — the arc. What's the theory of the case?
2. **Where confidence is thin** — which decisions have Low/Medium confidence or heavily inferred Truths? Those are the real blockers, not the loudest ones.
3. **Kill criteria proximity** — are any kill criteria close to triggering based on git activity + context? If a decision said "kill if no partner signal in 60 days" and 45 days have passed, that's the red line.
4. **Decision dependencies** — which decisions block other decisions? The upstream blocker is the red line, not the downstream aspiration.
5. **Founder's operating style** — look at the paths they chose across decisions. If they consistently pick build-first paths over outreach-first paths, the Action must match that pattern. If they chose "demo → partner interest" over "cold outreach → discovery calls" in a prior decision, do not recommend cold outreach.
6. **Open questions** — what questions remain unresolved across decisions? The most repeated open question is a signal.

**Then produce this output — nothing more:**

```
**Status:** [one sentence — where the company/project stands right now, grounded in context + journal arc + recent git activity. Reference the actual decisions and what they reveal about trajectory.]

**Red line:** [THE one thing blocking velocity — force-rank, do not list multiple items. Ground this in the journal: which decision has the thinnest confidence, the nearest kill criterion, or the most downstream dependents? If two things are tied, name the dependency between them and pick the upstream one.]

**Action:** [what to do about the red line — specific, actionable, this week. Calibrated to the founder's demonstrated operating style from their path choices. Not "think about X" — "ship X" or "decide X" or "kill X".]

**Open decisions:** [N] active · [N] approaching stop conditions · [N] unresolved >30 days
```

**Rules:**
- Total output MUST be under 100 words (excluding the bold labels). Count before delivering.
- **Force-rank to ONE red line.** If you cannot pick one, the red line is: "No clear critical path — run `/cpo --roadmap` to force-rank competing priorities."
- The red line is forward-looking: what unblocks the most downstream work? Not what's most urgent, not what's most important — what creates the most velocity when resolved.
- **The red line must be grounded in journal data.** Reference the actual decision that's blocking: its confidence level, its kill criteria, its open questions. A red line that could apply to any startup is wrong — it must be specific to THIS founder's situation as revealed by their decision history.
- **Action must be concrete.** "Decide whether to pursue enterprise" is concrete. "Think about enterprise strategy" is not. Prefer verbs: ship, decide, kill, test, commit, cut.
- **Action must be calibrated to the founder's demonstrated operating style.** Read the journal's path choices and verdicts. If the founder consistently picks build-first paths, the Action leverages what's been built (e.g., "demo the PoC to [target]", "ship a working prototype that proves [X]"). If they pick research-first paths, the Action is a research move. Never default to generic startup playbook advice that contradicts the founder's own decision patterns.
- Do not run Five Truths. Do not produce paths. Do not write a journal entry. This is a read, not an analysis.
- If strategy files exist, the red line should be consistent with the strategic posture. If it contradicts, flag: *"Note: this conflicts with [strategy file] — may need a `/cpo --scan-strategy` refresh."*

**If NO_CONTEXT and NO_DECISIONS and NO_GIT:**
> *"Nothing to read yet — no context, no decisions, no git history. Run `/cpo --save-context` to set up, then start making decisions."*

**Terminal prompt:** End with: *"Dig into the red line? Run `/cpo [red line topic]` to analyze it. Or `/cpo --brief` for the full weekly picture."*
