# `--scan-strategy` Flag

**Two paths depending on whether a question is present.**

---

## Path A — Standalone (`/cpo --scan-strategy` with no question)

**What it does:** Re-runs the filename heuristic scan, reads up to 5 files (2,000 tokens each), and outputs a refreshed posture summary. Use this after adding new strategy files or running `--import-context`.

```bash
_STRAT_FILES=$(find . -maxdepth 4 \( -iname "*strategy*.md" -o -iname "*roadmap*.md" -o -iname "*positioning*.md" -o -iname "*vision*.md" -o -iname "DECISIONS.md" -o -iname "*goals*.md" -o -iname "*thesis*.md" \) ! -path "*/node_modules/*" ! -path "*/.git/*" 2>/dev/null | head -5)
_STRAT_SAVED=$(ls -t .claude/strategy/*.md 2>/dev/null | head -3)
if [ -n "$_STRAT_FILES" ] || [ -n "$_STRAT_SAVED" ]; then
  echo "STRATEGY_FILES_FOUND:"
  [ -n "$_STRAT_FILES" ] && echo "$_STRAT_FILES"
  [ -n "$_STRAT_SAVED" ] && echo "STRATEGY_SAVED:" && echo "$_STRAT_SAVED"
else
  echo "NO_STRATEGY_FILES"
fi
```

Then read the listed files (prioritize `.claude/strategy/` first) and synthesize.

**Output:**

> *Strategic context — based on [N] file(s): [synthesis in 2–3 sentences]. Most recent: [filename].*

End with: *"Update complete — I'll use this posture as context for the next analysis."*

If `NO_STRATEGY_FILES`: *"No strategy files found. Add `.md` files with names containing strategy, roadmap, positioning, vision, goals, or thesis — or run `--import-context [path]` to bring one in."*

---

## Path B — With question (`/cpo --scan-strategy [question]`)

**What it does:** Scans strategy files, then cross-references them against the specific question before running the normal four-action flow. The grounding options in Action 1 are strategy-anchored — built from the tensions and alignments found, not from generic structural angles.

**This replaces the standard `STRATEGY_FILES_FOUND` posture confirmation.** Do not produce a separate posture summary — the cross-reference IS the strategy context. One output, not two stacked.

**Execution:**

1. Run the same bash block as Path A.

2. Read the listed files (same token cap: ≤5 files × ≤2,000 tokens each, `.claude/strategy/` first).

3. For each file, identify:
   - What it says that **supports or is directly relevant to** the question
   - What it says that **conflicts with or complicates** the question
   - What is **silent** — missing from the docs but needed to answer the question

4. Output the cross-reference **before** the Frame line, in this format:

> *Strategy cross-reference — [question in 3–5 words]:*
> *↑ Supports: [finding from docs — cite file] [inference tag if inferred]*
> *↓ Conflicts: [finding from docs — cite file] [inference tag if inferred]*
> *○ Silent: [what the docs don't address that this decision needs]*

   Omit any row that has nothing to say (e.g., if there are no conflicts, omit the Conflicts row). If all three rows are empty, note: *"Strategy docs exist but don't speak to this question — proceeding from inference."*

   **Do not begin Action 1 (Frame) until after the cross-reference has been output.** The cross-reference must appear first so the user can correct the Frame with full tension awareness. Output order is non-negotiable: cross-reference → reframe check (if triggered) → Frame.

5. **Reframe check (conditional — fires before Frame).** After outputting the cross-reference, check the ↓ Conflicts rows. A conflict is **question-reframing** if it changes any of:
   - The **success metric** being optimized (e.g., developer adoption vs. TVL)
   - The **primary user** the answer is built for (e.g., framework devs vs. institutional partners)
   - The **time horizon** the decision operates in (e.g., 3-month sprint vs. 18-month platform play)

   If a question-reframing conflict exists: output one line — *"Tension [name] may change the question itself — correct it now before I lock in grounding options, or say proceed."* — then **stop and wait** for the user's response. Reframe if they correct; proceed if they confirm.

   If no question-reframing conflict exists: proceed directly to the Frame. Do not produce a reframe check line.

   **This is distinct from the posture summary gate (which is always skipped in Path B).** The reframe check fires only when a conflict meets the three-criteria test above.

6. Then run the normal four-action flow starting at Action 1 (Frame).

7. **Grounding options (Action 1) must be strategy-anchored.** If the cross-reference surfaced a conflict, options must represent ways to resolve or defer that specific conflict — not generic structural angles. If the cross-reference surfaced a strong alignment, options should explore implications (sequencing, scope, timing) rather than re-litigate the alignment.

   **Self-verification before outputting grounding options:** For each option, identify the specific tension from the cross-reference it resolves or defers. If you cannot name it, rewrite the option. Generic distribution or risk-posture options that don't map to a named tension are not acceptable.

   **Labels must be situational — derived from the strategic dimension at play, not pre-assigned.** Do not use Bold/Balanced/Conservative. Use verb phrases that name the position:
   - For conflict tensions: *"Resolve toward [X] / Resolve toward [Y] / Defer [conflict name]"*
   - For alignment tensions (sequencing, scope, timing): *"Sequence by [logic] / Optimize for [outcome] / Time-box [decision]"*
   - The label axis should be derived from what actually differentiates the options, named in terms of the tension — not from a generic risk spectrum.

   Example — if positioning doc says "premium only" and goals doc says "SMB velocity via freemium":
   > ❌ Generic labels + generic options: A) Bold — gate on activation · B) Balanced — gate on volume · C) Conservative — separate Pro tier
   > ✅ Tension-mapped labels + anchored options: A) Resolve toward premium (subordinate SMB velocity, protect positioning) · B) Resolve toward freemium (subordinate premium positioning, run time-boxed experiment) · C) Defer the conflict (separate SMB acquisition from pricing architecture — decide later)

8. After the Verdict and next-steps menu, confirm: *"Strategy cross-reference was based on [N] file(s): [list]. Run `--scan-strategy` alone to refresh without re-analyzing."*

**If `NO_STRATEGY_FILES`:** Skip the cross-reference entirely. Note inline: *"No strategy files found — grounding from inference only."* Then proceed with the standard four-action flow.
