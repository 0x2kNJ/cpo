# Mode: `premortem` — Full Template

**Stance:** Future failure investigator. It's 12 months from now. The initiative failed. You're writing the post-mortem. Work backward.

**Use when:** Before committing to an irreversible decision. Before a major launch. When confidence is high — that's exactly when you need this.

**Do NOT use when:** In a retrospective on something that already failed — use `postmortem` instead.

---

## Required Outputs

- [ ] Single highest-probability cause of failure (not a list)
- [ ] Warning signs we're currently rationalizing — named specifically
- [ ] The highest-leverage decision that changed trajectory named
- [ ] Risk rating across 4 dimensions (Execution · Market · Org · Narrative)
- [ ] De-risking action with specific checkpoint for killing the initiative

---

## Output Template

```
## Premortem: [Initiative Name]

### Set the Scene
It's [12 / 18 / 24] months from now. [Initiative] has failed. Not partially — it's clearly, unambiguously not working. Leadership is discussing what went wrong. Write from that future moment.

### Most Likely Cause of Failure
Not a list. The single highest-probability cause. The one where, reading this back in 12 months, everyone nods and says "we knew this."

### Warning Signs We Ignored
What signals existed at decision time that we rationalized away? What would a skeptical outsider have flagged?

### Second-Order Failures
What else broke as a result? (Team morale, customer trust, opportunity cost, competitor advantage gained)

### The Highest-Leverage Decision We Got Wrong
The single decision, made before the failure point, that most changed the trajectory. Not the obvious one — the subtle one.

### Premortem Verdict
**Risk rating:** [Low / Medium / High / Critical] for each of: [Execution | Market | Organizational | Narrative]
**The assumption carrying the most weight:** [name it]
**To proceed with confidence, you must de-risk:** [specific action] within [timeframe]
**If that de-risking fails:** kill the initiative at [checkpoint], not after.
```
