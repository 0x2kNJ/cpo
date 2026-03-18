# CPO Skill — Behavioral Test Cases

These test cases define expected behavior for the `/cpo` skill. Use to verify behavior after modifying SKILL.md or any mode template.

Each test has: **Input**, **Expected behavior**, **Pass criteria**, **Fail signals**.

---

## Test 1: Direct question auto-routes without menu

**Input:**
```
/cpo should we kill our enterprise motion
```

**Expected behavior:**
- Does NOT show a menu (A/B/C options)
- Immediately routes to `ceo` mode
- Leads with a verdict (Go / No-Go / Conditional)
- Includes Three Paths (Bold / Balanced / Conservative)
- Includes kill criteria

**Pass criteria:**
- First line of output contains a verdict or "Running:" prefix
- Output contains "Three Paths" or Bold/Balanced/Conservative structure
- Output contains kill criteria

**Fail signals:**
- Output shows "A) ... B) ... C) ..." menu before giving verdict
- Output asks "What would you like to do?" or similar
- Output is more than ~400 words without `--deep`

---

## Test 2: Zero-argument invocation

**Input:**
```
/cpo
```

**Expected behavior:**
- Returns exactly: "What's the problem or decision? I'll show you the options."
- Nothing else

**Pass criteria:**
- Output is one sentence or very close to it
- No mode list, no capability explanation, no "here's what I can do"

**Fail signals:**
- Output lists modes or capabilities
- Output is more than 2 sentences

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

## Test 4: Ambiguous prompt shows options menu

**Input:**
```
/cpo we're thinking about building a paid API
```

**Expected behavior:**
- Shows 2–4 lettered options (A/B/C/D)
- Options are in plain English, no internal mode names visible
- Includes escape hatch: "Or describe what you need and I'll route it"

**Pass criteria:**
- Output contains lettered options
- No mode names (ceo, blue-ocean, red-team, etc.) appear in output
- Options are specific to the prompt, not a generic capability list

**Fail signals:**
- Mode names appear (e.g., "A) ceo mode", "B) blue-ocean analysis")
- Options are generic instead of specific to "paid API for a product"
- Only one option shown

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

## Running these tests

These are behavioral tests — run them manually by invoking `/cpo` with the specified inputs and checking the output against pass criteria.

For automated testing: the expected output patterns can be used as assertions in an LLM eval harness (e.g., checking for presence/absence of specific phrases in the output).

Version: 3.7.0
Last updated: 2026-03-17
