# Truth Subagents — Independent Per-Truth Analysis for `--deep`

**Purpose:** Prevent cross-Truth bias in `--deep` mode's Five Truths Assessment. When analyzing all five Truths in sequence, findings from earlier Truths unconsciously anchor later ones — e.g., a strong Economic Truth finding makes the Strategic Truth assessment overly optimistic. Independent per-Truth analysis produces cleaner, more honest assessments.

## When It Fires

**Only in `--deep` mode.** Standard mode already identifies only the Dominant Truth. `--go` and `--quick` are speed-optimized — adding subagent overhead defeats their purpose.

## How It Works

### Step 1 — Shared Context Preparation

Before the per-Truth analysis, prepare a shared context block. Each Truth assessment receives only the dimensions of shared context relevant to its lens — not the full context block. Filter as follows:

| Truth | Include from shared context (`context.md` fields + prompt inferences) |
|---|---|
| User Truth | Stage, Top priorities, user segment (inferred from prompt or Business model) |
| Strategic Truth | Stage, Business model, competitors (inferred from prompt), Top priorities |
| Economic Truth | Business model, Core constraint (if financial), Open question (if financial), pricing/revenue details (inferred from prompt) |
| Macro-Political Truth | Business model vertical, Core constraint (if regulatory), any regulatory/geopolitical signals from prompt |
| Execution Truth | Core constraint (if resource-based), Operating bias, team/tech/runway details (inferred from prompt) |

**Filtering principle:** Each Truth receives only `context.md` fields relevant to its lens, plus prompt-inferred details that fall within that Truth's domain. Fields like "team size," "burn rate," and "tech stack" are NOT stored in `context.md` — they must be inferred from the user's prompt or Core constraint. If a field is unavailable, the Truth assessment flags it as inferred.

Full shared context (all fields) is only used during Step 3 synthesis — where cross-Truth interactions are explicitly being analyzed. During Step 2, each Truth sees only its filtered slice.

**Base case (no prior entry):** If no prior decision entry exists (new named decision or untagged prompt), shared context is: user context from `~/.cpo/context.md` + stage + doctrine. No delta handling needed — proceed to Step 2.

**Returning decision handling:** When a `#decision-id` is loaded (DECISION_OBJECT_LOADED), the prior decision's Truth findings are NOT included in each Truth's shared context for Step 2. Each Truth is assessed fresh with new context only. After Step 3 synthesis, add a closing delta block to Section 2: *"Vs. prior ([date]): [Truth name] shifted — [one-line summary of change]. [Truth name] unchanged."* This surfaces what moved without anchoring current assessment to prior conclusions.

**Score profile weakness:** If `_WEAK_TRUTH` is set, flag it only in the shared context slice for that specific Truth's assessment. Do not inject it into other Truths' slices.

### Step 2 — Independent Truth Assessments

Run each Truth assessment independently. Each assessment receives ONLY the shared context — NOT the findings from other Truths. The assessments are:

1. **User Truth** — What does the user actually want, fear, and do? Behavior > stated preference.
2. **Strategic Truth** — Where does this move position us on the competitive board?
3. **Economic Truth** — Does the unit economics work? CAC, LTV, payback, margin at scale?
4. **Macro-Political Truth** — What regulatory, geopolitical, or ecosystem forces could override good execution?
5. **Execution Truth** — Can we actually build this with our current team, runway, and tech stack?

Each assessment produces:
- **Finding:** 2-3 sentence assessment
- **Evidence level:** fact / assumption / inference / judgment for each claim
- **Confidence:** High / Medium / Low for this specific Truth
- **Key risk:** The single biggest risk this Truth surfaces
- **Data gap:** What data would move this from inference to fact

### Step 3 — Merge and Synthesize

After all five assessments complete, merge the findings. This is where cross-Truth interactions are analyzed — but now they emerge from independent findings rather than from anchoring:

- **Dominant Truth identification:** Which Truth has the strongest bearing on the decision? Base this on the independent findings, not on which Truth was analyzed first.
- **Conflict detection:** Do any Truths directly contradict each other? (e.g., Economic Truth says "unit economics don't work" while Strategic Truth says "must enter this market now"). Surface conflicts explicitly — they are the most valuable output.
- **Truth fingerprint:** Classify each Truth as Grounded (evidence available) or Inferred (assumption-dependent) based on the independent assessments.

### Step 4 — Integration into `--deep` Output

The merged findings feed into `--deep`'s 10-section format:

- **Section 2 (Five Truths Assessment):** Present all five independent findings first (one by one). Then, as a closing block within Section 2, synthesize cross-Truth conflicts: *"Conflict: [Truth A] suggests X, but [Truth B] suggests Y. Resolution depends on [named data point]."* Synthesis happens inside Section 2 — not after it — so Section 3 can be built from complete conflict-aware context.
- **Section 3 (Strategic Options):** Three paths are generated AFTER Section 2 synthesis is complete. Each path should reflect the Truth conflicts from synthesis — ideally, each path resolves the dominant conflict differently.
- **Section 6 (Risks & Mitigations):** Pull `key_risk` from each Truth assessment.

## Implementation Model

**This is NOT a separate Agent tool invocation.** It is a structured reasoning approach within the same response context. The model generates each Truth's assessment in an isolated mental frame — completing one Truth fully before starting the next, without referencing prior Truth findings during generation.

**Practical enforcement:** When generating each Truth assessment in `--deep` mode:
1. State the Truth name and its question.
2. Assess ONLY using shared context + this Truth's lens.
3. Produce the finding, evidence level, confidence, key risk, and data gap.
4. Do NOT reference findings from previously assessed Truths.
5. After all five are complete, THEN synthesize cross-Truth interactions.

**Trace integration:** If execution trace is active, append a `truth_subagents` checkpoint:

```json
{
  "step": "truth_subagents",
  "truths_assessed": ["User", "Strategic", "Economic", "Macro-Political", "Execution"],
  "dominant_truth": "[name]",
  "conflicts_found": [{"truth_a": "[name]", "truth_b": "[name]", "description": "[one line]"}],
  "fingerprint": {"grounded": ["[names]"], "inferred": ["[names]"]}
}
```

## Why This Exists

In sequential Five Truths analysis, the first Truth assessed acts as an anchor. If Economic Truth is assessed first and shows strong unit economics, the Strategic Truth assessment unconsciously inherits optimism ("economics work, so the strategic bet is sound"). Independent assessment prevents this — each Truth stands on its own evidence, and conflicts between Truths become visible instead of being unconsciously smoothed over.
