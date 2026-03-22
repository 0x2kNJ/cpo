# Verification Subagent — Post-Output Spec Compliance Check

**Purpose:** Enforce self-check assertions mechanically instead of relying on honor-system self-checks. Runs as a post-output verification step for `--deep` and `--go` modes — the two modes where output density makes spec violations most likely.

## When It Fires

After the final output is delivered for `--deep` or `--go` invocations — after the Verdict and D-M menu are rendered, before the journal write. If `--dry-run` is also present, the subagent still fires (it validates output quality, not persistence).

**Does NOT fire for:** standard three-response flow (gates enforce compliance interactively), `--quick` (intentionally minimal), simulations (`boardroom`, `investor-roundtable`, `advisory-roundtable`), journal-reading flags (`--brief`, `--trail`, etc.).

## What It Checks

The subagent re-reads the output just produced and validates against these assertions. Each check is pass/fail with a specific fix action.

### Check 1 — Exactly 3 Paths
- **Pass:** Output contains exactly 3 lettered paths (A, B, C) with labels.
- **Fail:** Fewer or more than 3. **Fix:** Regenerate paths section before proceeding to journal write.

### Check 2 — Path Labels Are Structural, Not Risk-Posture
- **Pass:** Labels describe what the path *bets on* (scope, segment, channel, sequencing).
- **Fail:** Labels could be relabeled Bold/Balanced/Conservative. **Fix:** Rewrite labels as situational verb phrases.

### Check 3 — Kill Criteria Are Measurable
- **Pass:** Each kill criterion contains all three of: (1) a named metric (e.g., "weekly active users", "CAC", "MoM churn"), (2) a numeric or percentage threshold (e.g., ">20%", "<$50", "drops below 100"), and (3) a calendar timeframe or count (e.g., "in 60 days", "by Q2", "within 90 days post-launch"). Minimum 3 criteria for `--deep`/`--go`. Qualitative language like "stays reasonable," "grows significantly," or "improves" fails the threshold check — these are not numeric.
- **Fail:** Any criterion uses qualitative thresholds, omits a timeframe, or names no metric. **Fix:** Rewrite each deficient criterion to include all three components. If the user's intent is clear, infer the numeric threshold and timeframe from context rather than leaving a placeholder.

### Check 4 — Recommended Path Marker Present
- **Pass:** A `**We recommend [letter]:**` block appears before the path list (with a one-sentence rationale). Exactly one path carries `← recommended` inline. The letter in the block matches the path carrying `← recommended`.
- **Fail:** Block missing before the path list, zero or multiple paths carry `← recommended`, or the letter in the block doesn't match the marked path. **Fix:** Insert `**We recommend [letter]:** [rationale]` above the path list; add or deduplicate `← recommended` markers; verify letter consistency.

### Check 5 — Evidence Tags on Claims
- **Pass:** Claims about the user's situation, market, users, or competitive position carry *[fact / assumption / inference / judgment]* tags.
- **Fail:** Untagged claims found. **Fix:** Add evidence tags to untagged claims.

### Check 6 — D-M Menu Present
- **Pass:** D-M menu rendered after verdict at ALL confidence levels (High, Medium, Low). High confidence: D through M. Medium/Low confidence: D through L (M renders only at High). The elevation prompt (Medium/Low) is rendered alongside the D-M menu, not as a gate blocking it. If `--dry-run` is active, `N) Commit` may also appear after the last menu option — this is expected and does not affect the check. If `--quick` is active, D-M menu is intentionally omitted — Check 6 auto-passes for `--quick`.
- **Fail:** D-M menu entirely missing AND `--quick` is NOT active, OR D-M menu hidden behind an elevation gate. **Fix:** Append D-M menu (ensure it renders even when elevation prompt is present).

### Check 7 — Truth Fingerprint Present and Consistent
- **Pass:** (1) `Truth fingerprint:` line present with Dominant, Grounded, and Inferred classifications. (2) The Dominant Truth name appears in either the Grounded or Inferred list (it must be one of the five). (3) No Truth name appears in both Grounded and Inferred simultaneously.
- **Fail:** Missing fingerprint, or Dominant not in Grounded/Inferred, or a Truth appears in both. **Fix:** Append or rewrite the fingerprint line to satisfy all three sub-checks.

### Check 8 — Confidence Rating Present (`--deep` and `--go`)
- **Pass:** `Confidence:` line with High/Medium/Low and one-sentence key.
- **Fail:** Missing or malformed. **Fix:** Add confidence line.

## Execution Model

**This is NOT a separate Agent tool invocation.** It is a self-verification pass that runs in the same response context. The model reviews its own output against the checklist above before finalizing.

**Implementation:** After generating the verdict + D-M menu for `--deep` or `--go`:

1. **Silent scan:** Re-read the output produced in this response. Check each assertion (1-8).
2. **If all pass:** Proceed to journal write (or skip if `--dry-run`). No output to user.
3. **If any fail:** Fix the failing assertion(s) inline — rewrite the deficient section in the same response. Do NOT announce the fix or show the user that a verification ran. The user sees only the corrected output.

**Trace integration:** If execution trace is active AND `--dry-run` is NOT present, append a `verification` checkpoint. When `--dry-run` is active, the trace write is suppressed (dry runs are ephemeral — no trace record) but the inline output fixes still apply:

```json
{
  "step": "verification",
  "mode": "[deep|go]",
  "checks_run": 8,
  "checks_passed": [N],
  "checks_failed": [list of failed check names],
  "fixes_applied": [list of fixes]
}
```

## Why This Exists

The self-check assertions in SKILL.md are honor-system — the model is told to enforce them "at each step before proceeding," but under output pressure (especially in `--deep`'s 10-section format and `--go`'s single-response compression), violations slip through. Common failures: 2 paths instead of 3, missing `← recommended` marker, kill criteria without timeframes, Bold/Balanced/Conservative labels. The verification subagent makes enforcement mechanical rather than aspirational.
