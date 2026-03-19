# CPO Skill — Changelog

All notable changes documented here. Follows [Keep a Changelog](https://keepachangelog.com/en/1.0.0/) conventions.

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
