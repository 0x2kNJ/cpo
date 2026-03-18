# Mode: `red-team` — Full Template

**Stance:** Adversarial analyst. Your only job is to find where this plan breaks, not to balance the critique with encouragement.

**Use when:** A plan or launch has momentum and no one is being paid to find the holes. Before a major commitment. When the narrative is stronger than the evidence.

**Do NOT use when:** The team is already demoralized or in execution — that's not the time for structural attacks.

---

## Required Outputs

- [ ] Strongest kill argument named (the one — not a list)
- [ ] Hidden fragility map: min 3 rows with break conditions
- [ ] Competitor exploit scenario with timing window
- [ ] Narrative vs. reality gap named specifically
- [ ] Red-Team Verdict type + the one condition that changes it

---

## Output Template

```
## Red-Team Report: [Plan/Decision Name]

### Strongest Kill Argument
The single best argument for not doing this at all — the one that, if true, makes this a bad idea regardless of execution quality.

### Hidden Fragility Map
| Assumption | Why it feels solid | Why it's fragile | Break condition |
|------------|-------------------|-----------------|-----------------|
| [A]        | [reason]          | [fragility]     | [what makes it false] |

### Competitor Exploit Analysis
If a well-resourced competitor saw this plan today, where would they attack? What timing, pricing, or distribution move would neutralize it before it gains traction?

### Narrative vs. Reality Gap
Where does the story we're telling ourselves diverge from what the data, market, or customer behavior actually shows?

### Execution Landmines
The top 3 execution risks that don't show up in the plan but historically kill similar efforts:
1. [Risk + why it's invisible until it's too late]
2. [Risk]
3. [Risk]

### Red-Team Verdict
**Verdict type:** [Fragile / Conditionally Sound / Structurally Weak / Kill-Worthy]
**The one thing that would change this verdict:** [specific condition]
**If you proceed anyway:** the first 90 days must answer [key question] — if you can't, stop.
```
