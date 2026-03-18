# CPO Skill — Behavioral Test Cases

These test cases define expected behavior for the `/cpo` skill. Use to verify behavior after modifying SKILL.md or any mode template.

Each test has: **Input**, **Expected behavior**, **Pass criteria**, **Fail signals**.

---

## Test 1: Direct prompt — four-action output

**Input:**
```
/cpo should we kill our enterprise motion
```

**Expected behavior:**
- Does NOT show a menu (A/B/C options)
- Runs all four actions in one response: Frame → Assess → Paths → Verdict
- Line 1 starts with `*I'm reading this as:`
- Line 2 starts with `*The` and names a specific Truth
- Paths use exactly `**Bold**`, `**Balanced**`, `**Conservative**`
- Verdict line starts with `*Verdict:` and ends with `Confidence: [level].*`

**Pass criteria:**
- All four structural markers present in correct order
- No menu (A/B/C) before output
- No questions before output
- Verdict names one of the three paths

**Fail signals:**
- Output shows `A) ... B) ... C) ...` menu
- Output asks a question before delivering Frame
- Verdict does not name Bold/Balanced/Conservative explicitly
- Structural markers out of order or missing

---

## Test 2: Zero-argument invocation

**Input:**
```
/cpo
```

**Expected behavior:**
- Returns exactly one question: "What are we deciding?"
- Nothing else — no options list, no capability explanation

**Pass criteria:**
- Output is one sentence
- No mode list, no capability explanation, no "here's what I can do"

**Fail signals:**
- Output lists modes or capabilities
- Output is more than 2 sentences
- Output shows "I'll show you the options" or similar

---

## Test 3: --go flag skips menu and simulation gate

**Input:**
```
/cpo --go simulate my Series B board meeting
```

**Expected behavior:**
- Does NOT ask "Shall I set up the room? A) Yes B) Analysis instead"
- Immediately starts the simulation
- Asks for board composition (this is mode-level setup, not a gate)

**Pass criteria:**
- No confirmation gate appears
- Simulation starts within the first response
- Board composition question (if asked) is framed as setup, not a gate

**Fail signals:**
- Output asks "shall I set up the room?" before starting
- Output shows a menu of options

---

## Test 4: Ambiguous prompt — four-action with visible inference

**Input:**
```
/cpo we're thinking about building a paid API
```

**Expected behavior:**
- Runs four-action flow, does NOT show A/B/C options menu
- Frame states the inferred decision and flags uncertainty inline
- Assess identifies the Dominant Truth for this specific decision
- Paths are tailored to "paid API" decision, not generic
- All four structural markers present

**Pass criteria:**
- Line 1 starts with `*I'm reading this as:`
- Frame flags what's being inferred (e.g., stage, whether this is build vs. pricing vs. GTM)
- No A/B/C menu appears
- No internal mode names visible

**Fail signals:**
- Output shows `A) ... B) ... C) ...` menu
- Frame states decision confidently without flagging uncertainty
- Paths are generic, not specific to API monetization decision

---

## Test 5: Context loaded — no bootstrap questions

**Setup:** `~/.cpo/context.md` exists with a valid Stage and recent Last updated date.

**Input:**
```
/cpo should we double down on our enterprise motion
```

**Expected behavior:**
- Runs preamble bash (loads context)
- States "Reading context: [stage] — optimizing for [doctrine]." in one line
- Routes to `ceo` mode (direct question)
- Does NOT ask about stage, business model, constraint, or any bootstrap field

**Pass criteria:**
- Context acknowledgment line appears
- No AskUserQuestion calls for context fields
- Verdict appears in first response

**Fail signals:**
- AskUserQuestion fires asking about stage or business model
- No context acknowledgment line
- Bootstrap questions appear despite CONTEXT_LOADED

---

## Test 6: Calibration — first use, no context, no --silent

**Setup:** `~/.cpo/context.md` does NOT exist.

**Input:**
```
/cpo we're building a B2B SaaS for legal teams and thinking about our GTM motion
```

**Expected behavior:**
- Infers stage from prompt (if possible — "B2B SaaS" gives model; stage less clear)
- Flags inference inline: *"Inferring: B2B SaaS — stage unclear, assuming early-growth"*
- If stage can't be inferred: asks ONE question (stage + model combined) before showing options
- Does NOT ask 5 questions before showing any output

**Pass criteria:**
- At most one AskUserQuestion fires before output is shown
- Inference is flagged explicitly
- Proceeds with analysis after at most one round-trip

**Fail signals:**
- Multiple sequential AskUserQuestion calls before any output
- Full 5-field bootstrap fires unprompted

---

## Test 7: Simulation gate fires for roundtable (no --go)

**Input:**
```
/cpo let's run an investor roundtable
```

**Expected behavior:**
- Shows the simulation confirmation gate: "You've asked for an investor simulation — shall I set up the room? A) Yes, let's start · B) Actually, I'd rather have a strategic analysis instead."

**Pass criteria:**
- Confirmation gate appears
- Two options: start simulation or switch to analysis

**Fail signals:**
- Simulation starts immediately without gate
- A general options menu appears instead of the gate

---

## Test 8: Staleness check — recent context, no pivot language

**Setup:** `~/.cpo/context.md` has `Last updated: [30 days ago]`, Stage: post-PMF.

**Input:**
```
/cpo should we expand to Europe?
```

**Expected behavior:**
- Loads context, confirms stage in one line
- Does NOT ask "is your context still accurate?"
- Routes to `ceo` mode and gives verdict

**Pass criteria:**
- No staleness question
- One-line context confirmation
- Analysis proceeds immediately

**Fail signals:**
- Staleness question fires for a routine question
- Output asks "is your stage still post-PMF?"

---

## Test 9: Staleness check — explicit pivot language

**Setup:** `~/.cpo/context.md` has `Last updated: [30 days ago]`, Stage: post-PMF.

**Input:**
```
/cpo I'm starting to rethink our entire thesis — not sure the enterprise motion was right
```

**Expected behavior:**
- Detects "rethink" as explicit pivot language
- Fires staleness check ONCE: "Your context says post-PMF — still accurate? Yes/no."
- After user responds, proceeds with analysis

**Pass criteria:**
- Staleness check fires exactly once
- Framed as a yes/no question
- Does not re-run full bootstrap

**Fail signals:**
- No staleness check despite "rethink" language
- Full bootstrap fires instead of a targeted yes/no
- Staleness check fires multiple times

---

## Test 10: Role detection — influencer framing

**Input:**
```
/cpo I want to propose a pricing change but I'm not sure my CPO will approve it
```

**Expected behavior:**
- Detects influencer role ("my CPO")
- Surfaces the split: "I'm hearing two things — validating the pricing change and building the case for your CPO. Which first?"
- Options: A) Validate the decision · B) Build the pitch

**Pass criteria:**
- Role split is surfaced explicitly
- Two clear options offered

**Fail signals:**
- Routes silently to upward-pitch without surfacing the split
- Routes to ceo mode ignoring the influencer signal
- No mention of the dual nature of the request

---

## Test 11: Four-action structure — all markers enforced

**Input:**
```
/cpo what GTM motion should we use for our developer tool
```

**Expected behavior:**
- All four structural markers appear in correct order in one response
- No questions, no menu, no preamble before Line 1

**Pass criteria:**
- Line 1: starts with `*I'm reading this as:`
- Line 2: starts with `*The` + Truth name + `is what this turns on:`
- Lines 3–5: `**Bold**`, `**Balanced**`, `**Conservative**` in that order
- Final line: starts with `*Verdict:` and ends with `Confidence: [High/Medium/Low].*`
- All in one response

**Fail signals:**
- Any structural marker missing
- Markers out of order
- Any question before Line 1
- "Assessment:", "Framing:", "Paths:" headers used instead of the enforced markers
- Verdict does not end with `Confidence: [level].*`

---

## Test 12: Correction loop — re-run from Assess

**Turn 1 input:**
```
/cpo should we expand to enterprise
```
CPO infers post-PMF in the Frame line.

**Turn 2 input:**
```
actually we're pre-PMF, $200k ARR
```

**Expected behavior:**
- CPO acknowledges the correction in one line: `*Got it — re-running with pre-PMF, $200k ARR.*`
- Immediately re-runs from Action 2 (Assess) with updated assumption
- Does NOT repeat the Frame line
- New Paths are materially different from the post-PMF version (pre-PMF paths should be more conservative / learning-oriented)
- New Verdict reflects pre-PMF doctrine

**Pass criteria:**
- Acknowledgment line is one sentence starting with `*Got it`
- Action 1 (Frame) is NOT repeated
- Paths and Verdict change meaningfully between Turn 1 and Turn 2

**Fail signals:**
- CPO asks a clarifying question instead of re-running
- Frame line is repeated in Turn 2 response
- Paths in Turn 2 are identical to Turn 1

---

## Test 13: --go skips Frame and Assess

**Input:**
```
/cpo --go should we raise now
```

**Expected behavior:**
- One-line prefix: `*Running: go/no-go on raising now.*` or similar
- Immediately delivers Paths + Verdict
- No Frame line (`*I'm reading this as:`)
- No Assess line (`*The [Truth] is what this turns on:`)

**Pass criteria:**
- `*Running:` prefix appears
- No `*I'm reading this as:` line
- No `*The ... is what this turns on:` line
- Bold/Balanced/Conservative + Verdict present

**Fail signals:**
- Frame line appears despite `--go`
- Assess line appears despite `--go`
- A question is asked before output

---

## Test 14: --quick delivers Verdict only

**Input:**
```
/cpo --quick should we kill the mobile app
```

**Expected behavior:**
- One paragraph only
- No Frame line, no Assess line, no Paths structure
- Verdict is embedded in prose: names a path, gives a reason, names kill criteria

**Pass criteria:**
- Output is one paragraph (3–5 sentences)
- No `**Bold**`, `**Balanced**`, `**Conservative**` labels
- Verdict is clearly stated (path + reason + kill criteria)
- No questions

**Fail signals:**
- Full four-action structure appears
- Three Paths labels appear
- Output is more than one paragraph without `--deep`

---

## Test 15: --deep expands Assess to all five Truths

**Input:**
```
/cpo --deep should we launch a free tier
```

**Expected behavior:**
- Frame line present as normal (`*I'm reading this as:`)
- Assess expands to all five Truths — one paragraph each, not just the Dominant Truth
- All five Truth names appear: User, Strategic, Economic, Macro-Political, Execution
- Full 10-section output follows after Conservative path
- Verdict still present at end with Confidence level

**Pass criteria:**
- Frame line present
- All five Truth names appear in Assess section
- Output contains sections: Problem Definition, Five Truths Assessment, Strategic Options, Recommendation + Kill Criteria, Sequencing, Risks, GTM Considerations, Organizational Implications, Open Questions, Decision Memo
- Verdict present at end with `Confidence: [level].*`
- Output is substantially longer than a default four-action response

**Fail signals:**
- Only Dominant Truth appears (same as default behavior)
- 10-section output absent
- Verdict missing
- Frame line absent (--deep does not suppress Frame)
- Output same length as non-`--deep` response

---

## Running these tests

These are behavioral tests — run them manually by invoking `/cpo` with the specified inputs and checking the output against pass criteria.

For automated testing: the expected output patterns can be used as assertions in an LLM eval harness (e.g., checking for presence/absence of specific phrases in the output).

Version: 4.0.0
Last updated: 2026-03-18
