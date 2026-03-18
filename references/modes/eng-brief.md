# Mode: `eng-brief` — Full Template

**Stance**: Product-to-engineering alignment. Give engineering clear context on what to build and why, without over-specifying implementation. The best eng briefs explain the problem space, constraints, and success criteria — then trust engineering to find the best solution.

**Use when**: handing off a validated product direction to engineering, writing a PRD or brief, aligning eng on the "what and why" before they start building.
**Do NOT use when**: the direction itself isn't validated yet (use `ceo` or default mode first), or when you're responding to engineering's concerns (use `eng-translate`).

**Five Truths emphasis**: User Truth + Execution Truth (co-primary). Engineers need to understand the user problem deeply enough to make good technical tradeoffs. They also need honest execution constraints.

---

## Calibration

Ask via `AskUserQuestion` before running the template. Max 4 total.
*If AskUserQuestion unavailable:* ask as a numbered list and wait for response.

| Question | Header | Options |
|---|---|---|
| Who is this brief for? | Audience | Our team, knows the codebase well · Our team, recently onboarded · External contractors / new hires |
| Is the "what" settled? | Scope | Fully decided — just spec it · Open to eng input on approach · Actively seeking alternative approaches |
| What's the timeline reality? | Timeline | Ship this sprint · Ship this quarter · No hard deadline |
| What's the biggest risk of getting this brief wrong? | Risk | Engineering builds the wrong thing · Engineering builds the right thing wrong (over/under-engineered) · Scope creep during build |

**Calibration adjustments**:
- *Scope open + no hard deadline*: Lead with Strategic Context and User Problem. Don't write the Scope table until open questions are resolved — a premature scope table produces false precision.
- *Fully decided + this sprint*: Skip lengthy context setup. Lead with Scope, Constraints, Tradeoffs, and Success Criteria. Be maximally specific on what done looks like.
- *External contractors or new hires*: Be more explicit in every section. No internal references. Add a "Context you'd normally get from Slack" section to the top. Constraints and Tradeoffs especially need spelling out.

---

## Required Outputs

- [ ] Objective in "enable [who] to [do what] so that [outcome]" format
- [ ] At least one real user signal included (quote, ticket, metric, or competitive signal)
- [ ] Scope table completed (in / out with reasoning)
- [ ] Tradeoff priorities stated explicitly (not just "use judgment")
- [ ] Acceptance criteria table completed (min 2 rows, at least 1 quantitative)
- [ ] Open questions table completed (or explicitly empty with reason)

---

## Template

1. **Objective** — One sentence that a new engineer could ship to.
   - Format: "Enable [who] to [do what] so that [business/user outcome]."
   - Test: Can an engineer who wasn't in any of the meetings understand what "done" looks like from this sentence alone?
   - Anti-pattern: "Build X feature." That's a solution, not an objective.

2. **User Problem** — Engineers make better decisions when they feel the user's pain.
   - Job-to-be-done: What is the user actually trying to accomplish? (Not "use our product" — the underlying job.)
   - Current workaround: What do they do today? Be specific — describe the actual workflow steps.
   - Pain quantification: How bad is it? (Time wasted? Revenue at risk? Trust broken? Frequency?)
   - Real signal: Include at least one piece of evidence — user quote, support ticket theme, usage data, or competitive loss. If none exists, flag it explicitly: "We don't have direct user signal here yet — this is an inference from [X]."

3. **Why Now** — Context that changes how engineering prioritizes.
   - What changed that makes this the right time? (New data, competitive move, strategic window, unblocked dependency)
   - Cost of waiting a quarter: What gets worse, closes, or breaks if we wait 90 days?
   - Risk of shipping too early: What's not yet validated that we're betting on?

4. **Strategic Context** — Where this sits in the larger picture.
   - Current phase: Are we searching (validating hypotheses) or executing (scaling what works)?
   - What this unlocks next: What becomes possible after this ships?
   - What this forecloses: What options or directions does this approach close off?
   - Company-level connection: How does this contribute to the 12-month position?

5. **Scope** — Explicit, testable, bounded.

   | In Scope | Out of Scope | Why out of scope |
   |----------|-------------|-----------------|
   | [specific capability] | [thing we're not building] | [one-line reason] |

   "In scope" means: if engineering ships this and it doesn't include X, they've failed. Be that specific.
   "Out of scope" is the most important column. Be exhaustive. Every item in this column prevents a future scope creep conversation.

6. **Constraints** — Hard limits engineering must respect.
   - Timeline: [date] — is this a hard date (external commitment) or a target (internal)?
   - Performance: [any specific requirements — latency, throughput, reliability]
   - Backward compatibility: [what must not break for existing users or integrations]
   - Regulatory / compliance: [any non-negotiable requirements]
   - Stack / architecture: [any constraints imposed by existing systems or external dependencies]
   - Resource: [team size, key personnel dependencies, other active projects competing for attention]

7. **Tradeoffs** — Where engineering has latitude, and how to use it.
   - "If we can't do [X] in the timeline, prefer [Y] over [Z] because [user impact reason]."
   - "It's acceptable to sacrifice [A] in order to protect [B]."
   - "The thing we must never compromise on, regardless of time pressure, is [Z]."
   - This section is the most important input for engineering judgment calls during build. If it's missing, engineering will either ask constantly or guess wrong.

8. **Success Criteria** — How we know this worked.

   | Criteria | How we measure it | Target | Must-have or nice-to-have |
   |----------|------------------|--------|--------------------------|
   | [criterion 1] | [measurement method] | [value] | Must-have |
   | [criterion 2] | [measurement method] | [value] | Nice-to-have |

   - At least one quantitative metric. If you can't measure it, you can't ship it with confidence.
   - Include the failure signal: What would tell us this didn't work? Define it before shipping so we don't rationalize after.

9. **Open Questions** — Blockers to resolve before build starts.
   If any of these are answered wrong during build, the spec materially changes. Resolve them now.

   | Question | Why it matters | Owner | Needed by | Blocking build? |
   |----------|----------------|-------|-----------|-----------------|
   | [question] | [what changes if answered differently] | [person] | [date] | Y/N |

   If this table is empty, say so explicitly: "No open questions — the direction is fully resolved."
