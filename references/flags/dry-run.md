# Flag: `--dry-run` — Exploratory Mode (No Journal Write)

**Purpose:** Run the full four-action flow (Frame → Assess → Paths → Verdict) without writing to the decision journal. For "what if" questions, hypothetical exploration, and thinking out loud — without polluting the journal with decisions that were never real.

## Trigger

`/cpo --dry-run [prompt]` or combined with other flags: `--dry-run --go`, `--dry-run --deep`

## Behavior

**Everything runs normally** — preamble, context load, mode routing, three-response flow (or `--go`/`--quick` bypass), D-M menu. The user gets the exact same output they would without `--dry-run`.

**What changes:**

1. **No journal write.** After the verdict, skip the journal write step entirely. Do not create a YAML file. Do not run the coherence validator against active entries (since there's nothing to persist, coherence is informational noise).

2. **No revision supersession.** Since no entry is written, no prior entries are touched.

3. **No trace file.** Skip execution trace write — dry runs are ephemeral.

4. **Visual indicator.** Add one line at the top of Response 1 (or the single `--go`/`--quick` response):

   ```
   *[Dry run — exploring, not committing. Nothing written to journal.]*
   ```

5. **D-M menu available.** All D-M picks work normally — simulations (H, I), stress tests (D), deep dives (E), etc. None of them write to the journal either while `--dry-run` is active.

6. **Commit escape hatch.** Add one extra option after all D-M picks (after M if rendered, otherwise after L):

   ```
   N) Commit  — write this decision to the journal (exits dry-run mode)
   ```

   `N` is always the next letter after the last D-M option — it does not conflict with path selection (A/B/C) or the D–M action menu. When verification subagent is active (`--deep`/`--go`), its Check 6 passes if the D-M menu through M is present; `N) Commit` is additional and does not affect the check.

   If the user picks N: write the journal entry using the standard journal-schema.md flow (including coherence validator and trace). Use `_DRY_START_DATE` as the `date:` field so the Decision Decay Index counts from when the analysis happened, not when the user clicked commit. Confirm: *"Decision committed to journal. Dry-run mode exited."*

**Timestamp tracking:** At invocation, capture the dry-run start date:
```bash
_DRY_START_DATE=$(date +%Y-%m-%d)
```
Pass `_DRY_START_DATE` through the session context. If `N) Commit` is selected, `journal-schema.md` uses `_DRY_START_DATE` as `date:` (see schema: `_ENTRY_DATE="${_DRY_START_DATE:-$_DATE}"`). This prevents the Decision Decay Index from being off by the duration of the exploration session.

## Combination Rules

- `--dry-run --go`: Works. Single response, no journal.
- `--dry-run --quick`: Works. Condensed response, no journal.
- `--dry-run --deep`: Works. Full 10-section, no journal.
- `--dry-run --brief`: Invalid combination. `--brief` reads the journal, doesn't produce a decision. If both present: *"--dry-run doesn't apply to --brief (no decision to skip). Running --brief normally."* Drop `--dry-run`.
- `--dry-run --since [date] [question]`: Works. `--since` enriches with temporal delta from the journal, then runs the full four-action flow with that context. No journal write. Combination produces: temporal cross-reference inline, then dry-run analysis.
- `--dry-run --since` (alone, no question): Invalid — `--since` without a question just reads the journal. Drop `--dry-run`, run `--since` normally: *"--dry-run doesn't apply to --since (journal read, no question). Running --since normally."*
- `--dry-run --score` / `--verify` / `--assumptions` / `--history` / `--trail` / `--outcome` / `--patterns` / `--replay`: Invalid — these are journal-reading flags with no decision to produce. Same treatment: drop `--dry-run`, run flag normally, note: *"--dry-run doesn't apply to [flag] (journal read, not write). Running [flag] normally."*
- `--dry-run --export`: Works. Exports the response as markdown to `~/.cpo/exports/[date]-[mode]-dry-run.md` (filename includes `-dry-run` marker). No journal write.
- `--dry-run #decision-id [new context]`: Works. Loads the prior decision's context for comparison (DECISION_OBJECT_LOADED — delta frame), generates the full exploratory analysis including the delta frame, but does NOT write revision N+1 to the journal and does NOT supersede prior revisions. Verdict is shown as exploratory — pick `N) Commit` to persist as a real revision.

## Why This Exists

The journal is the foundation of the Intelligence Loop — `--score`, `--verify`, `--assumptions`, `--drift` all depend on clean data. Exploratory "what if I did X instead?" questions pollute that data with decisions the user never intended to act on. `--dry-run` preserves journal integrity while keeping the full analytical flow available for exploration.
