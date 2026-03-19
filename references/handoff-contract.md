# Skill Handoff Contract — For Skill Authors

## Overview

The CPO Handoff Contract allows any skill to route a decision fork to `/cpo --decide`. CPO acts as the decision layer: it receives structured context from the calling skill, discovers what skills are installed, and recommends the best next action.

**When to use:** When your skill hits a fork that is fundamentally a product or strategy decision — not a technical execution question. Examples: go/no-go after QA finds a critical bug, release timing after a ship pre-check, priority call after a retrospective surfaces a recurring pattern.

---

## Contract Format

Before invoking `/cpo --decide`, your skill MUST emit this exact block:

```
**CPO Handoff Request**
From: [skill name — e.g., qa, ship, review, retro]
Context: [what the skill was doing — 1–3 sentences describing the situation]
Decision: [the specific fork that requires a product judgment — one sentence]
Options considered: [optional — list any options your skill already evaluated]
```

Then instruct the user to run `/cpo --decide` to proceed.

---

## Field Definitions

| Field | Required | Description |
|-------|----------|-------------|
| `From:` | Yes | The calling skill's name. Used by CPO to calibrate recommendation (e.g., a `qa` handoff signals quality risk; a `ship` handoff signals release timing). |
| `Context:` | Yes | What the skill was doing when it hit the fork. Include the relevant facts CPO needs to make a routing decision. 1–3 sentences. |
| `Decision:` | Yes | The exact fork in one sentence. Should be a genuine decision question, not a statement. Bad: *"We found a bug."* Good: *"Should we hotfix before the scheduled release or delay the release to investigate properly?"* |
| `Options considered:` | No | If your skill has already evaluated options, list them. CPO will incorporate them into routing. If omitted, CPO derives options from context. |

---

## What CPO Does With the Handoff

1. Reads the `CPO Handoff Request` block.
2. Runs silent skill discovery to map what tools are installed.
3. Matches the decision type to the best available skill or CPO capability.
4. Outputs a routing recommendation with one-line rationale.
5. For high-stakes handoffs (critical bugs, production issues, irreversible actions, revenue impact), appends a `Watch for:` line with a measurable threshold.
6. Asks: *"Want me to hand off now? (Reply Y or describe what you need instead)"*

---

## Implementation Example

Here is how `/qa` might implement the handoff contract when it finds a critical bug before release:

```markdown
**CPO Handoff Request**
From: qa
Context: QA found a critical authentication bypass in the login flow. The bug is reproducible on mobile Safari and affects ~30% of users based on our analytics breakdown. The scheduled release is in 2 hours.
Decision: Should we delay the release to fix the bug, or hotfix it post-release and proceed on schedule?
Options considered: A) Delay release (estimated 4–6 hours to patch + re-test), B) Release on schedule with a hotfix SLA, C) Feature-flag the affected flow and release without it
```

Then: *"Run `/cpo --decide` for routing guidance."*

---

## CPO-Aware Detection

A skill is considered `[✓ CPO-aware]` if its `SKILL.md` file contains either:
- A `CPO Handoff Request` block, OR
- A reference to `/cpo --decide`

CPO detects this automatically when `--stack` is run:
```bash
grep -rl "CPO Handoff Request\|cpo --decide" ~/.claude/skills/ ~/.claude/plugins/cache/ 2>/dev/null
```

Skills flagged as CPO-aware appear with `[✓ CPO-aware]` in the `--stack` output.

---

## Version Guard

The `--decide` flag requires CPO v1.4.4+. If a user has an older CPO version that does not include `--decide` in its flag list, CPO will output:

> *"CPO received a handoff request but `--decide` requires v1.4.4+. Best available guidance: [one-line manual recommendation based on context]."*

Skill authors do not need to handle this case — CPO handles the version check internally. Your skill should always emit the `CPO Handoff Request` block regardless of CPO version.

---

## Anti-Patterns

**Don't use handoffs for:**
- Technical questions with a clear right answer (use your skill's own judgment)
- User preference questions (ask the user directly)
- Decisions your skill is already equipped to make (don't outsource unnecessarily)

**Don't include:**
- Code snippets in the `Context:` field — summarize the technical situation in plain language
- More than 3 sentences in `Context:` — if it's longer, you're not summarizing
- Multiple decision questions in `Decision:` — pick the most important fork

---

## Checklist for Skill Authors

- [ ] `CPO Handoff Request` block is emitted before the user is told to run `/cpo --decide`
- [ ] `From:` field names your skill accurately
- [ ] `Context:` is 1–3 sentences of plain-language situation summary
- [ ] `Decision:` is a single, genuine decision question (not a statement)
- [ ] `SKILL.md` contains either the `CPO Handoff Request` template or a reference to `/cpo --decide` (for CPO-aware detection)
