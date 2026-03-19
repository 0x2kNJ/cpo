# Preamble State Handlers — Detailed Rules

These rules govern how to interpret and act on each preamble bash output state. SKILL.md contains the bash block itself; this file contains the detailed handling logic.

---

## Context State Handling

- `CONTEXT_LOADED_FRESH`: use the printed context. Confirm in one line: *"Reading context: [stage] — optimizing for [doctrine]."* No staleness check unless pivot language detected.
- `CONTEXT_LOADED_STALE`: use context but trigger staleness check once: *"Your context says [stage] — still accurate? Yes/no."* If `--silent`: skip. If confirmed stale: offer to update Stage inline, do not re-run full bootstrap.
- `CONTEXT_LOADED_MINIMAL`: use context but flag inline: *"Context loaded — only [N] fields filled. Inferring the rest."* Offer to run `--save-context` after the first output.
- `NO_CONTEXT`: infer stage and model from the user's prompt. Note inferences inline. Ask at most one question before showing output. Never ask 5 questions before the user sees anything.

**Staleness check:** Trigger on `CONTEXT_LOADED_STALE` OR if any context state includes explicit pivot/thesis language ("rethink," "pivot," "starting over," "not sure about our thesis," "what went wrong"). Fire at most once. If `--silent`: skip entirely.

---

## Decision Journal State Handling

**DECISIONS_FOUND:** Load the printed entries as recent decision history. Use them silently to: (1) inform `--since` diffs, (2) power `--brief` and `--trail` outputs, (3) detect pattern consistency ("Last month you committed to the Balanced path on enterprise — this week you're asking about doubling down. What changed?"). Do not read these aloud unless the user asks for history or `--brief` / `--trail` is passed. Entries with `status: invalidated` are silently skipped during context load — they remain accessible via `--history` for audit.

**NO_DECISIONS:** No prior decision journal entries. Proceed normally. If the user asks about history, note there are no logged decisions yet.

**DECISION_OBJECT_LOADED: [id]:** A named decision object exists. All matching journal entries are printed chronologically. This is a *returning decision*. Store `_DECISION_ID` for the journal write.

**DECISION_OBJECT_NEW: [id]:** The user tagged a decision with `#name` but no prior entries exist. This is a *new named decision*. Store `_DECISION_ID`. Proceed with the normal four-action flow.

---

## Integrations State Handling

**INTEGRATIONS_FOUND:** Live data is available. Inject it into the Five Truths assessment under the relevant Truth. Label every live data point with its source and date: *[source-name, YYYY-MM-DD]*. Never mix live data with inference without labeling. If a live data point directly touches a kill criterion, surface it prominently: *"Kill criterion approaching: CAC $487/$500 [source-name]."*

**NO_INTEGRATIONS:** Proceed from context and inference. Offer once at the end of the first output: *"No live data connected — run `/cpo --setup-integrations` to detect available data sources and enrich Five Truths with real numbers."* Never offer again in the same session.

---

## Version State Handling

**VERSION_MISMATCH:** Surface at the top of the first response: *"Note: you're running v[skill_version] but your saved state is from v[installed]. Things may behave differently. Run `--update` to upgrade or `--save-context` to refresh."* Use the actual values from the bash output — never hardcode. Then continue normally.

---

## Strategy Files State Handling

**STRATEGY_FILES_FOUND:** After the preamble bash output, read up to 5 of the listed files using the Read tool — hard cap 2,000 tokens per file (prioritize `.claude/strategy/` entries first, then project files). Run the lightweight tension check: note any conflicts that change the **success metric**, **primary user**, or **time horizon** of the user's question.

**If a question-reframing tension is found:** Output posture as 2-sentence context, then surface the tension as an angle-selection directly — the user's pick IS the confirmation:

> *Strategy ([N] files): [2-sentence posture — what the strategy commits to and what this means for the decision.]*
> *One open tension: [tension name] — [one sentence on what changes depending on how you resolve it]. Which angle?*

-> AskUserQuestion with A/B/C options that resolve the tension. **Always include a `RECOMMENDATION:` field**. The user's selection IS the confirmed frame — proceed immediately to Paths (Response 2).

**Label timing:** Labels here are provisional — derived from the tension itself, not from the full Assess. After the user picks, run Assess silently to validate the frame. If the dominant Truth shifts the angle, acknowledge in one line before Paths.

**If no question-reframing tension is found:** Fold posture silently into the Frame inference. Proceed directly to Response 1 with `*I'm reading this as:` as normal. After Line 1, append one inline context note: *[Strategy: [one-clause posture summary.]]*.

**Exception — `--go` with `STRATEGY_FILES_FOUND`:** Skip the tension-surfacing step entirely. Incorporate posture silently into the Frame. If a question-reframing conflict is found, add one inline line: *[Strategy tension: [name] — may affect framing — correct if wrong.]*.

**Token budget:** Strategy content is capped at 10,000 tokens total. Strategy context is always lower priority than the three-response architecture.

**NO_STRATEGY_FILES:** Proceed normally. After the full three-response flow is complete, offer once: *"No strategy docs found in this project. Want me to create a baseline? I'll ask 5 questions and save it to `.claude/strategy/`."*
