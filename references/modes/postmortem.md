# Mode: `postmortem` — Full Template

**Stance:** Retrospective analyst. The initiative is over. The question isn't whether it failed — it's what specifically failed, why, and what the team should carry forward as judgment. This mode produces honest, actionable learning, not a defensive narrative.

**Use when:** A product launch, strategic bet, or major initiative concluded with outcomes materially below expectations. Before the next planning cycle. When the team needs shared understanding of what happened — without the usual politics.

**Do NOT use when:** The initiative is still in progress — use `premortem` before committing and `red-team` while planning. This mode is strictly retrospective.

---

## Required Outputs

- [ ] What actually happened (facts, not narrative)
- [ ] Root cause identified (deepest "why" — not the surface failure)
- [ ] Signals we could have known at decision time — named specifically
- [ ] The assumption that was most wrong named plainly
- [ ] Specific change to future decision-making (not "be more careful")

---

## Output Template

```
## Postmortem: [Initiative Name]
**Period:** [start] → [end]  **Outcome:** [what happened vs. what was expected]

### What Happened (Factual)
No narrative yet — just what occurred. Timeline of key events, decisions, and outcomes. Distinguish what we observed from what we inferred.

### Root Cause Analysis
Work backward from the failure. Use "why → why → why" until you hit a systemic cause, not a surface symptom. Specific enough to be actionable.

**Root cause:** [single sentence]

### What We Could Have Known
Signals that existed at decision time and were rationalized, ignored, or not looked for.

| Signal | When available | Why we missed it |
|--------|---------------|-----------------|
| [signal] | [timing] | [reason] |

### The Assumption That Was Wrong
The single assumption carrying the most weight in the original decision. The one that, if different, changes the outcome.

**The assumption:** [state it clearly]
**Why we held it:** [what made it feel solid]
**When it broke:** [specific moment]

### What Changes
Not "we'll be more careful." Concrete changes.
- **Decision change:** [what specifically changes in how we decide]
- **What we should have done instead:** [specific alternative]

### The Uncomfortable Truth
The thing the team knows but hasn't said explicitly. Say it.

### Postmortem Verdict
**Avoidable?** [Yes / Partially / No] — and the evidence
**Carry forward:** The single most transferable lesson for the next planning cycle
**Don't repeat:** The specific failure pattern to watch for next time
```
