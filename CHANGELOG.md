# CPO Skill — Changelog

All notable changes documented here. Follows [Keep a Changelog](https://keepachangelog.com/en/1.0.0/) conventions.

---

## [3.4.0] — 2026-03-22

**Complete rebuild.** CPO v3.4.0 reduces from 544 lines/32KB to 406 lines/17KB while adding structural guarantees the previous version lacked. Every `[FRAME]` and `[PATHS]` response now ends with a mandatory AskUserQuestion gate — not a text STOP marker, but a mechanical call the model cannot skip. The 30+ flags and 20 modes of v2.7.1 are replaced with 8 essential flags that each do exactly one thing.

### Added

- **Structural gates** — `[FRAME]` and `[PATHS]` each end with a mandatory `AskUserQuestion` call. Not a "⛔ STOP" suggestion — a hard mechanical gate.
- **Phase markers** — `[FRAME]` / `[PATHS]` / `[VERDICT]` / `[GO]` as self-contained response identifiers. The model knows which phase it's in without turn-counting.
- **Premise checks in `[FRAME]`** — Four questions before any path exploration: Right problem (root cause or symptom)? Who benefits (specific user + human outcome)? Prove it (stage-specific forcing question). Delay test (cost of delay: high or low, and why).
- **Door type classification** — Every decision classified as one-way or two-way on entry. One-way doors auto-suggest `--deep`; low-magnitude two-way doors auto-suggest `--quick`. Bezos framework applied mechanically, not decoratively.
- **`--decide` inbound handoff** — Any gstack skill can route a decision fork to CPO via the `CPO Handoff Request` block. CPO skips urgency validation (the calling skill did it) and suggests returning to the caller after verdict.
- **Prior art scan** — Preamble loads last 5 decisions and surfaces keyword matches before framing a new decision.
- **Signals integration** — Preamble reads `~/.cpo/signals/*-latest.yaml` and surfaces red-flag signals from other skills inside `[FRAME]`.
- **`--journal` flag** — Reads last 10 decision entries. Explicit read mode — distinct from automatic journal write (which happens after every verdict).
- **`--review` flag** — Surfaces all active decisions and prompts for current data against kill criteria.
- **gstack discovery** — CPO is surfaced as a suggestion in `/office-hours` (design doc approval gate), `/plan-ceo-review` (Step 0A premise challenge), and the master gstack skill (three new trigger descriptions).
- **HARD GATE RULE** — Added to top of The Flow section: `[FRAME]` and `[PATHS]` responses MUST end with AskUserQuestion. `[VERDICT]` is terminal.

### Changed

- **`--save-context` reduced to 5 questions** (was 6). Operating bias question removed.
- **`--go` adds door type** — `[GO]` template now includes `*Door type: [one-way / two-way].*`
- **`--deep` Gate 2 offers A/B/C only** — In `--deep` mode, the 10-section expansion already covers what 1/2/3 would; Gate 2 no longer offers the pressure-test options.
- **`--save-context` asks one question at a time** — Previously unspecified; now explicit: "Ask one question at a time via AskUserQuestion."
- **Journal write timing** — "After every verdict" is now explicitly defined for `--go`/`--quick`: write immediately after generating the response, before the user replies.
- **Preamble bash** — Replaced `xargs cat` (breaks on special characters) with `while read -r f; do cat "$f"; echo "---"; done`.
- **`--decide` skips premise checks** — Skips "Right problem?", forcing question, and delay test. Keeps "Who benefits?" (still needs User Truth grounding). `#name` takes precedence over prior art rule.
- **Conditional skip** — Preserved meaning: emit `[FRAME]` with premise checks, skip grounding AskUserQuestion only; emit `[FRAME]+[PATHS]` in one response.

### Removed

- **30+ flags eliminated** — `--status`, `--brief`, `--kills`, `--graph`, `--outcome`, `--verify`, `--score`, `--assumptions`, `--replay`, `--patterns`, `--sell-up`, `--drift`, `--invalidate`, and 17 more. All replaced by 8 essential flags.
- **20 modes eliminated** — `advisory-roundtable`, `boardroom`, `investor-roundtable`, `investor-story`, `board-story`, `board-memo`, `narrative`, `gtm`, `launch-os`, `red-team`, `premortem`, `postmortem`, `eng-brief`, `eng-translate`, `ceo`, `discovery`, `org-design`, `sequence`, `blue-ocean`, `sell-up`.
- **60 reference files eliminated** — All `flags/`, `modes/`, `domains/`, `internal/` directories archived. Replaced by 3 reference files: `examples.md`, `frameworks.md`, `handoff-contract.md`.
- **Self-scoring/replay/batting-average** — Decision quality score, counterfactual replay, prediction verification, and founder pattern drift removed as structurally dishonest (the model was scoring its own outputs).
- **Investor/board simulations** — Removed from core skill. Not a product decision framework feature.
- **Decision decay index / coherence validator / dependency graph** — Removed. These require persistent computational state the skill cannot reliably maintain.
- **CURSOR.md** — Removed; Cursor users use SKILL.md directly.

---

## [2.7.1] — 2026-03-20

**Recommendation block above paths.** The answer is now always first.

### Changed

- **Recommendation position moved above path list** — Instead of the recommendation being inline with one of the path letters (buried mid-list), it now leads as a dedicated block: `**We recommend [letter]:** [one-sentence rationale]`. This appears before the path list in Response 2, `--go`, and after every path rewrite in the challenge loop. The recommendation is always at the same visual position regardless of which letter is recommended.
- **`← recommended` marker retained** inline on the corresponding path for machine parsing and verification subagent Check 4. The letter in the block must match.
- **Dropped `**Recommended** ·` prefix** (added in 2.7.0) — superseded by the cleaner block-above pattern.
- `SKILL.md` — Updated Response 2 template, `--go` template, freeform input handler inline rule.
- `references/four-actions.md` — Updated Action 3 path format spec with example block.
- `references/internal/verification-subagent.md` — Check 4 updated to verify block-above presence + letter consistency.

---

## [2.7.0] — 2026-03-20

**Two UX features: conversational flow + recommended path visibility.** CPO now feels like talking to an advisor who uses a framework, not navigating a menu system.

### Added

- **Freeform input at every gate** — When the user types something that isn't a recognized option (A/B/C, 1/2/3, D-M) at any decision point, CPO engages conversationally: answers the question or responds to the idea in 2-4 sentences, integrates implications into the paths if relevant, and re-surfaces the same decision point. No new flag, no new menu option — just natural conversation without breaking the structured flow.
- **Recommended path visual treatment** — The recommended path now leads with bold `**Recommended**` prefix before the label for instant scannability: `B) **Recommended** · **[Label]** — [description] ← recommended`. The `← recommended` end marker is preserved for machine parsing and verification subagent Check 4. `*Why:*` replaced with `**Rationale:** *[explanation]*` for clearer visual hierarchy.

### Changed

- `SKILL.md` — Version bumped to 2.7.0. Updated Response 2 and `--go` templates with new recommended path format. Added freeform input handler to inline loop behaviors.
- `references/four-actions.md` — Updated Action 3 path format spec. Added freeform input handler section under pre-path challenge rules.
- `references/internal/verification-subagent.md` — Check 4 updated to validate both `**Recommended**` prefix and `← recommended` end marker, plus `**Rationale:**` line.

---

## [2.6.3] — 2026-03-20

**Comprehensive audit — 22 issues identified, 10 fixed.** Full cross-file consistency pass across the entire CPO codebase. Focused on spec coherence between reference files, pipeline ordering, schema completeness, and gate sequencing.

### Added

- **`entry_type` field in journal schema** — New field classifying entries as `decision` (default) or `ship_event` (shipped artifact). `--brief` scans for `ship_event` to render "Recent ships" section. Previously referenced in brief.md but missing from schema.
- **`verified:` weighting in `--score`** — Score computation now weights entries: `verified: yes` = 1.0, `verified: no` = 0.75, missing (pre-v1.6) = 1.0. Display note shows breakdown.
- **`verified:` fallback in `--patterns`** — Pre-v1.6 entries without `verified:` field treated as full-weight (1.0), consistent with `--score` weighting.
- **Weak Truth threshold in `--score`** — Weakest Truth only set if accuracy < 70%. If all Truths >= 70%, writes `none (all ≥70%)` and suppresses preamble-handler weak Truth behaviors.

### Fixed

- **Trace checkpoint ordering** — `truth_subagents` and `verification` were listed after `journal` in trace.md. Reordered to match Execution Pipeline: truth_subagents (order 5, during Assess), verification (order 8, post-output), journal (order 9). Added Pipeline stage column.
- **Check 6 D-M menu confidence gate** — Verification subagent Check 6 implied D-M menu scope varied by confidence but didn't clarify it renders at ALL levels. Now explicit: High = D-M, Medium/Low = D-L, elevation prompt renders alongside (not blocking) D-M.
- **Kill criteria vs elevation gate sequencing** — four-actions.md had both gates but no ordering. Now explicit: kill criteria gate fires FIRST (≥3 required), then elevation check, then D-M menu renders.
- **Truth subagent context table** — Referenced "burn rate" and fields not in context.md. Aligned to actual context.md fields + prompt inferences per Truth.
- **Execution Pipeline enrichment ordering** — Made explicit: `--scan-strategy` → `--since` → `--roadmap`, with reframe check rules.

### Changed

- `SKILL.md` — Version bumped to 2.6.3.
- `references/internal/trace.md` — Added Order column and Pipeline stage column. Reordered checkpoints.
- `references/internal/verification-subagent.md` — Check 6 rewritten for confidence-level clarity.
- `references/internal/journal-schema.md` — Added `entry_type` field and YAML template entry. Schema v1.6 (no bump — additive field).
- `references/four-actions.md` — Added explicit gate sequencing paragraph.
- `references/flags/patterns.md` — Added `verified:` fallback rule for pre-v1.6 entries.
- `references/flags/score.md` — Added verification weighting section and weak Truth threshold.

---

## [2.6.2] — 2026-03-20

**Panel review fixes — Gary Tan, Mike Krieger, Boris Cherny.** 8 issues from post-ship panel review of v2.6.1. Biggest changes: `verified:` field in journal schema, mode precedence pipeline, per-Truth context filtering, and Check 3 measurability enforcement.

### Added

- **`verified:` field in journal schema** — Every journal entry now carries `verified: yes/no`. `yes` for `--deep` or `--go` outputs (verification subagent ran). `no` for `--quick`, standard three-response flow, simulations, and dry-run commits. Scanners (`--score`, `--patterns`) weight `verified: no` entries lower. Schema bumped to v1.6.
- **Mode precedence pipeline (`SKILL.md`)** — Explicit 7-step execution order: Preamble → Enrichment → Reframe check → Primary mode → Post-output (verification + truth subagent synthesis) → Journal write → Trace write. With worked example for `--dry-run --deep --since [date] #decision-id`. Eliminates flag-combo ambiguity.
- **Truth subagent per-Truth context filtering** — Shared context is now filtered per-Truth: Economic Truth gets runway/burn, Execution Truth gets team size/tech stack, User Truth gets user segment — not all constraints to all Truths. Prevents anchoring via irrelevant context bleed.
- **`_DRY_START_DATE` timestamp tracking** — Dry-run now captures start date at invocation. Journal entry `date:` on `N) Commit` uses `_DRY_START_DATE`, not the commit timestamp. Prevents Decision Decay Index from drifting by exploration session duration.
- **Coherence + truth-subagent conflict integration** — When a truth-subagent cross-Truth conflict mirrors an active coherence contradiction, a single merged warning fires instead of two separate notes. Prevents duplicate warnings on the same underlying tension.

### Fixed

- **[Boris] Check 3 measurability non-mechanical** — "Stays reasonable" passed the check because it's semantic, not numeric. Check 3 now requires all three: named metric + numeric/percentage threshold + calendar timeframe. Qualitative language explicitly fails. If user intent is clear, model infers numeric values from context rather than leaving placeholders.
- **[Mike] Check 6 `--quick` false-fail** — `--quick` intentionally omits D-M menu, but Check 6 previously required D-M menu presence unconditionally. Check 6 now auto-passes when `--quick` is active.
- **[Mike] Truth subagents returning-decision base case missing** — When a returning decision's first `--deep` run has no prior Truth findings, the spec was silent. Now: base case is "user context + stage + doctrine only, no delta handling."
- **[Gary] Dry-run commits unweighted in `--score`** — Decisions committed from dry-run (never interactively pressure-tested) were indistinguishable from fully deliberated decisions. `verified: no` field marks them for lower weighting.

### Changed

- `SKILL.md` — Version bumped to 2.6.2. Added Execution Pipeline section with 7-step precedence order.
- `references/internal/journal-schema.md` — Schema v1.6: added `verified:` field to YAML template and field rules. Added `_DRY_START_DATE` timestamp support. Added coherence + truth-subagent conflict integration note.
- `references/flags/dry-run.md` — Added `_DRY_START_DATE` bash capture and commit timestamp spec.
- `references/internal/truth-subagents.md` — Step 1 rewritten with per-Truth context filter table. Base case added. Returning decision handling preserved.
- `references/internal/verification-subagent.md` — Check 3 rewritten with numeric/calendar enforcement. Check 6 updated with `--quick` auto-pass and `N) Commit` clarification.

---

## [2.6.1] — 2026-03-20

**Post-ship audit fixes for v2.6.0.** 8 bugs found across the 3 new features.

### Fixed

- **[Critical] `--since` classified contradictorily** — `combinations.md` listed `--since` as both a valid dry-run stack AND an invalid journal-reading flag. Fixed: `--dry-run --since [date] [question]` is valid (temporal enrichment + dry-run analysis); `--dry-run --since` alone (no question) is invalid. Updated `combinations.md` and `dry-run.md`.
- **[Moderate] `N) Commit` escape hatch ambiguity** — No spec for how `N) Commit` interacts with the verification subagent's Check 6 or with M)'s conditionality. Fixed: `dry-run.md` now specifies N always appears after the last D-M option (after M if present, after L otherwise). `verification-subagent.md` Check 6 now explicitly allows `N) Commit` during `--dry-run` without failing the check.
- **[Moderate] `--dry-run #decision-id` behavior undefined** — Returning decision flow (DECISION_OBJECT_LOADED) was not specified for dry-run. Fixed: loads prior context for delta frame, does NOT write revision N+1 or supersede prior revisions. `N) Commit` persists as a real revision.
- **[Minor] Truth conflict synthesis timing unclear** — `truth-subagents.md` Step 4 was ambiguous about whether synthesis happened before or after Section 2 in `--deep` output. Fixed: synthesis runs as a closing block inside Section 2 (after all five independent findings), so Section 3 paths are built from complete conflict-aware context.
- **[Minor] Returning decision baseline for Truth subagents** — `truth-subagents.md` didn't specify whether prior Truth findings should be in shared context. Fixed: prior findings are excluded from Step 2 (fresh independent assessment); a delta block is added to Section 2 after synthesis.
- **[Minor] Verification subagent Check 7 fingerprint consistency** — Check 7 only validated presence, not correctness. Fixed: added three sub-checks: Dominant in Grounded/Inferred list, no Truth in both lists simultaneously.
- **[Minor] `--dry-run --export` output target undefined** — Fixed: exports to `~/.cpo/exports/[date]-[mode]-dry-run.md` with `-dry-run` marker in filename.
- **[Minor] Trace write ambiguity during `--dry-run` verification** — Verification subagent fires during `--dry-run --deep/--go` but trace is suppressed. Fixed: `verification-subagent.md` and `journal-schema.md` both now specify trace checkpoint is also suppressed during `--dry-run`; only inline output fixes apply.

### Changed

- `references/flags/dry-run.md` — Combination rules expanded with `--since` split behavior, `--export` filename spec, `#decision-id` dry-run behavior, and `N) Commit` clarification.
- `references/flags/combinations.md` — `--dry-run` section rewritten to resolve `--since` contradiction.
- `references/internal/truth-subagents.md` — Step 1 added returning decision handling; Step 4 Section 2 timing made explicit.
- `references/internal/verification-subagent.md` — Check 6 updated for `N) Commit`; Check 7 expanded with three sub-checks; trace integration clarified for `--dry-run`.
- `references/internal/journal-schema.md` — `--dry-run` gate clarified: trace suppressed but verification subagent inline checks still run.

---

## [2.6.0] — 2026-03-20

**Three new capabilities from Boris Cherny workflow analysis.** Exploratory mode, mechanical spec verification, and independent per-Truth analysis.

### Added

- **`--dry-run` flag** — Run the full four-action flow without writing to the decision journal. For "what if" exploration without polluting the Intelligence Loop's data. Stacks with `--go`, `--deep`, `--quick`, `--export`. Includes `N) Commit` escape hatch in D-M menu to persist a dry-run decision. Invalid with journal-reading flags (graceful fallback). New file: `references/flags/dry-run.md`.
- **Verification subagent (`--deep` and `--go`)** — Post-output mechanical verification pass that enforces 8 spec compliance checks: exactly 3 paths, structural (not risk-posture) labels, measurable kill criteria (metric + threshold + timeframe), `← recommended` marker, evidence tags, D-M menu, truth fingerprint, confidence rating. Fixes violations inline before the user sees output. Replaces honor-system self-checks with mechanical enforcement. New file: `references/internal/verification-subagent.md`.
- **Truth subagents (`--deep` only)** — Independent per-Truth assessment protocol for Section 2 (Five Truths Assessment). Each Truth is assessed in isolation using only shared context — no cross-Truth anchoring. Conflicts between Truths are synthesized only after all five assessments complete, making them visible instead of unconsciously smoothed. New file: `references/internal/truth-subagents.md`.

### Changed

- `SKILL.md` — Version bumped to 2.6.0. Added `--dry-run` to Flag Routing table. Added verification subagent reference to Self-Check Assertions section. Added truth subagents reference to `--deep` Response Format section.
- `references/internal/journal-schema.md` — Added `--dry-run` gate to "When to Write" section.
- `references/internal/trace.md` — Added `truth_subagents` and `verification` checkpoint rows.
- `references/flags/help.md` — Added `--dry-run` to flags list.
- `references/flags/combinations.md` — Added `--dry-run` combination rules section.

---

## [2.5.4] — 2026-03-20

**Full codebase audit — 75 files read, 7 bugs fixed across 8 files.** Comprehensive cross-reference audit of every file in the skill: all 19 modes, 30+ flags, 5 domains, 6 internal files, and all misc references. Panel-reviewed on Opus by Gary Tan (YC), Mike Krieger (Anthropic CPO), Boris Cherny (Head of Claude Code).

### Fixed

- **[Boris] Preamble loads superseded entries** — Decision journal preamble loaded 5 most recent by mtime, ignoring status. After v2.5.3 supersession `sed` updates mtime, superseded entries would sort to top and crowd out active decisions. Now filters for `status: active` before loading.
- **[Boris] `--outcome` fragile sed for consequence updates** — Used `sed {n;n;s/...}` to update consequence status, which breaks if schema changes or marker text contains special chars. Now uses Edit tool (available since v2.5.3).
- **[Mike] `--trail` references Bold/Balanced/Conservative** — Table example showed `[balanced/bold/conservative]` for path chosen, directly contradicting SKILL.md's prohibition on risk-posture labels. Now shows `[A/B/C — situational label]`.
- **[Boris] `--save-context` says "5 fields" but has 6** — Operating bias (added in v2.x) wasn't counted. Fixed to "6 fields".
- **[Gary] `help` missing 9 flags** — `--patterns`, `--status`, `--graph`, `--kills`, `--decide`, `--compare`, `--context`, `--no-context`, `--focus` were all missing from the help output. Added all. Also added `score-profile.md` and `signals/` to persistent layers list.
- **[Mike] personas.md "Nick Krieger"** — First name was wrong. Fixed to "Mike Krieger" (Anthropic CPO, Instagram co-founder).
- **[Boris] `--drift` bash `head -200` truncation** — Piped output through `head -200` which could truncate entries. Replaced with a counted loop that loads exactly 10 non-invalidated/non-superseded entries.

### Changed

- `SKILL.md` — Preamble decision load now uses `grep -l "status: active"` filter; version bumped to 2.5.4.
- `references/flags/trail.md` — Path chosen column example corrected.
- `references/flags/outcome.md` — Consequence updates use Edit tool instead of sed.
- `references/flags/save-context.md` — Field count corrected to 6.
- `references/flags/help.md` — All 35 flags listed; persistent layers updated.
- `references/flags/drift.md` — Bash rewritten as counted loop without line truncation.
- `references/personas.md` — Name corrected.

---

## [2.5.3] — 2026-03-20

**Structural integrity fix — revision supersession + Edit tool.** Deep panel review (Opus pass). 3 bugs, 1 root cause: the journal never superseded prior revisions, corrupting every scanner in the Intelligence Loop.

### Fixed

- **[Boris/Gary] Revision supersession** — When a `decision_id` gets revision N+1, prior revisions now marked `status: superseded` via targeted `sed` on the top-level status line. All scanners (`--verify`, `--assumptions`, `--brief`, `--score`, coherence validator) grep for `status: active` — superseded revisions are now automatically excluded. Audit trail preserved via `--history` and `--replay`.
- **[Boris] Edit tool added to `allowed-tools`** — Consequence updates in `--verify` now use surgical Edit instead of fragile Bash file rewrites. Prevents YAML corruption from LLM reconstruction errors.
- **[Mike] `--verify` division by zero** — If all consequences are pushed as Inconclusive (0 confirmed, 0 disconfirmed), accuracy calculation now shows "no predictions resolved" instead of dividing by zero.

### Changed

- `references/internal/journal-schema.md` — Added revision supersession step to the write flow with bash block. `sed` targets `^status: active$` only — consequence-level status fields (indented) are unaffected.
- `references/flags/verify.md` — Step 3 now specifies Edit tool for consequence updates. Step 4 handles zero-denominator case.
- `SKILL.md` — Added `Edit` to `allowed-tools` frontmatter.

---

## [2.5.1] — 2026-03-20

**Panel review pass — 5 spec bugs fixed.** Test + review by Gary Tan (YC), Mike Krieger (Anthropic CPO), Boris Cherny (Head of Claude Code). All critical behavioral bugs corrected; no new features.

### Fixed

- **[Boris] Frontmatter version mismatch** — `SKILL.md` front matter said `version: 2.3.0`; bash block said `2.5.0`. Skill registries read frontmatter. Corrected to `2.5.0`.
- **[Boris/Gary] `--score --since` profile corruption** — Running `/cpo --score --since [date]` wrote a partial-period analysis as the permanent score profile, overwriting full history. Now gated: Step 4 only writes the profile when `--since` is absent. When `--since` is present, user is notified that the profile was not updated.
- **[Mike] Transparency inconsistency in confidence adjustments** — Hot calibration silently downgraded High → Medium while Dominant Truth cap showed a visible note. Both adjustments now show a note. Consistent policy: all confidence modifications are transparent.
- **[Boris/Mike] Double-trigger overlap unspecified** — When both hot calibration AND Dominant Truth = Weak Truth fired simultaneously, the spec had no tie-breaker. Added: Dominant Truth note takes priority; hot calibration note is suppressed when the cap already fires (one note, one adjustment).
- **[Gary] No score profile staleness detection** — A profile written months ago could drive wrong behavior indefinitely. Added `SCORE_PROFILE_STALE` state: preamble bash checks profile `Generated:` date; if >90 days, emits `SCORE_PROFILE_STALE: [date]`. Handler surfaces one-time notice to refresh. Profile is still used — not discarded.

### Changed

- `SKILL.md` — Score profile bash block: added `_SP_DATE` / `_SP_AGE` check; emits `SCORE_PROFILE_STALE` when >90 days old. Frontmatter version corrected to `2.5.0`. State handling hint updated to include `SCORE_PROFILE_STALE`.
- `references/flags/score.md` — Step 4 gated behind `--since` absence check; added user-facing note when gate fires.
- `references/internal/preamble-handlers.md` — Hot calibration modifier now explicit with a note (not silent). Double-trigger rule added. `SCORE_PROFILE_STALE` handler section added.

---

## [2.5.0] — 2026-03-19

**The true closed loop — CPO adjusts its behavior based on your decision history.** After running `--score`, CPO writes a compact score profile to `~/.cpo/score-profile.md`. Every future session loads this profile silently at startup and uses it to surface your weakest Truth more aggressively, prioritize it first in Blind spots (even when grounded), and cap Confidence from High to Medium when your track record on that Truth is low. CPO now knows your judgment better over time.

### Added

- **Score profile write (Step 4 of `--score`)** — After computing scores with ≥3 verified decisions, writes `~/.cpo/score-profile.md` with: weakest/strongest Truth, per-Truth accuracy, and calibration classification (hot / calibrated / cold). Profile persists across sessions.
- **Score profile preamble load** — Preamble bash block now checks for `~/.cpo/score-profile.md` at session start. Outputs `SCORE_PROFILE_FOUND` (with profile content) or `NO_SCORE_PROFILE`.
- **Profile-aware Assess** — When the Dominant Truth matches your weakest historical Truth, appends one bracketed note: *[Profile: [Truth name] is your weakest historically at [X]% — treating as inference-heavy regardless of available data.]*
- **Profile-aware Blind spots** — Weak Truth always appears first in blind spots list, even if it was grounded in this analysis. Labeled with historical accuracy.
- **Profile-aware Confidence** — When Dominant Truth = Weak Truth and computed confidence would be High, caps at Medium. Notes what specific data would unlock High.
- **Calibration modifier** — `hot` calibration (High calls <70% accurate): silently downgrades High → Medium. `cold` calibration (High calls >90% accurate): adds one post-Verdict note suggesting bolder commitment.

### Changed

- `references/flags/score.md` — Added Step 4 (score profile write with exact bash block). Never writes placeholder values to disk.
- `references/internal/preamble-handlers.md` — Added SCORE_PROFILE_FOUND / NO_SCORE_PROFILE handler section with full behavior specification.
- `SKILL.md` — Added 8-line score profile load to preamble bash block; bumped version to 2.5.0.

### Size

Net additions: +45 lines across 3 files. All behavior in reference files; SKILL.md core grew 8 lines. Total skill size unchanged from lazy-load architecture.

---

## [2.4.0] — 2026-03-19

**Decision Intelligence Loop — CPO now gets smarter about you the more you use it.** 5 new capabilities that close the feedback cycle: consequence verification, decision quality score, assumption tracker, counterfactual replay, and automatic decision similarity matching. Panel-reviewed to 9.5/10 by Gary Tan (YC), Mike Krieger (Anthropic CPO), Boris Cherny (Head of Claude Code). 10 spec fixes across 2 review rounds.

### Added

- **`--verify`** — Consequence verification loop. Surfaces predictions past their check date for 3-second verify (confirmed / disconfirmed / push +30 days). 1-3 consequences verify inline during `--brief`; 4+ batch via `--verify`. Updates specific consequence `status:` field in YAML — never corrupts top-level fields.
- **`--score`** — Decision quality score. Computes prediction accuracy, confidence calibration (does "High" actually mean >80%?), blind spot rate, and per-Truth accuracy. Minimum 3 verified decisions. Read-only.
- **`--assumptions`** — Assumption tracker. Ages every ungrounded inference across active decisions: Fresh (<30d) / Aging (30-60d) / Stale (60-90d) / Critical (>90d). Validation method is decision-context-specific — not generic placeholders.
- **`--replay #name`** — Counterfactual replay. Re-runs a past decision with new information: shows which Truths shift, updated paths, whether the verdict would have changed, and a one-line Lesson — the meta-learning for next time. Read-only, no journal write.
- **Decision similarity (Check 4 in coherence pipeline)** — Automatic. At journal write time, scans for structurally similar past decisions with verified outcomes. Surfaces: "Pattern match: this resembles #[id] ([date]) — that one [succeeded/failed] because [reason]. Run `--replay` to compare." Requires at least one verified consequence before firing.
- **`--brief` consequence verification inline** — 1-3 due consequences now verify inside `--brief` instead of redirecting to `--verify`. Make it effortless.
- **`--brief` aging assumptions alert** — When STALE or CRITICAL assumptions exist (>60d unvalidated), brief adds one line: "[N] assumptions aging past 60 days — run `--assumptions` to review."

### Changed

- `brief.md` — Consequence checks due section updated to inline-verify 1-3, redirect 4+ to `--verify`
- `brief.md` — Added Aging assumptions section
- `journal-schema.md` — Added Check 4 (decision similarity) to Spec Coherence Validator pipeline
- `help.md` — Added `--verify`, `--score`, `--assumptions`, `--replay #name` to flag list; added `briefs/` to persistent layers
- `SKILL.md` — Added 4 new flag routing entries

### Fixed

- `--score` kill discipline metric removed (no data source in schema) — replaced with Truth accuracy per Dominant Truth
- `--score` minimum data threshold lowered from 5 to 3 decisions

---

## [2.3.0] — 2026-03-19

**7 structural intelligence features — capabilities structurally impossible in traditional PM workflows.** Panel-reviewed to 10/10 by Gary Tan (YC), Mike Krieger (Anthropic CPO), Boris Cherny (Head of Claude Code). 28 spec fixes across 2 review rounds.

### Added

- **Spec Coherence Validator** — write-time linter checks new journal entries against all active entries for contradictions, scope-aware assumption collisions, and kill criteria overlap. Warnings surface in ALL modes (standard, verbose, deep). Capped at 3 warnings per write.
- **Decision Dependency Graph (`--graph`)** — directed graph across active decisions with bottleneck scoring (fan-out + staleness penalty + confidence discount), edge confidence (strong `→` [explicit] vs weak `→?` [inferred]), small-graph simplification for ≤5 decisions, schema version gate (v1.5+), cross-reference with `--kills` for compounding risk.
- **Kill Criterion Countdown (`--kills`)** — cross-cutting dashboard of ALL active kill criteria with days remaining, 5 status classifications (TRIGGERED / APPROACHING / ACTIVE / NO_TIMEFRAME / EXTERNAL), date anchor rules, all-green happy path, cross-reference with `--graph`.
- **Consequence Tracking** — auto-generated falsifiable predictions at decision time (2-3 per decision), check_date derived from kill criteria timeframes (50-75% of shortest), status lifecycle (pending → confirmed / disconfirmed), integrated into `--brief`, `--status`, and `--outcome`.
- **Decision Decay Index** — mechanical scoring formula `(days/divisor) × (inferred/5) × confidence_multiplier` with plain-English labels (Fresh / Aging / Stale / Degraded). `shelf_life` field for long-horizon decisions prevents false decay alarms.
- **Session Replay** — hindsight bias guard in `--outcome` that reconstructs information state at decision time before recording outcomes. Displays Truth fingerprint, open questions, kill criteria, paths considered, and predictions made. Pre-v1.4 graceful fallback.
- **Founder Pattern Drift** — rolling 10-decision windows comparing recent vs prior behavioral patterns across 5 dimensions. Sort by YAML `date:` field, not filesystem mtime.
- **`path_chosen` field** — new journal schema field tracking which path letter (A/B/C) the founder selected. Powers `--patterns` path preference analysis and recommended-path adherence rate.
- **`shelf_life` field** — optional journal field (days) for long-horizon decisions. Used by Decision Decay Index instead of default 30-day divisor.
- **`consequences` field** — journal schema field with `marker`, `check_date`, and `status` per consequence.

### Changed

- `--brief` bash loader now filters `status: active` (was loading invalidated entries)
- `--brief` adds Consequence checks due and Decision decay sections
- `--status` corrected to "8 steps (with sub-steps)" — added Steps 6b (Consequence check) and 6c (Decision decay)
- `--status` verbose trace template now includes 6b and 6c
- `--outcome` removes stale "Bold/Balanced/Conservative" reference
- `--outcome` adds Session Replay, consequence status updates, and learning extraction
- `--patterns` Path Preference rewritten: letters aren't risk levels, measures recommended-path adherence rate
- `--patterns` adds Rolling Window Analysis with sort-by-YAML-date rule
- Journal schema version remains 1.5 with new fields
- Version bumped to 2.3.0
- 30 flags (was 28)

---

## [2.2.0] — 2026-03-19

**`--status` 9-point overhaul + operating bias + cross-skill signals contract.**

### Added

- `operating_bias` field in `--save-context` (build-first / research-first / relationship-first)
- `--status` action recommendations calibrated to operating bias
- Cross-skill signals contract (`~/.cpo/signals/`) — other skills can write structured YAML that enriches `--status`
- Git velocity interpretation in `--status` (commits trailing 7d vs prior 7d, merge freshness, stale branches)
- Ship-to-decision alignment check in `--status`
- Delta detection across `--status` runs via `~/.cpo/last-status.yaml`

### Changed

- `--status` synthesis expanded from 5 to 8 steps (with sub-steps)
- `--status` force-rank priority order formalized (7 levels)
- Version bumped to 2.2.0

---

## [2.1.0] — 2026-03-19

**Architecture split + `--invalidate`, `--invalidate-all`, `--drift` flags.**

### Added

- `--invalidate [topic or #id]` — retire superseded decisions with annotation
- `--invalidate-all` — bulk invalidation with confirmation gate; `--hard` variant for permanent deletion
- `--drift` — on-demand logic drift detection across recent decisions
- `references/flags/invalidate.md`, `references/flags/drift.md`
- `references/signals-contract.md` — cross-skill signals specification

### Changed

- SKILL.md architecture: mode/flag stubs compressed to routing tables
- Version bumped to 2.1.0

---

## [2.0.0] — 2026-03-19

**Major refactor: SKILL.md split from 1,430 → 403 lines.** Panel consensus (Gary Tan, Mike Krieger, Boris Cherny) was that the file was too big for consistent model compliance — attention is finite and rules compete. Behavioral core (templates, ⛔ gates, hard rules) now dominates signal-to-noise. Detailed generation rules moved to reference files loaded on demand.

### Architecture

- **SKILL.md:** 403 lines — preamble, response templates + ⛔ gates, Five Truths, hard rules, mode/flag routing tables, self-check assertions, red flags
- **`references/four-actions.md`:** All detailed Action 1-4 rules — grounding option quality bar, path label generation, worked examples, D-M menu behavior, elevation loop, kill criteria quality bar, blind spots rules, structural rules, calibration protocol, user role detection
- **`references/internal/preamble-handlers.md`:** Detailed state handling for CONTEXT_LOADED_*, DECISIONS_FOUND, INTEGRATIONS_FOUND, VERSION_MISMATCH, STRATEGY_FILES_FOUND
- **`references/internal/routing-map.md`:** Internal mode routing table (user language → mode)
- **`references/internal/journal-schema.md`:** YAML schema + write rules for decision journal
- **`references/flags/help.md`:** Help invocation text

### Changed

- Mode stubs compressed from 5 lines/mode to single routing table row
- Flag stubs compressed from section-per-flag to single routing table row
- Operating principles, communication style, calibration protocol moved to `references/four-actions.md`
- Version bumped to 2.0.0

### Preserved

- All ⛔ GATE markers and MUST/MUST NOT constraints from v1.9.3-1.9.4
- All response templates (R1, R2, R3) with exact formatting
- All self-check assertions
- Preamble bash block unchanged

---

## [1.9.4] — 2026-03-19

**1/2/3 challenge block and D-M menu visibility fixes.** Model was generating its own path-selection prompt instead of using the template, and the D-M menu was being hidden by the elevation rule.

### Fixed

- Added explicit instruction: "This is the exact output for Response 2. Do not add your own path-selection question."
- Changed 1/2/3 block from "Not ready to commit?" to "Before committing — stress test, analyze, or reality check:"
- D-M menu now always renders below elevation prompt (was previously suppressed entirely)
- Fixed internal contradiction: old rule said "suppress entire D-M menu" during elevation, new rule says "always show"

---

## [1.9.3] — 2026-03-19

**Stronger gate enforcement + panel fixes.** v1.9.2 gates didn't fire in Cursor — model merged all responses, skipped 1/2/3 challenge block, invented custom options. Gates upgraded from soft "stop" to MUST contain / MUST NOT contain per response. Challenge block made structurally non-optional. CURSOR.md header updated.

### Fixed (panel: Gary Tan, Mike Krieger, Boris Cherny)

- **MUST/MUST NOT constraints per gate:** GATE 1 MUST contain frame + truth + grounding question; MUST NOT contain paths, verdict, D-M. GATE 2 MUST contain 3 paths + 1/2/3 challenge block; MUST NOT contain verdict, kill criteria, D-M.
- **1/2/3 challenge block non-optional (Krieger):** "Not ready to commit?" → "Before committing — stress test, analyze, or reality check:". Added to GATE 2 MUST contain list.
- **CURSOR.md header updated (Cherny):** States enforcement lives in SKILL.md, file is setup guide only. Primary instruction: "Just type `/cpo`."
- **Self-check wording strengthened:** "this response ends here" → "STOP. This response is complete."

---

## [1.9.2] — 2026-03-19

**Gate markers into SKILL.md — `/cpo` now self-enforcing without setup.**

Added two ⛔ GATE markers + self-check questions directly into the output format section of SKILL.md (at Response 1 and Response 2 boundaries). These were previously only in CURSOR.md, which users never loaded. Now the enforcement travels with the skill — typing `/cpo` in Cursor Composer loads SKILL.md, and SKILL.md contains the gates. No `@CURSOR.md`, no `.cursorrules`, no setup required.

### Added

- ⛔ **GATE 1** after Response 1 template: "Do not generate paths. Self-check: has the user replied with a grounding choice?"
- ⛔ **GATE 2** after Response 2 template: "Do not generate the Verdict. Do not render D-M options. Self-check: has the user replied with a path choice or pre-commitment pick?"
- Version bumped to 1.9.2 in preamble bash

### Architecture note

SKILL.md is now the single source of truth for gate enforcement. CURSOR.md remains as a setup guide for users who want to load it explicitly via `@CURSOR.md` or `.cursorrules`, but its enforcement content is no longer needed there — SKILL.md covers it.

---

## [1.9.1] — 2026-03-19

**Revert v1.9.0 architecture rewrite.** The v1.9.0 complete rewrite of CURSOR.md removed inline ⛔ GATE markers and self-check questions that were the behavioral cues making gate enforcement work in Cursor Composer. The rewrite replaced them with abstract rules at the top of the document, which models ignored entirely — producing identical broken output (all phases merged, no gates, wrong Truth names). Reverted to v1.8.12 content which has the inline markers that were working.

### Reverted

- **CURSOR.md restored to v1.8.12** — all inline ⛔ GATE markers, self-check questions, trigger table, smoke test, and structural rules restored
- **v1.9.0 architecture (instructions-first) abandoned** — the approach of putting abstract rules at line 1 while removing inline behavioral cues produced worse compliance than the original document

### Lesson

Inline behavioral cues (⛔ STOP markers placed at the exact point where the model should stop, self-check questions that force the model to verify state before proceeding) are more effective than abstract rules at the top of a document. The model needs the cue at the moment of decision, not as a pre-loaded instruction.

---

## [1.9.0] — 2026-03-19 [REVERTED in 1.9.1]

**Architecture rewrite: instructions-first CURSOR.md.** Reverted — see 1.9.1 notes.

---

## [1.8.12] — 2026-03-19

Patch: Six UX/flow bugs fixed — grounding correction first-class, D-M/1-2-3 boundary enforced, challenge loop cap removed, F surfaced post-path-commit, structural rules corrected. Root cause of screenshot bug (D/E/H/I in structural rule) eliminated.

### Fixed

- **Root cause fix — structural rule `(D/E/H/I)` → `(numbered 1/2/3)`:** The structural rules section described Response 2's terminal challenge block as `(D/E/H/I — never inside the overlay)`. D/E/H/I is post-verdict letter notation. Any model reading this spec would generate D-K at the path stage instead of 1/2/3. This is confirmed by production screenshots showing D) Stress test / G) Sell-up / J) Roadmap / K) Eng brief appearing after Paths, with no path-selection AskUserQuestion. Fixed to: `(numbered 1/2/3 — never inside the overlay, never lettered D-M)` with an explicit prohibition: "⛔ D-M letters must not appear anywhere in Response 2."
- **Final check rule disambiguated by response phase:** The universal final check said "verify the last element is AskUserQuestion, D-M menu, or contextual next-step" — applying "D-M menu" universally caused models to treat D-M as correct for all phases. Replaced with phase-specific terminal element rules: Response 1 ends with grounding AskUserQuestion; Response 2 ends with path AskUserQuestion + 1/2/3 block; Response 3 ends with D-M menu. Added: "If you are about to write D) E) F) as a terminal element, verify you are in Response 3 — if not, stop and replace."
- **Grounding correction promoted to first-class AskUserQuestion option (D) Reframe):** The correction path was buried in plain text below the grounding modal: "Or correct the frame in a sentence." In Cursor, the modal shows only A/B/C — the correction path was invisible. Fixed: grounding AskUserQuestion now always includes a 4th option `D) Reframe — [one clause naming element to correct]. Type your correction.` When D is selected or the user types free-text: re-run from Action 2, no re-ask of grounding question, no re-run of Action 1.
- **Challenge block "challenge the top path" label corrected to "challenge all three paths":** The spec body said "Challenge options run against all three paths, not just the recommended one" but the label in the challenge block said "Stress test — challenge the top path." Direct contradiction — models resolving the ambiguity would challenge path B only. Fixed in both the Action 3 pre-path challenge rules section and the Response 2 output format template.
- **Pre-path challenge loop hard cap removed:** The 2-round hard stop ("after the second challenge completes, do not re-offer 1/2/3") was paternalistic and forced commitment before the user was ready. Replaced with: no hard cap — continue offering 1/2/3 until the user commits. After 3+ rounds, add one non-blocking nudge line ("what's still unresolved?") then continue. Never force commitment.
- **D-M letter boundary rule added to CURSOR.md trigger table:** New explicit row for user correction (D or free text) + new inline boundary rule: "Response 2 uses numbers only (1/2/3). Response 3 uses letters only (D-M). If you are writing D) E) F) and have not yet received a path selection, you are in the wrong phase — stop and output 1/2/3."
- **Pre-path challenge rules extended to enumerate all D-M letters as post-verdict-only:** Previous rule said "G/H/I are post-verdict options only — never render in the pre-path challenge block." Extended to explicitly list all D-M letters: "D–M options (Stress test, Deep analysis, Reality check, Sell-up, Board sim, Investor sim, Roadmap, Eng brief, Hand off, New evidence) are post-verdict only." Closes the case where a model might reason that D/E are acceptable at the path stage because they share names with 1/2.
- **F) Reality check surfaced prominently post-path-commit:** Added one-line callout immediately after the Verdict line: "Path [letter] locked. If you want to pressure-test before acting, start with F) Reality check below." Makes F discoverable without adding a pre-verdict gate.
- **CURSOR.md smoke test expanded from 3 to 5 prompts:** Added steps to verify D) Reframe triggers correctly (re-runs Assess → Paths + 1/2/3, not Verdict) and that 1) Stress test fires correctly (analysis + re-surfaced path AskUserQuestion + 1/2/3, not Verdict). Step 5 confirms D-M menu contains letters not numbers.
- **Critical Output Rules version tag updated:** v1.8.11 → v1.8.12.

---

## [1.8.11] — 2026-03-19

Patch: CURSOR.md-excluded feature register added — closes Boris Cherny's last remaining spec gap.

### Fixed

- **CURSOR.md-excluded feature register added:** Co-located with the feature compatibility rule and Conditional Grounding disabled note. Provides an explicit enumerated list of SKILL.md features intentionally absent from CURSOR.md, with version and reason for each exclusion. Solves the re-invention risk: a future editor adding a CURSOR.md exception can now check this register to discover whether the feature was already tried and disabled. Current entry: Conditional Grounding (disabled v1.8.8; requires AskUserQuestion modal to enforce R2→R3 gate; SKILL.md-only). Register includes a `*(Add future exclusions here with version and reason.)*` placeholder to signal it is a live document.
- **Critical Output Rules version tag updated:** v1.8.10 → v1.8.11.

---

## [1.8.10] — 2026-03-19

Patch: DECISION_OBJECT_LOADED added to trigger table; pre-response gate check; CURSOR gate verification test; STRATEGY_FILES_FOUND bullet trimmed.

### Fixed

- **`DECISION_OBJECT_LOADED` row added to trigger table:** The table claimed to be exhaustive but omitted returning decisions — a `/cpo #name` call with a loaded decision object collapses GATE 1 (delta frame replaces grounding question), while GATE 2 still applies. Added as a new row: Initial `/cpo` call + `DECISION_OBJECT_LOADED` → Delta frame (no grounding question) → GATE 2. The table is now actually exhaustive.
- **Gate exception scope rule updated to enumerate all 3 exceptions:** Previous wording said "`STRATEGY_FILES_FOUND` collapses GATE 1 only. `--go`/`--quick` collapse all gates. No other exceptions exist." Now reads: "Exactly three exceptions exist in CURSOR.md: `STRATEGY_FILES_FOUND` collapses GATE 1 only; `DECISION_OBJECT_LOADED` collapses GATE 1 only; `--go`/`--quick` collapse all gates." Eliminates the false implication that only 2 exceptions exist.
- **Pre-response gate check added to Critical Output Rules:** New bullet immediately after the trigger table: "Before writing the first word of any response, identify which table row matches your current situation. Generate exactly what that row specifies — nothing more. If the gate column says 'wait for user reply,' your output ends there." Forces explicit table lookup before model begins writing.
- **STRATEGY_FILES_FOUND bullet trimmed:** The standalone bullet restated what the trigger table already said (Frame + Assess + Paths + GATE 2). Trimmed to non-redundant information only: "The user's angle selection IS the confirmed frame — no grounding question. ⛔ GATE 2 applies — stop after the 1/2/3 block. Do not generate the Verdict."
- **CURSOR gate verification test added to Critical Output Rules:** New final bullet with a 3-prompt smoke test: ① standard prompt → Frame + grounding only (fail = Paths appeared without user reply); ② reply A → Paths + 1/2/3 only (fail = Verdict appeared); ③ reply A → Verdict + D–M menu (gates verified). Gives editors and users a behavioral contract that is testable, not just spec-asserted.
- **Critical Output Rules version tag updated:** v1.8.9 → v1.8.10.

---

## [1.8.9] — 2026-03-19

Patch: Exhaustive trigger→output→gate table; gate exception scope rule; CURSOR.md vs SKILL.md environment compatibility guardrail.

### Fixed

- **Exhaustive trigger→output→gate table added to Critical Output Rules:** The ONE RESPONSE PER TURN rule previously stated the principle but did not enumerate every legal trigger. A model could reason its way into an unlisted exception (e.g., treating `STRATEGY_FILES_FOUND` as collapsing GATE 2 as well as GATE 1). Replaced the prose rule with an exhaustive table: Initial `/cpo` call (standard) → Frame + Assess + grounding question → GATE 1; Initial `/cpo` call + `STRATEGY_FILES_FOUND` → Frame + Assess + Paths + 1/2/3 block → GATE 2; User replies with grounding → Paths + 1/2/3 block → GATE 2; User replies with path or pre-commitment → Verdict + D–M menu → None; User picks D–M option → That one option + re-surface remaining → None; `--go`/`--quick` → Single condensed response → None. The table is explicitly marked exhaustive.
- **Gate exception scope rule added:** Codified that an exception collapsing GATE N does NOT automatically collapse GATE N+1. `STRATEGY_FILES_FOUND` collapses GATE 1 only. `--go`/`--quick` collapse all gates. No other exceptions exist in CURSOR.md. Added inline to the trigger table and cross-referenced from the `STRATEGY_FILES_FOUND` bullet.
- **CURSOR.md vs SKILL.md environment compatibility rule added:** Added a guardrail for future editors: before adding any gate exception to CURSOR.md, the editor must ask "Does a mechanical pause enforce this gate (e.g., AskUserQuestion modal)?" If not, the exception is unsafe and belongs in SKILL.md only. This rule is co-located with the Conditional Grounding disabled note to create a single authoritative location for CURSOR.md exception policy.
- **Critical Output Rules version tag updated:** v1.8.8 → v1.8.9.

---

## [1.8.8] — 2026-03-19

Patch: Conditional Grounding disabled in CURSOR.md; Verdict anti-hallucination rules added to Critical Output Rules.

### Fixed

- **Conditional Grounding DISABLED in CURSOR.md (root cause of gate regression):** The exception said "collapse R1+R2 into a single response when the prompt is specific." This directly contradicted the ONE RESPONSE PER TURN rule ("initial call = R1 only"). Cursor models resolved the contradiction by doing R1+R2+R3 in one pass. The exception is not compatible with Cursor's agentic mode where there is no mechanical pause mechanism. In CURSOR.md, GATE 1 is now unconditional — always ask a grounding question regardless of prompt specificity. The Conditional Grounding exception remains available in SKILL.md where AskUserQuestion creates a hard mechanical pause.
- **Verdict format anti-hallucination rules added to Critical Output Rules:** The model was inventing non-spec sections in Response 3 ("Why:" block, "Weekend execution plan" table, "Dependencies to confirm", "Concrete sequence"). Added to the read-first section: prohibited sections explicitly enumerated; one-line reason belongs on the Verdict line itself, not in a separate paragraph.
- **Processing narration prohibited:** Added to Critical Output Rules: "Never open with 'Using the CPO skill to...' or 'Treating this as...' — start directly with `*I'm reading this as:`."
- **Critical Output Rules version tag updated:** v1.8.7 → v1.8.8.

---

## [1.8.7] — 2026-03-19

Hotfix: GATE 2 was bypassed by Conditional Grounding and STRATEGY_FILES_FOUND exceptions.

### Fixed

- **Root cause — Conditional Grounding exception collapsed all gates (CURSOR.md):** The spec said "skip the grounding question and proceed directly to Paths" with a bridge line "Your frame is clear — going straight to paths." Cursor models read this as licence to run R1+R2+R3 in one pass when context was rich. The exception only ever intended to collapse the R1→R2 gate (grounding question skipped), NOT the R2→R3 gate (path choice still required). Fixed by rewriting the Conditional Grounding section: "This collapses the R1+R2 gate only — GATE 2 still requires a user reply."
- **STRATEGY_FILES_FOUND exception same vulnerability (CURSOR.md Critical Output Rules):** "Proceed immediately to Paths" had no GATE 2 re-assertion at the exception site. Added inline: "⛔ GATE 2 still applies: after Paths + 1/2/3 block, stop. Wait for user reply."
- **Self-check 2 reinforced for exception paths:** Added explicit note: "This self-check applies even when Conditional Grounding or STRATEGY_FILES_FOUND collapsed R1+R2 — the R2→R3 gate is never skipped by those exceptions."
- **Critical Output Rules version tag updated:** v1.8.3 → v1.8.7.

---

## [1.8.6] — 2026-03-19

Patch: pre-commitment challenge tools renamed 1/2/3 (was D/E/F); explicit skip path added to CTA.

### Fixed

- **Pre-commitment challenge block relabeled 1/2/3 (both files):** D/E/F labels on the pre-commitment challenge block were logically after C in the alphabet, implying post-commitment sequence. Renamed to 1/2/3 throughout — numbers carry no sequence implication relative to A/B/C. Post-verdict D–M menu is unaffected.
- **Explicit skip path added to CTA:** "Reply A, B, or C" did not make clear that skipping the challenge block was an option. CTA now reads: "Reply 1, 2, or 3 to dig deeper — or A, B, or C to commit now." Both branches visible on one line.
- **Layout rule annotation updated:** explains why numbers are used instead of letters.
- **All pre-commitment references updated (CURSOR.md):** Critical Output Rules preamble, self-check assertions, structural rules, example template, pre-path challenge rules, inference rule label.
- **All pre-commitment references updated (SKILL.md):** Main spec block, AskUserQuestion unavailable fallback, pre-path challenge rules, 3) Reality check inference rule, "Note on Reality check — two contexts" updated to clarify 3) pre-path vs F) post-verdict.
- **Stale example templates fixed (both files):** Secondary full-example code blocks still had old D/E/F labels — updated to 1/2/3.

---

## [1.8.5] — 2026-03-19

Patch: challenge block UX fix — D/E/F reordered above commit prompt, call-to-action updated.

### Fixed

- **Challenge block reordered above commit prompt (CURSOR.md + SKILL.md):** The D/E/F pre-commitment challenge block was appended AFTER the "Reply A, B, or C" prompt, making D/E/F look like post-commitment options. Fixed by moving the challenge block FIRST, with the commit prompt incorporated at the bottom of the same block. Layout rule added: "The challenge block (D/E/F) always appears BEFORE the commit prompt — never appended after it."
- **Call-to-action updated to include D/E/F explicitly (both files):** "Reply A, B, or C — or correct the Frame if it's off" gave no affordance for D/E/F. Replaced with: "Reply A, B, or C to commit — or D/E/F to dig deeper first. Correct the Frame if it's off."
- **Header renamed to remove ambiguity (both files):** "Want to dig deeper before committing?" read as an optional addendum. Replaced with "Not ready to commit? Dig deeper first:" which signals pre-commitment intent clearly and positions D/E/F as tools to reach commit confidence, not post-commit analysis.
- **SKILL.md AskUserQuestion unavailable fallback updated:** Same three fixes applied to the plain-text fallback path.

---

## [1.8.4] — 2026-03-19

Patch: remaining panel items — confidence key definition inline, self-check assertions at both gates.

### Fixed

- **Confidence key definition now inline in Critical Output Rules (CURSOR.md):** The bullet previously said "output the one-sentence definition" without specifying what that definition is. A model reading only the Critical Output Rules section would know to output something but not what. Added the full definition inline: *High = stake material decisions on this without additional data · Medium = directionally right, one named assumption could invert · Low = too thin to have conviction, treat as directional only.*
- **Self-check assertions added at both gates (CURSOR.md Structural rules):** Added positive-framing gate checks co-located with the Response 1 and Response 2 formatting rules. Gate 1: *"Has the user replied with a grounding choice in this same message? If no → end here."* Gate 2: *"Has the user replied with a path choice or challenge pick in this same message? If no → end here."* These complement the architectural ONE RESPONSE PER TURN rule with active decision-point checks rather than only prohibition language.

---

## [1.8.3] — 2026-03-19

Patch: Cursor still runs through in one pass — architectural fix + H/I menu drop + GATE 2 misplacement.

### Fixed

- **ONE RESPONSE PER TURN rule (CURSOR.md Critical Output Rules + Structural rules) — architectural fix:** ⛔ GATE markers from v1.8.2 proved insufficient — Cursor's agentic model treats the entire `/cpo` invocation as a single task to complete and ignores inline stop signals. Root cause: the spec described three response sections, but the model read it as one continuous output. Fix: the enforcement rule is now framed architecturally — each user turn triggers exactly one response phase. The model's job for an initial `/cpo` call IS Response 1 and nothing more. It cannot answer a question it just asked. Rule added both to Critical Output Rules (read first) and to Structural rules (co-located with the formatting spec).
- **GATE 2 misplacement fixed (CURSOR.md):** In v1.8.2, the GATE 2 marker was placed between the path-selection prompt and the "append challenge block" instruction, causing the challenge block (D/E/F) to never render — the model stopped before reaching it. Moved to after the closing of the challenge block.
- **H and I dropped from post-verdict menu (CURSOR.md progressive disclosure):** The `→ More:` collapsed line was inside the `── Communicate upwards ──` group block. Models were reading "Communicate upwards = G + More" and then rendering J/K/L separately as Move it forward — but attributing H/I to the More line already "handled" under Communicate upwards, then dropping them. Fix: the More line now sits OUTSIDE any group header, with explicit rule: "Do not drop H or I."
- **Version tag corrected:** Critical Output Rules header updated from v1.8.1 → v1.8.3.

---

## [1.8.2] — 2026-03-19

Patch: Cursor agentic loop runs through without stopping + progressive disclosure rendering bug.

### Fixed

- **⛔ STOP gates added (CURSOR.md) — critical:** Cursor's agentic mode auto-continues through response boundaries. Added explicit GATE markers at both user-input boundaries: Gate 1 after Response 1 (grounding question), Gate 2 after Response 2 (path selection). Soft language like "generate paths only after the user confirms" was not enforced — replaced with hard stop markers that explicitly prohibit auto-selection and auto-continuation.
- **THREE HARD STOPS rule added to Critical Output Rules (CURSOR.md):** Read-first section now opens with the three gates enumerated explicitly — grounding gate, path gate, D–M pick gate — with named prohibition ("Do not auto-select"). Models reading the preamble see this before any response.
- **Progressive disclosure code block fix (CURSOR.md):** The first-time user menu rule was a single compressed inline string. Models were misreading it and rendering K) Eng brief as a standalone top-level bullet alongside D–G (H, I, J, L dropped entirely). Expanded into a proper fenced code block with exact output format and explicit rule: the `→ More:` line is a single collapsed summary — do not render H, I, J, K, L as separate items.

---

## [1.8.1] — 2026-03-19

Patch: six panel-identified bugs and recommendations fixed before prod.

### Fixed

- **CURSOR.md Critical Output Rules stale L) reference:** `New evidence L) routing` corrected to `M) routing`. New evidence was moved to M in v1.7.0; read-first section had never been updated — live production bug.
- **`→ eng-brief` removed from L) Hand off sub-menu (SKILL.md + CURSOR.md):** K) Eng brief is now first-class non-repeatable. Listing `→ eng-brief` in the Hand off sub-menu created a bypass of the non-repeatable gate — removed from both files.
- **AskUserQuestion group header rendering spec added (SKILL.md):** AskUserQuestion overlays in Claude Code render options as a flat list — `── Group ──` separator lines are dropped. New rule: prepend group name as `[Group name] →` prefix to the first option of each group inside AskUserQuestion. Plain-text fallback continues to use full `── Group ──` separators.
- **Group separator enforcement reminder added (CURSOR.md):** Critical Output Rules now explicitly states `── Group ──` separator lines are structural, not cosmetic, and must not be omitted or collapsed into a flat list.
- **M) suppression now includes founder guidance:** When M) is suppressed (Medium/Low confidence), the rule now directs founders to the `→ To reach` elevation block as the data intake mechanism with explicit redirect copy.
- **Pre-path challenge loop capped at 2 rounds (SKILL.md + CURSOR.md):** Previously unbounded. After 2 challenge rounds, D/E/F are removed and a forced path-selection prompt fires: *"You've stress-tested from two angles. Pick a path — or tell me what's still unresolved."*

### Meta

- **Critical Output Rules versioned:** Header now reads `## ⚠️ Critical Output Rules — v1.8.0` so future editors know to bump version on next release.

---

## [1.8.0] — 2026-03-19

Menu grouping redesign and Eng brief promotion. Three semantic groups replace the v1.7.0 tier labels. Reviewed and approved by panel (Gary Tan, Mike Krieger, Boris Cherny).

### Changed

- **Post-verdict menu groups renamed and restructured:** Three groups with orientation headers replace the v1.7.0 tier names. Headers are visual labels — letters remain directly typeable, no two-step interaction required.
  - **Analyze further (D–F):** D) Stress test · E) Deep analysis · F) Reality check
  - **Communicate upwards (G–I):** G) Sell-up · H) Board simulation · I) Investor simulation
  - **Move it forward (J–L):** J) Roadmap · K) Eng brief *(promoted from sub-option)* · L) Hand off · [M) New evidence *(ungrouped, conditional)*]

- **K) Eng brief promoted to first-class letter:** Previously buried inside the K) Hand off sub-menu as `→ eng-brief`. Now a standalone pick at K — triggers `/cpo --mode eng-brief` inline, translating the decision for engineering with constraints, success criteria, and implementation risks. K is non-repeatable (one-shot translation per decision).

- **L) Hand off (was K):** Shifted from K to L. Behavior unchanged — silent discovery + context-aware sub-menu. L is repeatable.

- **L) Something else removed:** Three semantic groups provide full coverage; the escape-hatch letter removed. Free-text input still works for anything not covered by D–L.

- **Non-repeatable/repeatable picks updated:** Non-repeatable: D, E, F, G, J, K (was D, E, F, G, J, L). Repeatable: H, I, L, M (was H, I, K, M). Re-surfacing always-retain letters updated from H/I/K to H/I/L.

- **Pre-path challenge block header updated:** `Want to dig deeper before committing?` → `── Validate before committing ──` in Response 2, creating consistent visual language with the post-verdict group headers across the full flow.

- **Progressive disclosure updated:** "More →" line now shows `K) Eng brief · L) Hand off` instead of `K) Hand off · L) Something else`. Group headers in "More" preview updated to Analyze further / Communicate upwards.

- **`--quick` format line updated:** K) Eng brief, L) Hand off, Something else removed.

- **If user picks K handler added:** Mirrors H/I pattern — runs eng-brief inline, then re-surfaces D–M menu.

- **If user picks L handler renamed:** K → L throughout (Hand off handler, discovery block, "L is repeatable" note).

- **D–L references updated to D–M** in structural rules, final check rule, boardroom/investor transcript prompts.

---

## [1.7.0] — 2026-03-19

Three systemic flow fixes identified from live output screenshots — path selection gate, confidence-elevation loop gating, and post-verdict menu restructure. All three approved by panel (Gary Tan, Mike Krieger, Boris Cherny).

### Fixed

- **Path selection gate (self-check assertion):** Model was rendering the Verdict without the user explicitly picking a path. New self-check assertion `path_was_selected_by_user must be true before Verdict renders` enforces the gate in both SKILL.md and CURSOR.md. Exception: `--go` and `--quick` bypass by design (auto-select recommended path). CURSOR.md trace checkpoint updated to include `path_was_selected_by_user` in verdict step.

- **Confidence-elevation loop gating:** When confidence was Medium/Low, the full D-L next-steps menu was rendering simultaneously with the elevation prompt — allowing users to skip gap resolution and proceed without grounding the decision. Fix: suppress the entire D-M menu when the elevation block fires. Menu unlocks only after (a) user provides the missing input and confidence re-evaluates, OR (b) user explicitly replies "skip". New self-check assertion `elevation_loop_active true → D-M menu suppressed` added to both files. Loop exit conditions updated from "proceed with D-L menu" to "render D-M menu".

### Changed

- **Post-verdict menu restructured from D-L to D-M with 3 semantic tiers:**
  - **Validate:** D) Stress test · E) Deep analysis · F) Reality check *(post-verdict: reacts to chosen path/verdict, not all three paths)*
  - **Communicate:** G) Sell-up · H) Board simulation · I) Investor simulation
  - **Execute:** J) Roadmap *(moved from F)* · K) Hand off *(moved from J)* · L) Something else *(moved from K)* · [M) New evidence *(moved from L)*]
  - Tier headers (`── Validate ──`, `── Communicate ──`, `── Execute ──`) added to all menu instances
  - Progressive disclosure updated: first-time users see D–G + "More →" *(now covers full Validate tier + first Communicate option)*

- **Post-verdict F) Reality check:** New variant added — reacts to the chosen path and current verdict (commitment validator), not all three paths (comparison tool). Pre-path F) Reality check in Response 2 is unchanged. Documented in spec: "two contexts, same letter" to prevent future editor confusion. Handler spec added for post-verdict F).

- **Non-repeatable/repeatable picks updated:** Non-repeatable: D, E, F, G, J, L (was D, E, F, G, K). Repeatable: H, I, K, M (was H, I, J). Re-surfacing always-retain letters updated from H/I/J to H/I/K.

- **Letter renames throughout (both SKILL.md and CURSOR.md):**
  - F) Roadmap → J) Roadmap
  - J) Hand off → K) Hand off
  - K) Something else → L) Something else
  - L) New evidence → M) New evidence
  - "If user picks K" → "If user picks L" (Something else)
  - "If user picks J (Hand off)" → "If user picks K (Hand off)"
  - "J is repeatable" → "K is repeatable"
  - "If user picks L)" → "If user picks M)"
  - Simulation-informed RECOMMENDATION: "recommend F: Roadmap" → "recommend J: Roadmap"

- **`_SKILL_VERSION`:** 1.5.0 → 1.7.0 in both SKILL.md and CURSOR.md *(bumped at session start before all edits)*.

---

## [1.5.0] — 2026-03-19

Architecture optimization — lazy-loading expanded. Three large inline sections moved to reference files with graceful fallbacks; two more compressed to stubs in-place. SKILL.md and CURSOR.md shrunk significantly without removing any behavior. All self-check enforcement gates kept inline.

### Added

- **`references/flags/decide.md`** — Full `--decide` spec extracted here (Skill Handoff Contract, Discovery bash, Decision Logic, Output Format × 3, Install Source Map, Rules).
- **`references/internal/contributor.md`** — Full Contributor Mode spec extracted here (bash block, field report template, fill instructions, max-3-per-session rule).
- **`references/internal/trace.md`** — Execution Trace checkpoint table + trace file init extracted here. Self-check assertions kept inline in SKILL.md (enforcement gates must not be lazy-loaded).

### Changed

- **SKILL.md `--decide` section:** 133-line full spec → 3-line stub with `Read references/flags/decide.md` + fallback instruction.
- **SKILL.md Contributor Mode:** 35-line full spec → 2-line stub with `Read references/internal/contributor.md` + silent-skip fallback.
- **SKILL.md Execution Trace:** 35-line full spec → 12-line compact version (checkpoint table → reference file; self-check assertions retained inline).
- **SKILL.md `--patterns` section:** 48-line full spec → 4-line stub with `Read references/flags/patterns.md` (reference file already existed; inline content redundant).
- **CURSOR.md `--decide` section:** 78-line full spec → 4-line stub with `cat ~/.claude/skills/cpo/references/flags/decide.md` + fallback instruction.
- **`_SKILL_VERSION`:** 1.4.9 → 1.5.0 in both SKILL.md and CURSOR.md.

---

## [1.4.9] — 2026-03-19

Universal terminal rule — no CPO response ends without a user action prompt. Every flow endpoint (main Response 3, elevation mini-flow, simulations, utility flags, execution-artifact modes) now has an explicit next-step prompt. Boardroom and investor-roundtable standalone invocations get a "what next?" line after transcript save. Final check rule added to structural rules in both SKILL.md and CURSOR.md: model must verify the last element is an action prompt before delivering any response. Label change: "CEO/board" → "c-suite" in G) Leadership reaction across all files.

### Added

- **Universal terminal rule:** Every response endpoint — main flow, elevation mini-flow, inline simulation (H/I picks), standalone boardroom/investor-roundtable, utility flags (`--brief`, `--trail`, `--history`, `--outcome`, `--patterns`, `--drift`), execution-artifact modes (eng-brief, eng-translate) — must end with either the D-L menu (decision flows) or a contextual next-step prompt (utility/intelligence flows).
- **Final check enforcement anchor:** Structural rules now include a pre-delivery check: "verify the last substantive element is a user action prompt; if not, append the appropriate prompt before delivering." Fires on every response in every mode without exception.
- **Elevation mini-flow step 6 (SKILL.md):** Explicit step 6 added: "Render the D-L next-steps menu — same AskUserQuestion format and rules as Response 3." Previously the elevation success path had no D-L instruction.
- **Boardroom post-transcript prompt:** After "Transcript saved," outputs "What next? Type a new decision, run `/cpo [topic]` to revisit anything the board raised, or pick from your remaining D–L options if this was an H) pick."
- **Investor-roundtable post-transcript prompt:** Same pattern — "What next? Type a new decision, run `/cpo [topic]` to revisit anything investors challenged, or pick from your remaining D–L options if this was an I) pick."

### Changed

- **G) Leadership reaction label:** "how does each path land with your CEO/board?" → "how does each path land with your c-suite?" in SKILL.md, CURSOR.md, and CHANGELOG.md.
- **`_SKILL_VERSION`:** 1.4.8 → 1.4.9 in both SKILL.md and CURSOR.md.

---

## [1.4.8] — 2026-03-19

G) Leadership reaction added to the pre-path challenge block. Asks "how does each path land with your c-suite?" before committing — frames all three paths for the target audience, not just the recommended one. Distinct from the post-verdict G) sell-up, which packages a committed decision.

### Added

- **G) Leadership reaction in pre-path challenge block:** Surfaces in Response 2 alongside D/E/H/I. Frames all three paths for the specified audience (CEO, board, eng-lead). Pre-path framing: "which path lands with leadership?" Post-verdict G) framing: "how do I pitch this committed decision?" — different questions, same letter.

### Changed

- **`_SKILL_VERSION`:** 1.4.7 → 1.4.8 in both SKILL.md and CURSOR.md.

---

## [1.4.7] — 2026-03-19

Pre-path challenge menu. D/E/H/I challenge options (Pre-mortem, Deep dive, Board simulation, Investor simulation) now surface at the path-selection stage (Response 2), before the user commits to a path — not only after the Verdict. Challenge runs against all three paths, not just the recommended one. Loop has no fixed limit. Suppressed by `--go` and `--quick`.

### Added

- **Pre-path challenge block:** After the path-selection prompt in Response 2, a plain-text challenge block renders: D) Pre-mortem · E) Deep dive · H) Board simulation · I) Investor simulation. The user can challenge any number of times before picking a path.
- **Pre-path challenge rules spec:** Challenge options run against all three paths; after a challenge completes, path-selection is re-surfaced; journal write still happens after the Verdict in Action 4 (unchanged).

### Changed

- **`_SKILL_VERSION`:** bumped to `"1.4.7"` in both SKILL.md and CURSOR.md (also corrects stale version strings that lagged behind v1.4.5/1.4.6).
- **Structural rules:** Response 2 rule updated — no longer "ends with `Reply A, B, or C`" but "ends with path-selection prompt + challenge block."
- **`--go` and `--quick`:** Explicitly suppress the challenge block entirely. Documented in both structural rules and pre-path challenge rules.
- **`--deep`:** Challenge block still renders after the path-selection AskUserQuestion when `--deep` is active.

---

## [1.4.6] — 2026-03-18

Bulk journal management. `--invalidate-all` marks all active entries as invalidated in one operation. Optional `#name` filter to scope to a single decision object. Optional `--hard` modifier for permanent file deletion (irreversible).

### Added

- **`--invalidate-all`:** Bulk invalidation — marks all active journal entries as invalidated with a single YES-confirmed command. Optional `#name` filter to scope to one decision object. `--hard` variant permanently deletes YAML files from disk. Always shows entry count before confirmation; reason optional (defaults to "bulk invalidation").

### Changed

- **Flag count:** 26 → 27.
- **`references/flags/invalidate.md`:** Added `--invalidate-all` section with full spec, steps, and rules.

---

## [1.4.5] — 2026-03-18

Journal override and logic drift detection. Three additions: `--invalidate` to retire superseded decisions, passive drift surface on journal writes, and `--drift` for on-demand structural contradiction detection.

### Added

- **`--invalidate [topic or #id]`:** Mark a past decision as intentionally retired. Annotates the YAML entry with `status: invalidated`, `invalidated_date`, and `invalidated_reason`. Future context loads (`DECISIONS_FOUND`) skip invalidated entries silently; `--history` always shows them with `[invalidated]` marker for audit. Never automatic — always user-initiated with a reason.
- **Passive drift surface:** When writing any journal entry, CPO now also checks whether the incoming entry's Dominant Truth fingerprint differs from the most recent non-invalidated entry on the same topic. If it does, one non-blocking note is appended: *"Note: Dominant Truth shifted from [prior] → [current] — intentional? Run `--drift` or `--invalidate [topic]`."* Does not interrupt the flow.
- **`--drift` flag:** On-demand logic drift detection. Scans last 10 non-invalidated journal entries, surfaces only structural contradictions: Truth fingerprint shifts across ≥3 consecutive entries without `delta_from_prior`, verdict reversals without acknowledgment, or kill criteria degradation. Keyword similarity alone does not trigger drift — fingerprint-based only.
- **`references/flags/invalidate.md`:** Full `--invalidate` spec with step-by-step execution, rules, and per-flag behavior table.
- **`references/flags/drift.md`:** Full `--drift` spec with structural drift definition, what does/doesn't count, bash execution block, and output format.

### Changed

- **Journal schema bumped to `"1.5"`:** Three new fields added to every journal entry: `status: active`, `invalidated_date: ""`, `invalidated_reason: ""`.
- **`DECISIONS_FOUND` context load:** Silently skips entries with `status: invalidated`. Full history (including invalidated) always available via `--history`.
- **Progressive disclosure nit (from panel review):** Clarified that the first-time user condition is "no prior journal entries *ever*" — not "no entries in current session." Prevents experienced founders starting a fresh session from seeing the compressed menu.
- **`--patterns` minimum threshold note:** Added inline note that 3 entries is the technical gate; meaningful patterns emerge at 8–10 entries.

---

## [1.4.4] — 2026-03-18

8 improvements: Truth Fingerprint tracking, `--decide` version guard, `--stack` CPO-aware detection, kill criterion in `--decide` outputs, `--patterns` flag (Decision DNA), `--quick` mode, `references/handoff-contract.md`, progressive D–L menu disclosure for first-time users.

### Added

- **Truth Fingerprint:** Response 3 now renders `**Truth fingerprint:** Dominant · Grounded · Inferred` after blind spots. Written to journal as `truth_fingerprint:` field. Journal schema bumped to `schema_version: "1.4"`.
- **`--decide` version guard:** If CPO receives a `CPO Handoff Request` block but `--decide` is not in its flag list (older version), outputs a graceful fallback recommendation instead of silently ignoring the handoff.
- **`--stack` CPO-aware indicator:** Skills that implement the `--decide` handoff contract (contain `CPO Handoff Request` or reference `/cpo --decide`) are now marked `[✓ CPO-aware]` in stack output. Detection via grep scan.
- **Kill criterion in `--decide` outputs:** High-stakes handoffs (critical bugs, production issues, irreversible actions, revenue impact) now append a `Watch for:` line with one measurable threshold before the hand-off confirmation.
- **`--patterns` flag:** New Decision DNA analysis — scans decision journal for Truth weighting bias, path preference, kill criteria hit rate, reversal rate, and confidence calibration. Outputs plain-language pattern summary with one behavioral "Watch for."
- **`--quick` mode:** Single-response condensed output — all four actions in one response, no grounding question, one kill criterion, no blind spots, no Truth fingerprint. Low-confidence warning appended when needed.
- **`references/handoff-contract.md`:** Full Skill Handoff Contract documentation for skill authors — format spec, field definitions, anti-patterns, checklist.
- **Progressive D–L menu disclosure:** First-time users (preamble returned `NO_DECISIONS`) see condensed D–G menu with `More →` inline. Returning users see full D–L menu.

---

## [1.4.1] — 2026-03-18

Second panel review pass (Gary Tan · Mike Krieger · Boris Cherny). 7 bug fixes, 2 new features, Bold/Balanced/Conservative contamination cleanup.

### Fixed — spec bugs (both files unless noted)

**Elevation exit contradiction:** Line 341 said "(I) renders since elevation exited)" which contradicted the confidence-based rule. Removed the parenthetical — confidence is now the sole source of truth for I) rendering. I) rendering follows the confidence rule (High only) regardless of how elevation exits.

**Journal template keys:** `bold/balanced/conservative` keys contradicted the "Never use Bold/Balanced/Conservative" rule. Changed to `path_a/path_b/path_c`.

**Trace field:** `d_h_menu_blocked_until` → `d_i_menu_blocked_until` across both files.

**Flag loading fallback (CURSOR.md):** Added fallback rule for `.cursorrules` flag `cat` failures — proceed from stub description, same as mode loading fallback.

**SKILL.md elevation exit wording drift:** SKILL.md used a different formulation than CURSOR.md for the same elevation exit rule. Matched CURSOR.md's exact wording.

**Bold/Balanced/Conservative contamination (SKILL.md — 7 instances):** Four authoritative sections (Action summary box, Help text, Operating Principles, Decision Flow) still said "Bold · Balanced · Conservative" as path labels — directly contradicting Action 3's explicit ban. All replaced with "Three situational paths" / "situational labels." Three additional stale references in grounding quality bar ("those come next"), grounding option bar ("already handled by Bold/Balanced/Conservative"), and `--outcome` flag description also fixed. Zero B/B/C-positive references remain.

**`--go` flag table parity (SKILL.md):** Missing "Also skips the `STRATEGY_FILES_FOUND` confirmation gate" clause. Added to match CURSOR.md.

### Added — Five Truths persistence

After completing Assess (when a decision object exists via `#name`, or when `--deep` is passed), writes a Five Truths snapshot to `~/.cpo/.scratch/[slug]-truths.md`. Users can reference it in multi-turn conversations. After the Verdict in Response 3, one line: *"Five Truths saved to `~/.cpo/.scratch/[slug]-truths.md` — reference anytime."*

### Added — Next-steps re-surfacing with RECOMMENDATION

After completing any D–I pick, the remaining unused picks are re-offered with a `RECOMMENDATION:` line naming the single most valuable next pick given what just happened. Initial menu (first D–I after Verdict) has no RECOMMENDATION; all re-surfaced menus include one. Loop continues until picks are exhausted or user skips/starts a new decision. AskUserQuestion format (SKILL.md) with plain text fallback.

### Changed — "Data loop" → "New evidence"

Option I) renamed from "Data loop" to "New evidence" across both files — enforcement TL;DR, menu instances, routing sections, and all references. Conditionality explanation added: I) renders only when confidence is High.

### Changed — Menu letter range D–H → D–I

All D–H references updated to D–I across both files, including kill criteria gate, elevation exits, trace fields, enforcement blocks, and inline letter references.

### Updated — README.md

- Three Paths section: Bold/Balanced/Conservative table replaced with situational label description and examples
- All 4 example outputs updated from B/B/C labels to situational labels
- Version bumped to v1.4.1

---

## [1.4.0] — 2026-03-18

Strategy enrichment extended across the base flow and key flags. Situational path labels. Flag combination rules formalized.

### Changed — Base flow `STRATEGY_FILES_FOUND` handler

The base `/cpo [question]` flow now runs a **lightweight tension check** in addition to posture synthesis when strategy files are found. After reading and synthesizing the files, it scans for ↓ conflicts that change the success metric, primary user, or time horizon. Any found are surfaced in the same confirmation block before the user responds. One gate covers both posture and tensions. This is the lightweight version — no full ↑/↓/○ cross-reference output; use `--scan-strategy [question]` for the full explicit treatment.

`--go` + `STRATEGY_FILES_FOUND` updated: tension check runs silently; if a question-reframing conflict is found, one inline note is added; no reframe gate fires.

### Changed — Action 3 path labels (global)

**Bold/Balanced/Conservative removed as default path labels.** Path labels are now situational — verb phrases derived from the confirmed grounding frame. Labels name what each path commits to, optimizes for, or defers — not the risk appetite. Derivation rules:
- For conflict tensions: *"Resolve toward [X] / Resolve toward [Y] / Defer [conflict]"*
- For sequencing/scope decisions: *"Concentrate on [focus] / Sequence by [dependency] / Time-box [decision]"*
- For any frame: labels must answer "what does this path bet on?" not "how risky is it?"

Self-check added to Action 3 spec: if replacing labels with Bold/Balanced/Conservative still makes sense → rewrite.

Structural rule updated. Response 2 template updated. Converging paths edge case updated.

### Changed — `--since` flag

Temporal cross-reference format made explicit. When `PRIOR_ENTRY_FOUND`, output a structured ↑ Reinforces / ↓ Challenges / ○ Still open block before the Frame. Reframe check added: if ↓ Challenges row changes success metric, primary user, or time horizon — fire one reframe check line and wait. One reframe check maximum per invocation.

### Changed — `--roadmap` flag

Strategy alignment check added (runs before Step 1 when strategy context was loaded from preamble). Cross-references the bet collection against strategy docs in ↑ Aligned / ↓ Conflicts / ○ Silent format. Reframe check: if a conflict changes the prioritization criteria — fire one reframe check and wait. When combined with `--scan-strategy`, the explicit scan-strategy cross-reference replaces the alignment check (no double output).

### Changed — `--sell-up` flag

Strategy alignment check added (runs before Step 1 when strategy context was loaded). Cross-references the pitch recommendation against strategy docs. ↓ contradictions surfaced as a warning note — not a gate. Sell-up always proceeds; the user decides how to handle conflicts.

### Added — `references/flags/combinations.md`

New reference file documenting valid and invalid flag combinations, global stacking rules, and per-combination behavior. Referenced from new SKILL.md stub: "Flag Combination Rules."

**Valid combinations:** `--scan-strategy + --since`, `--scan-strategy + --roadmap`, `--scan-strategy + --go`, `--since + --go`, `--scan-strategy + --sell-up`

**Invalid combinations:** Any `--scan-strategy`/`--since` with `--brief`, `--trail`, `--history`, `--outcome` — no question to cross-reference against.

**Global rules:** one reframe check per invocation; `--go` suppresses all reframe gates; enrichment is additive (strategy docs first, temporal delta second); reframe check always fires after all enrichment, before Frame.

### Fixed — three panel-identified gaps (post-review patch)

**combinations.md enrichment order ambiguity:** Global rules now explicitly state the fixed enrichment order — `--scan-strategy → --since → --roadmap`. Previously said "first one encountered" without defining what that order is.

**SKILL.md single-read clarification:** STRATEGY_FILES_FOUND lightweight tension check now explicitly marked "while reading for posture synthesis (not a separate read)" to prevent models from doing two sequential file reads.

**CURSOR.md v1.4.0 parity (full):** All v1.4.0 behavioral changes now present inline in CURSOR.md (Cursor cannot defer to external reference files):
- STRATEGY_FILES_FOUND handler: posture synthesis + lightweight tension check + `--go` exception
- Action 3 paths template: situational labels, derivation rules, self-check, worked example
- Response 2 template: updated to situational labels
- Structural rules: Bold/Balanced/Conservative removed, situational label rule added
- Converging paths edge case: updated
- Decision Flow diagram: updated
- Operating Principles: updated
- Help text (`cpo ?`): updated
- `--since` section: full temporal cross-reference format + reframe gate
- `--roadmap` section: strategy alignment check + reframe gate + note on --scan-strategy combination
- `--sell-up` section: strategy alignment check (warning not gate) + explicit skip if no strategy context
- New "Flag Combination Rules" section inline (enrichment order, valid/invalid combinations, global rules)

---

## [1.3.0] — 2026-03-18

`--scan-strategy` redesign, version drift fixes, and CURSOR.md parity.

### Changed — `--scan-strategy` (two-path design)

`--scan-strategy` now has two distinct execution paths:

- **Path A (standalone):** `/cpo --scan-strategy` with no question — unchanged behavior. Runs the filename heuristic scan, reads up to 5 strategy files, outputs a refreshed posture summary. Ends with "Update complete."
- **Path B (with question):** `/cpo --scan-strategy [question]` — new behavior. Scans and reads strategy files, cross-references them against the specific question (`↑ Supports / ↓ Conflicts / ○ Silent`), then runs the normal four-action flow. Grounding options (Action 1) are strategy-anchored — built from tensions and alignments found in the docs, not generic Bold/Balanced/Conservative angles.

Path B replaces the standard `STRATEGY_FILES_FOUND` posture confirmation — cross-reference IS the strategy context, one output not two stacked. Post-flow confirmation cites which files were used.

SKILL.md routing list exception, flags table description, and stub section updated. `references/flags/scan-strategy.md` fully rewritten with complete two-path spec including the strategy-anchored grounding quality rule and wrong/right examples.

### Changed — CURSOR.md parity

CURSOR.md updated to v1.3.0: `_SKILL_VERSION` bumped, routing list and flags table updated, `--scan-strategy` section fully rewritten inline with complete Path A and Path B spec (Cursor does not defer to external files — spec lives inline).

### Fixed — version drift

SKILL.md frontmatter `version` and preamble bash `_SKILL_VERSION` were both `1.0.0` despite the skill being at v1.2.0 editorially. Bumped to `1.3.0`. `last_updated` corrected from `2026-03-17` to `2026-03-18`. CURSOR.md `_SKILL_VERSION` similarly corrected from `1.0.0` to `1.3.0`.

### Added — `references/flags/export.md`, `references/flags/save-context.md`

`--export` and `--save-context` flag behavior externalized from SKILL.md into dedicated reference files, matching the externalization pattern established in v1.2.0. Flag count in `references/flags/` increases from 12 to 14.

### Size

- v1.3.0: ~10,492 words / 1,109 lines
- Reduction from v1.1.0 baseline (16,039 words): ~34.6%

---

## [1.2.0] — 2026-03-18

Architectural refactor: externalize all heavy flag behavior sections from SKILL.md into `references/flags/` files. Replaces inline content with 2-line stubs + Load instructions. No behavioral changes — all specification content preserved in the external files.

### Changed — SKILL.md

**Flag sections externalized (12 total):** `--brief`, `--trail`, `--history`, `--since`, `--outcome`, `--schedule-brief`, `--setup-integrations`, `--import-context`, `--scan-strategy`, `--roadmap`, `--sell-up`, Stack Detection & Workflow Handoffs

Each section reduced to:
```
## `--[flag]` Flag Behavior
**Trigger:** [description]
**Load:** `Read references/flags/[flag].md` — follow all instructions there.
```

**References section updated** to include `references/flags/[flag].md` in the full file list.

### Added — `references/flags/` directory

12 new files containing the full specification for each externalized flag. Content is identical to what was inline — no behavioral changes.

### Size reduction

- Before: 16,039 words / 1,768 lines (~20K tokens, ~10% of 200K context window)
- After: ~11,400 words / 1,208 lines (~14K tokens, ~7% of 200K context window)
- Reduction: ~29% (~4,600 words). Flags pay context cost only when invoked.

**Motivation:** Panel review identified "lost in the middle" attention degradation on a 1,768-line document — behavioral rules in the middle sections received lower attention weight. The mode stubs pattern (already used for 20 modes) applied to flags.

---

## [1.1.0] — 2026-03-18

Spec hardening pass following three-panel review (Gary Tan · Mike Krieger · Boris Cherny). All changes are behavioral fixes or parity gaps — no feature additions.

### Fixed — SKILL.md

**P0 — `--go` + `STRATEGY_FILES_FOUND` gate conflict**
- `--go` promises no gates and no `AskUserQuestion` calls, but `STRATEGY_FILES_FOUND` used plain-prose "Wait for confirmation" that was not covered by `--go`'s existing escape hatch
- Added explicit exception: when `--go` is present, skip the `STRATEGY_FILES_FOUND` confirmation gate entirely, incorporate strategy synthesis silently into the Frame, and add one hedged inline line

**P1 — Structural rule contradiction**
- "Response 1 Line 1 always starts with `*I'm reading this as:`" clashed with the `STRATEGY_FILES_FOUND` exception that precedes it
- Updated structural rule to carry the exception inline

**P1 — `DECISION_OBJECT_LOADED` grounding behavior unspecified**
- Spec was silent on whether the grounding `AskUserQuestion` still fires for returning decisions
- Added explicit rule: the delta frame IS the grounding for returning decisions — no grounding `AskUserQuestion` fires in Response 1

**P1 — `NO_STRATEGY_FILES` offer timing collision**
- Spec said "after the first full response" but Response 1 already ends with a grounding `AskUserQuestion`, creating a collision
- Changed to "after the full three-response flow is complete (after Response 3 is delivered)"

**P1 — Confidence key missing from Response 3 template**
- Prose spec said "confidence key on very next line" but the Response 3 template had no slot for it
- Added `*Confidence key: [one-sentence definition]*` line to template

**P1 — `--scan-strategy` not independently executable**
- Spec described the heuristic in prose but gave no bash block — a model had to reconstruct it from the preamble
- Added full executable bash block inline under `--scan-strategy`

**P1 — Elevation loop exits redundant/ambiguous**
- Exits 3 and 4 were "User skips" and "User replies 'skip'" — effectively the same; also referenced a D–H menu not yet shown
- Simplified to 3 unambiguous exit conditions

**P2 — `--go` discoverability**
- New users never saw `--go` in the natural NO_CONTEXT+NO_DECISIONS path
- Added one-line tip at the end of the NO_CONTEXT+NO_DECISIONS handler

### Fixed — CURSOR.md

**P0 — Kill criteria gate missing from execution trace**
- Verdict checkpoint used `kill_criteria_count (populate as integer, 0 if none)` with no MUST assertion and no `d_h_menu_blocked_until` gate
- Updated checkpoint to: `kill_criteria_count (MUST be >= 3)`, `kill_criteria_are_measurable (MUST be true — each criterion must name a metric, a threshold, and a timeframe)`, and `d_h_menu_blocked_until: kill_criteria_count >= 3`

**P1 — Version hardcoding in bootstrap**
- `echo "1.0.0" > ~/.cpo/.version` would write the wrong version after any bump
- Changed to `echo "$_SKILL_VERSION" > ~/.cpo/.version`

**P1 — `--go` + `STRATEGY_FILES_FOUND` parity gap**
- CURSOR.md global flags table did not document the `STRATEGY_FILES_FOUND` gate exception for `--go`
- Added "Also skips the `STRATEGY_FILES_FOUND` confirmation gate" to the `--go` flag row

**P1 — `--scan-strategy` bash block missing**
- CURSOR.md described `--scan-strategy` in prose but omitted the executable bash block
- Added the same bash block as SKILL.md

**P2 — `NO_INTEGRATIONS` offer text truncated**
- CURSOR.md offer line omitted "and enrich Five Truths with real numbers" vs SKILL.md
- Restored full offer text

### Updated — README.md

- Flag count updated: 22 → 24
- Added `--import-context [path]` flag row
- Added `--scan-strategy` flag row
- Added `.claude/strategy/` row to persistent layers table
- Added `.claude/strategy/` to structure diagram
- Added `--schedule-brief` Cursor limitation note: *(Claude Code only — Cursor users see manual reminder setup.)*
- Updated gstack `/plan-ceo-review` handoff description to clarify CPO→recommendation→plan-ceo-review for experience/vision pressure-test
- Version bumped to v1.1.0

---

## [1.0.0] — 2026-03-17

First public release.

### Added

**Core advisor framework**
- 20 modes covering the full product lifecycle: `blue-ocean`, `ceo`, `sequence`, `gtm`, `discovery`, `narrative`, `launch-os`, `investor-story`, `red-team`, `premortem`, `postmortem`, `org-design`, `board-memo`, `board-story`, `eng-brief`, `eng-translate`, `advisory-roundtable`, `boardroom`, `investor-roundtable`, `upward-pitch`
- Five Truths assessment framework (User · Strategic · Economic · Macro-Political · Execution)
- Three Paths on every recommendation (Bold · Balanced · Conservative) with kill criteria
- Evidence labeling on all outputs (*fact / assumption / inference / judgment*)
- Stage-aware doctrine (pre-PMF / post-PMF / Series B+)
- Role detection — decision-maker vs. influencer framing

**Decision journal**
- Persistent YAML journal at `~/.cpo/decisions/` — auto-written after every major analysis
- `--brief` — proactive weekly intelligence brief: kill criteria alerts, unresolved decisions, pattern warnings
- `--trail` — 90-day strategic diary view
- `--history` — full journal with keyword filtering
- `--since` — temporal delta mode: diff current analysis against a prior baseline
- `--outcome` — close the loop on a prior decision, detect Bold/Balanced/Conservative path patterns

**Roadmap & prioritization**
- `--roadmap` — comparative prioritization across N bets: compressed Five Truths per bet, stage-aware scoring (3 dims pre-PMF, 6 dims post-PMF), dependency mapping, bias detection, ASCII output with kill criteria per bet

**Pitch-building**
- `--sell-up [audience]` — reframes any decision as an internal pitch for CEO, board, eng-lead, or custom audience. Produces 1-minute elevator pitch, 5-minute meeting version, objection pre-emption, concession strategy, and explicit ask

**Simulations**
- `boardroom` — live multi-turn board simulation with governance dynamics, per-member sentiment, post-meeting debrief, transcript saved to `~/.cpo/simulations/`
- `investor-roundtable` — live 5-round investor debate, Tier 1 + Tier 2 panel, M&A framing, transcript saved

**Integrations & exports**
- `--setup-integrations` — detect MCP data sources, map to Five Truths, static fallback via `~/.cpo/integrations.md`
- `--export` — write any output to `~/.cpo/exports/` as shareable Markdown
- `--schedule-brief` — set up recurring weekly brief via `anthropic-skills:schedule`

**Stack & discovery**
- `--stack` — show full product workflow with coverage status, detect complementary skills by description keywords
- Workflow handoffs — after every major output, suggest the logical next step (once per session, only if skill is installed)
- First-session discovery — on first invocation with no state, surface `--stack` in one line

**Persistence**
- Company context at `~/.cpo/context.md` — loads automatically each session
- `--save-context` — full bootstrap to collect and save all 5 context fields
- `--context [name]` — named context switching for multiple companies
- Version tracking at `~/.cpo/.version` — surfaces mismatches between skill and saved state

**Cursor edition**
- `CURSOR.md` — full Cursor-compatible version: same bash blocks, same `~/.cpo/` state, `cat`-based mode loading, plain question format replacing `AskUserQuestion` tool
- Shared decision journal between Claude Code and Cursor environments
