# Mode: `sequence` — Full Template

**Stance**: Prioritization engine. Take a set of initiatives, features, or ideas and produce a sequenced execution plan. The order you build things matters more than what you build — wrong sequencing wastes effort, closes off options, and delays learning.

**Use when**: you have multiple validated initiatives and need to decide what order to build them, or when a roadmap needs restructuring around dependencies and uncertainty reduction.
**Do NOT use when**: the initiatives themselves haven't been evaluated yet (use default mode or `ceo` first to assess whether each one is worth doing at all).

**Five Truths emphasis**: All five, but through the lens of *dependency and uncertainty*. What must be true before the next thing can start? What produces the most signal per unit of effort?

---

## Calibration

Ask via `AskUserQuestion` before running the template. Max 4 total.
*If AskUserQuestion unavailable:* ask as a numbered list and wait for response.

| Question | Header | Options |
|---|---|---|
| How many initiatives are we sequencing? | Count | 3–5 items · 6–15 items · 15+ items (full roadmap planning) |
| What's the primary constraint? | Constraint | Engineering capacity / team bandwidth · Capital / runway · Market timing / competitive window · Hard dependencies · All of the above |
| Are these initiatives validated or still hypotheses? | Status | All validated, just need ordering · Mix — some validated, some not · Mostly hypotheses |
| What's the planning horizon? | Horizon | Next sprint (2 weeks) · Next quarter · Next 6–12 months · Annual / multi-year |

**Calibration adjustments**:
- *15+ items + "all of the above" constraint*: Start with a kill pass before scoring anything. Remove items that can't survive any reasonable sequencing criteria. Only then apply the scoring table.
- *Mixed validated + hypotheses*: Do not sequence hypotheses alongside validated bets as if they're equivalent. Separate them explicitly. Validated bets get sequenced; hypotheses get assigned a validation task first.
- *Annual horizon*: Produce a phased roadmap (Phase 1 / Phase 2 / Phase 3) with explicit gates and kill conditions at each phase. A flat ranked list is the wrong format for annual planning.
- *Hard dependencies*: Run the dependency map first (step 3) before scoring. Dependency constraints override preference-based sequencing.

---

## Required Outputs

- [ ] Sequencing criteria stated explicitly (not just "here's the order")
- [ ] Scoring table completed for all initiatives
- [ ] Dependency map produced (ASCII or table)
- [ ] What We're Killing section completed (not just "backlog")
- [ ] Validation gates table completed with kill conditions defined
- [ ] Calendar reality check performed

---

## Template

1. **Current State** — Establish the baseline honestly.
   - What's been **built** and is in production / being used by real people?
   - What's been **validated** — evidence of demand, willingness to pay, or PMF signal?
   - What's been **assumed** — believed to be true but not yet tested?
   - What's **in flight** — being built right now, not yet shipped?
   - What's **blocking** — dependencies, unknowns, resource gaps that constrain sequencing?
   Be specific. "We've launched X" is different from "X is live and has 50 paying customers."

2. **Sequencing Criteria** — The principles driving this order.
   Name the criteria explicitly, so the team can debate the criteria rather than just the order. Common criteria:
   - **Uncertainty reduction**: Do the riskiest bets first while we can still pivot cheaply
   - **Dependency unblocking**: Ship the foundation before building on it
   - **Revenue impact**: Do the things that improve unit economics fastest
   - **Competitive urgency**: Do the things where the window is closing
   - **Learning value**: Do the things that produce the most actionable signal per unit of effort
   - **Organizational capacity**: Do the things the current team can actually execute

   State which 2-3 criteria are driving THIS sequencing. Rank them. If they conflict, state which wins.

3. **The Stack** — Scored, ordered, with reasoning.

   | Initiative | Uncertainty (H/M/L) | Revenue impact (H/M/L) | Blocks / Blocked by | Strategic value (H/M/L) | Effort (S/M/L/XL) | Phase |
   |------------|---------------------|----------------------|--------------------|-----------------------|-------------------|-------|
   | [Name] | | | [what it blocks] | | | 1 |

   For each initiative, also state:
   - **Why here** (in this position): what this unlocks OR what it depends on
   - **Signal produced**: what becomes known after this ships that we don't know now
   - **The hard part**: the one thing most likely to cause this to slip or fail

4. **Dependency Map** — Visual ordering constraints.
   ```
   [Initiative A] ──must-precede──▶ [Initiative B] ──must-precede──▶ [Initiative C]
         │                                                                   ▲
         └──────────────can-run-parallel──────────────▶ [Initiative D] ──────┘
   ```
   - What's the **critical path**? (The sequence that determines minimum total time)
   - What can run **in parallel** without shared state conflicts?
   - What **external dependencies** exist — partnerships, regulatory clearance, hires, infrastructure — that aren't in our control?

5. **What We're Killing** — Explicit deprioritization.
   This section is not optional. If you have 10 ideas and you're sequencing 4, you must account for the other 6.

   | Initiative | Why not in the stack | Conditions for revival | Who needs to know |
   |------------|---------------------|----------------------|------------------|
   | [Name] | [Reason] | [What would change this] | [Stakeholder] |

   "Not now" is a real decision. Make it explicitly and communicate it explicitly.

6. **Validation Gates** — Kill conditions between phases.
   Define these before you start. If you won't define kill conditions now, you won't honor them later.

   | Gate | After phase | Evidence required to proceed | Kill condition (stop if...) | Owner |
   |------|-------------|------------------------------|----------------------------|-------|
   | Gate 1 | Phase 1 | [what must be true to continue] | [what would cause us to stop/pivot] | [person] |
   | Gate 2 | Phase 2 | | | |

   A gate without a kill condition is not a gate — it's a checkpoint you'll rationalize through.

7. **Recommendation** — Full sequence with calendar reality check.
   - `RECOMMENDATION: Start with [X] because [reason tied to stated sequencing criteria]`
   - Full sequence: Phase 1 → Phase 2 → Phase 3 (with rough timelines)
   - **Calendar reality check**: Given current team size, active commitments, and the effort estimates above — is this sequence achievable? If not, what gets cut or pushed? Be honest about capacity.
   - What's the highest-risk assumption in this sequence? What happens to the plan if it's wrong?
