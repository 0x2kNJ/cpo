# Stack Detection & Workflow Handoffs

## Step 1 — Detect available skills (model-layer, run once per session)

On the first invocation, inspect your available skills/tools. Map each by **what workflow stage it serves**, not by name:

| If a skill's description mentions… | Maps to workflow stage |
|---|---|
| Architecture, plan review, engineering design, data flow | **Plan** |
| Code review, diff analysis, PR review, static analysis | **Review** |
| Deploy, ship, release, merge, push, PR creation | **Ship** |
| QA, testing, browser, screenshots, bug finding | **QA** |
| Retrospective, retro, team feedback, sprint review | **Retro** |
| Browse, navigate, web interaction, headless browser | **Browse** |

Build a silent map: `{ plan: "/plan-eng-review", review: "/review", ship: "/ship", qa: "/qa", retro: "/retro" }` — but use the **actual skill names** detected, not these examples. If a stage has no matching skill, mark it as `null`.

**Note on detection accuracy:** This mapping relies on parsing skill descriptions. Skills with unusual or terse descriptions may not map correctly — treat the map as best-effort, not authoritative. If a skill clearly belongs to a stage but its description doesn't match the keywords above, use judgment to assign it. When uncertain, leave it in the "OTHER DETECTED SKILLS" pool rather than force-fitting.

**Do not output this map.** Use it silently for handoffs, first-session discovery, and `--stack`.

## Step 2 — Workflow handoffs (after every major output)

Offer **one line** suggesting the next logical step. Offer once, at the end of the output, after the journal entry is written. Never repeat in the same session. Never force. **Only suggest skills that were actually detected in Step 1.** If the next logical skill isn't installed, skip the handoff — don't suggest what doesn't exist.

| After this output… | Suggest (if available) |
|---|---|
| Go/no-go verdict (clear Go) | **Plan** stage skill: *"Ready to plan the build? → `[detected plan skill]`"* |
| Go/no-go verdict (No-Go or Conditional) | No handoff — work stops here or needs more thinking |
| `--roadmap` (top bet decided) | **Plan** stage skill: *"Top bet locked? → `[detected plan skill]` to architect it"* |
| `--sell-up` (pitch produced) | **Plan** stage skill: *"Got the green light? → `[detected plan skill]` to start execution"* |
| `--outcome` (bet resolved, next bet ready) | Internal: *"Loop closed. Next bet? → `/cpo --roadmap` to re-stack"* |
| `--brief` (kill criterion approaching) | Internal: *"Need to act on this? → `/cpo [topic]` for full analysis"* |
| Boardroom / investor sim (post-debrief) | Internal: *"Prep materials next? → `/cpo board-story` or `/cpo investor-story`"* |

**Inbound handoffs (recognize workflow context):**

If the user mentions they just shipped, QA'd, or ran a retro — recognize it regardless of which specific skill they used:
- After shipping or QA: *"Shipped and verified? Run `/cpo --outcome [topic]` when you know if the bet paid off."*
- After a retro: *"Retro patterns worth feeding into strategy? Run `/cpo --roadmap` to re-prioritize."*

## Step 3 — First-session discovery (NO_CONTEXT + NO_DECISIONS only)

On the very first invocation ever (no saved state at all), append **one line** after the first output — nothing more. The user came for the analysis; give them one thing, not three:

> *First time here? Run `/cpo --stack` to see your full product workflow and what tools are available.*

Do not show the full stack diagram inline. Do not show the "no live data" offer on the same first output. Each of these surfaces on request or on the next relevant trigger. Show this line exactly once.

## `--stack` Flag Output

**Trigger:** `/cpo --stack`

Show the full detected workflow with coverage status. Unlike first-session discovery, this shows ALL stages including gaps:

```
PRODUCT STACK — detected skills
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

  Strategy     /cpo                    ✓ built-in
  Roadmap      /cpo --roadmap          ✓ built-in
  Sell-up      /cpo --sell-up          ✓ built-in
  Plan         [detected or "—"]       ✓ or ·
  Review       [detected or "—"]       ✓ or ·
  Ship         [detected or "—"]       ✓ or ·
  QA           [detected or "—"]       ✓ or ·
  Retro        [detected or "—"]       ✓ or ·
  Outcome      /cpo --outcome          ✓ built-in

Coverage: [N]/9 stages covered
```

If any stage is missing, add one line: *"Gaps in [stage names]. These stages work manually — the stack is strongest when all stages have skills."* No install instructions — just visibility.

## Discovering unknown complementary skills

If you detect a skill that doesn't fit any of the workflow stages above but seems potentially useful alongside `/cpo` (e.g., a design skill, a documentation skill, a scheduling skill), note it silently. When the user runs `--stack`, append a section:

```
OTHER DETECTED SKILLS (potentially complementary)
  [skill name]  — [1-line description of what it does]
  [skill name]  — [1-line description of what it does]
```

This surfaces skills the user has installed but may not know are relevant to their product workflow.
