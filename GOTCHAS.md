# CPO Gotchas — Failure Log
<!-- Append-only. Add new entries at bottom. Never delete or edit old ones. -->
<!-- Format: date | symptom | root cause | fix -->

---

### [2025-11-01] Instruction placement — abstract rules at top get ignored
**Symptom:** Model skipped phase gates or generated D-M letters at wrong phase
**Root cause:** v1.9.0 rewrote inline ⛔ STOP markers as abstract rules at top of file. Models pre-loaded rules but ignored them at the moment of decision.
**Fix:** Inline behavioral cues (⛔ markers, self-check questions) must appear at the exact point where the model should stop — not as preamble. Reverted to v1.8.12 pattern.

---

### [2025-11-15] Phase boundary confusion — D-M letters generated pre-verdict
**Symptom:** Response 2 (path selection) used lettered options (D-M) instead of numbered 1/2/3
**Root cause:** Spec was ambiguous — D-M labels appeared in the same section as path selection, model used them early
**Fix:** D-M letters are explicitly post-verdict only. Numbered 1/2/3 for path selection. Added gate assertion in SKILL.md §501–510.

---

### [2025-11-20] Challenge block applied only to recommended path
**Symptom:** Challenge options only critiqued Path A, skipping B and C
**Root cause:** Challenge section was placed immediately after Path A description; model scoped it locally
**Fix:** Challenge block must explicitly state "run against ALL three paths" — not just the recommended one.

---

### [2025-12-01] Grounding/correction path invisible in modal-only displays
**Symptom:** User couldn't find the correction/grounding option; it only appeared in full-page view
**Root cause:** Correction path was a secondary menu item; IDEs with modal-only popups didn't render it
**Fix:** Correction promoted to first-class option alongside the three primary paths.

---

### [2025-12-10] Strategy output = goals + buzzwords, not real choices
**Symptom:** Output included "become the leading AI platform for X" with no trade-offs
**Root cause:** Skipped the "real strategic choices" forcing function (Lesson 6 in product-canon)
**Fix:** Flag any strategy statement that has no explicit trade-off or eliminated option. "Strategy" that works for every company is not strategy.

---

### [2026-01-05] Data moat claimed without feedback loop evidence
**Symptom:** Product positioned as having data moat because "we have a lot of data"
**Root cause:** Confused data storage with compounding data advantage
**Fix:** Require explicit feedback loop: data → model improvement → better product → more data. Storing data ≠ moat. Ask: "Does our model improve automatically as usage grows?"

---

### [2026-01-18] Network effects claimed without network dependency
**Symptom:** "Better with more users" labeled as network effect
**Root cause:** Conflated improvement with dependency. True network effect = product is nearly useless without the network.
**Fix:** Force the test: "If we had 1 user, would the product still function?" If yes, it's not a true network effect.

---

### [2026-02-10] Pre-revenue decisions led with market size, not displacement cost
**Symptom:** CEO mode output opened with TAM/SAM/SOM; no mention of what the user must stop doing
**Root cause:** Default framing defaults to opportunity sizing, which is speculative at pre-revenue stage
**Fix:** Early-stage decisions must lead with: verdict → displacement cost → narrowest wedge. Market size in appendix only.

---

### [2026-03-10] AskUserQuestion not fetched before choice popups
**Symptom:** Silent failure — choice popup rendered nothing in Cursor/IDE environment
**Root cause:** ToolSearch for AskUserQuestion not called before the preamble bash block
**Fix:** ToolSearch `select:AskUserQuestion` is STEP 0 — before bash, before reading context, before any analysis. Zero exceptions.

---
<!-- NEW ENTRIES BELOW THIS LINE -->
