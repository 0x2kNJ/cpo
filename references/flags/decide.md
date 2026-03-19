# `--decide` Flag — Full Spec

**Purpose:** Inbound handoff from another skill. CPO acts as the decision layer: receives context from the calling skill, discovers what's available on the system, and routes to the best next action. Any installed skill can invoke CPO at a decision fork. CPO is never a dead end.

**Execute immediately** — no four-action flow. This is a routing decision, not a product analysis.

---

## Skill Handoff Contract

To invoke CPO from another skill, emit this block then call `/cpo --decide`:

```
**CPO Handoff Request**
From: [skill name — e.g., /ship, /qa, /review]
Context: [what we were doing — 1-3 sentences]
Decision: [what fork we hit — one sentence]
Options considered: [optional — what the calling skill identified as candidates]
```

**Example (from /qa after finding a critical regression):**
```
**CPO Handoff Request**
From: /qa
Context: QA pass complete on the pricing-redesign branch. Found a critical regression in the free-tier upgrade flow — 3 of 5 test scenarios fail.
Decision: Do we block the release, ship with a flag, or escalate for immediate hotfix?
Options considered: block release / ship behind feature flag / escalate to eng lead
```

---

## Discovery (silent, runs on trigger)

```bash
# Tier 1 — gstack skills
which ship qa review plan-eng-review plan-cpo-review retro 2>/dev/null

# Tier 2 — local custom skills
ls ~/.claude/skills/ 2>/dev/null

# Tier 3 — plugin cache skills
ls ~/.claude/plugins/cache/ 2>/dev/null
```

Map discovered skills to capability:

| Skill | Capability |
|-------|-----------|
| `ship` | Deployment, PR creation, release |
| `qa` | Quality assurance, regression testing |
| `review` | Code safety, staff engineer review |
| `plan-eng-review` | Architecture decisions, technical design |
| `plan-cpo-review` | Strategic/founder-level rethink |
| `retro` | Post-incident review, team reflection |
| Any skill in `~/.claude/skills/` | Read SKILL.md description to infer capability |

---

## Decision Logic

1. Parse the handoff context — understand what was happening and what fork was hit
2. Run discovery (silent)
3. **Match need to capability** using product judgment (not keyword matching). Weight factors: decision stakes (high stakes = recommend deeper analysis), time sensitivity (urgent = fastest available path), decision type (technical → review/plan-eng; strategic → plan-cpo; quality → qa; release → ship)
4. Select the best available skill
5. Deliver recommendation immediately — no four-action flow, no grounding questions

---

## Output Format

**When ideal skill is available:**
```
**CPO → [From] decision**
Given [context in one clause], the right next step is:

→ **[skill]** — [one-line reason why this skill, not another]

[Watch for: [one measurable threshold] — if this triggers, escalate before proceeding.]

Want me to hand off now? (Reply Y or describe what you need instead)
```

**"Watch for" rule:** Renders when the handoff context signals high stakes (critical bug, production issue, irreversible action, revenue impact). Omit for routine/reversible decisions.

**When ideal skill is NOT installed:**
```
**CPO → [From] decision**
Given [context in one clause], the ideal next step is [missing skill] — [why].

[missing skill] isn't installed. → Install via: [source/command]

Best available now: **[alternative skill]** — [why it's the right fallback given what's installed]

Want to proceed with [alternative]? (Reply Y, or install [missing skill] and re-run)
```

**When no suitable skill is available:**
```
**CPO → [From] decision**
Given [context], you'd benefit from [category of skill] — none are installed.

Recommended installs:
1. [skill] — [what it does, where to get it]
2. [skill] — [what it does, where to get it]

In the meantime: [specific manual action the user can take right now — never leave them stranded]
```

---

## Install Source Map

| Skill | Install |
|-------|---------|
| gstack skills (ship, qa, review, etc.) | `github.com/0x2kNJ/gstack` or gstack plugin |
| anthropic-skills | Claude Code plugin registry |
| Superpowers skills | Claude Code plugin registry |
| Custom skills | Add SKILL.md to `~/.claude/skills/[name]/` |

---

## Rules

- **Execute immediately** — no four-action flow. This is a routing decision, not a product analysis.
- **Never leave the user stranded** — always end with a concrete next action, even if no skill matches.
- **One recommendation** — don't list options. Pick the best one. If it's unavailable, name the best fallback.
- **Context-aware routing** — the recommendation must reflect the specific decision context, not generic suggestions.
- **Respect the calling skill** — don't dismiss what the calling skill surfaced. If `/qa` found a critical bug, don't recommend `/ship`.
- **Version guard:** If CPO receives a `CPO Handoff Request` block but `--decide` is not in its flag list (older version), output: *"CPO received a handoff request but `--decide` requires v1.4.4+. Best available guidance: [one-line manual recommendation based on context]."* Never silently ignore a handoff request.
