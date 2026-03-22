# Flag Combination Rules

**When two or more flags are present in a single invocation, apply the stacking rules below.**

---

## Global Rules (apply to all combinations)

**One reframe check per invocation.** Multiple flags may each be eligible to fire a reframe check (--scan-strategy Path B, --since, --roadmap). Only one fires — the first one eligible in the enrichment phase. **Enrichment runs in this fixed order: --scan-strategy → --since → --roadmap.** If --scan-strategy fires a reframe check, --since and --roadmap do not also fire one; if --scan-strategy does not fire (no question-reframing conflict), --since is next in line, then --roadmap.

**`--go` suppresses all reframe checks.** When `--go` is present, no reframe gate fires, regardless of what other flags are present. Cross-reference outputs and strategy-anchored grounding still run; only the stop-and-wait behavior is suppressed.

**One reframe check location: before the Frame.** If any combination produces a reframe check, it fires after all pre-Frame enrichment is complete — not after each individual flag's enrichment. Output order: all enrichment (cross-references, temporal deltas) → one reframe check (if triggered) → Frame.

**Enrichment is additive, not competitive.** When two flags both enrich context (e.g., --scan-strategy adds strategy cross-reference, --since adds temporal cross-reference), both outputs appear. Strategy docs output first (timeless context), temporal delta second (what changed).

---

## Valid Combinations

### `--scan-strategy [question]` + `--since [date] [question]`

**Strongest pairing.** Combines strategy intent (what you committed to) with empirical record (what happened since).

**Stacking order:**
1. Run --scan-strategy bash block (strategy files)
2. Run --since bash block (decision journal)
3. Output strategy cross-reference (↑/↓/○ against the question) — cite strategy files
4. Output temporal cross-reference (↑ Reinforces / ↓ Challenges / ○ Still open) — cite decision journal
5. Apply one reframe check: scan both cross-references for question-reframing conflicts (success metric / primary user / time horizon). If found in either: fire one reframe check line, stop and wait.
6. Frame → four-action flow with strategy-anchored grounding options

---

### `--scan-strategy [question]` + `--roadmap`

**Strategy docs anchor the prioritization criteria before the per-bet assessment runs.**

**Stacking order:**
1. Run --scan-strategy bash block (strategy files)
2. Output strategy cross-reference against the bet collection (↑ Aligned / ↓ Conflicts / ○ Silent)
3. Apply one reframe check if a conflict changes what "good roadmap execution" means
4. Run --roadmap Step 1 onward (per-bet Five Truths, scoring, sequencing)

The --roadmap alignment check (normally runs from preamble STRATEGY_FILES_FOUND context) is **replaced** by the explicit --scan-strategy cross-reference when both flags are present — do not run both.

---

### `--scan-strategy [question]` + `--go`

**Cross-reference and strategy-anchored grounding still fire; all gates are suppressed.**

**Behavior:**
- Run --scan-strategy bash block, read files, output cross-reference (↑/↓/○) inline
- Do not fire reframe check — --go suppresses all stop-and-wait behavior
- Incorporate strategy context into the Frame silently
- Grounding options must still be strategy-anchored (self-verification rule still applies)
- Path labels must still be situational
- Deliver all four actions in one response per --go rules

---

### `--since [date] [question]` + `--go`

**Temporal delta still fires; reframe check is suppressed.**

- Run --since bash block, find PRIOR_ENTRY_FOUND
- Output temporal cross-reference inline (*↑ Reinforces / ↓ Challenges / ○ Still open*)
- Do not fire reframe check
- Incorporate temporal context into Frame silently
- Deliver all four actions in one response

---

### `--scan-strategy [question]` + `--sell-up [audience]`

**Strategy alignment check runs as a warning, not a gate. Sell-up proceeds regardless.**

- Run --scan-strategy bash block, read files
- Output strategy cross-reference (↑/↓/○) against the pitch recommendation
- If ↓ contradictions: surface as a note, not a gate — sell-up proceeds
- No reframe check for sell-up (decision frame is already set)
- Run --sell-up steps normally

---

### `--scan-strategy` (Path A, no question) + any other flag

Path A is a standalone maintenance utility — it outputs a posture refresh and ends. **It does not combine with question-analysis flags.** If Path A is invoked alongside another flag, execute Path A first (posture refresh output), then execute the other flag independently on its own terms. Do not attempt to merge outputs.

---

## `--dry-run` Combinations

`--dry-run` stacks with any decision-producing flag (`--go`, `--deep`, `--quick`, `--memo`, `--silent`, `--compare`, `--scan-strategy`, `--sell-up`, `--export`). The analysis runs identically — only the journal write is suppressed.

**`--dry-run --since [date] [question]`:** Valid. `--since` enriches the analysis with a temporal delta from the journal, then hands off to the full four-action flow. No journal write from the resulting decision. See dry-run.md for full combination spec.

**`--dry-run --since` (alone, no question):** Invalid — `--since` without a question just reads the journal with no decision to analyze. Drop `--dry-run`, run `--since` normally.

**Invalid with journal-reading flags:** `--dry-run` + `--brief` / `--trail` / `--history` / `--outcome` / `--patterns` / `--score` / `--verify` / `--assumptions` / `--replay` / `--kills` / `--drift` / `--graph` / `--status`. These flags read the journal, not write to it — `--dry-run` has nothing to suppress. Drop `--dry-run`, run the flag normally, note: *"--dry-run doesn't apply to [flag] (journal read, not write). Running [flag] normally."*

---

## Invalid Combinations

These combinations are not supported — proceed with the primary flag only, note the incompatibility inline.

| Combination | Why invalid |
|---|---|
| `--scan-strategy` + `--brief` | `--brief` is a proactive scan with no question to cross-reference against |
| `--scan-strategy` + `--trail` | `--trail` is a historical record output, not a decision question |
| `--scan-strategy` + `--history` | `--history` is a journal display, not a question |
| `--scan-strategy` + `--outcome` | `--outcome` closes a prior decision — no new question to cross-reference |
| `--since` + `--brief` | `--brief` has no question; temporal delta has nothing to anchor against |

When an invalid combination is detected: note inline — *"[flag1] + [flag2] aren't combinable — [flag2] has no question to cross-reference against. Running [flag2] only."* Then run the more specific/analytical flag.
