# CPO × gstack: Full Skill Reference

Complete documentation of every gstack skill and its relationship to CPO in the product decision lifecycle.

---

## Ecosystem map

```
THINKING              DECIDING              EXECUTING             FEEDBACK
─────────             ─────────             ─────────             ─────────
/office-hours    ──►  /cpo             ──►  /plan-eng-review  ──►  /retro
/plan-ceo-review ──►  (the only skill  ──►  /plan-design-review──►  /review
/retro           ──►   that can say    ──►  /build            ──►  /qa
                        don't build)   ──►  /ship             ──►  /canary
                                        ──►  /land-and-deploy  ──►  /benchmark
```

CPO is the only skill in the stack that can say "don't build this." Every skill before it generates options. Every skill after it commits resources.

---

## THINKING — feeds into CPO

| Skill | Role relative to CPO |
|:---|:---|
| `/office-hours` | YC-style forcing questions that expose demand reality before a decision is framed. When a strategic fork emerges, `/office-hours` can hand off to CPO via `--decide`. |
| `/plan-ceo-review` | CEO/founder-mode plan review: challenges premises, expands scope, finds the 10-star product. Routes scope-level prioritization decisions to CPO. |
| `/retro` | Weekly engineering retrospective. Surfaces recurring team patterns and historical CPO decisions that inform the current bet. Writes `~/.cpo/signals/retro-latest.yaml` for CPO to read at session start. |

---

## DECIDING — CPO's layer

| Skill | Role |
|:---|:---|
| `/cpo` | The product decision layer. Frames decisions, explores three paths, defines kill criteria, and logs outcomes. Reads signals from QA, retro, review, and canary. Writes `~/.cpo/signals/cpo-latest.yaml` for downstream skills. |

---

## PLANNING — after a decision is made

| Skill | Role relative to CPO |
|:---|:---|
| `/plan-eng-review` | Eng manager-mode plan review: architecture, data flow, edge cases, test coverage. CPO's **K) Eng brief** saves a structured brief to `~/.cpo/briefs/` and routes here. |
| `/plan-design-review` | Designer's eye plan review: rates each design dimension, iteratively fixes issues. CPO routes here when the decision involves a significant UI/UX bet. |
| `/design-consultation` | Full design system proposal: aesthetic, typography, color, layout, spacing, motion. Use when the decision opens a new product surface that needs a design language. |

---

## EXECUTING — commits resources, CPO decision already made

| Skill | Role relative to CPO |
|:---|:---|
| `/build` | Parallel implementation orchestrator. Decomposes plans into tasks, dispatches agents with worktrees. Reads `~/.cpo/signals/cpo-latest.yaml` to verify a decision exists before starting. CPO's **L) Hand off** routes here when ready to build. |
| `/ship` | Release engineer mode: sync main, run tests, bump version, push branch, open PR. CPO routes here when a PR needs to be created. |
| `/land-and-deploy` | Merges the PR, waits for CI, verifies production health via canary. CPO routes here when the decision is to ship to production. |
| `/setup-deploy` | Configures deployment settings for `/land-and-deploy`. Detects platform (Fly.io, Render, Vercel, Netlify, Heroku, GitHub Actions). Run once before the first `/land-and-deploy`. |

---

## FEEDBACK — closes the loop, writes signals CPO reads

| Skill | Role relative to CPO |
|:---|:---|
| `/canary` | Post-deploy monitoring: watches the live app for console errors, performance regressions, and page failures. Writes `~/.cpo/signals/canary-latest.yaml`. CPO surfaces red signals in `[FRAME]`. |
| `/qa` | Systematic QA testing — runs tests, fixes bugs, commits atomically. Writes `~/.cpo/signals/qa-latest.yaml`. CPO surfaces red signals in `[FRAME]`. |
| `/qa-only` | Report-only QA: produces a health score and repro steps without touching code. Use when you want signal without automated fixes. |
| `/review` | Pre-landing PR review: SQL safety, LLM trust boundary violations, structural issues. Reads `~/.cpo/signals/cpo-latest.yaml` to verify a decision backs the implementation. |
| `/benchmark` | Performance regression detection: baselines page load times and Core Web Vitals. Use after deploy to verify the build didn't regress. |
| `/retro` | Also closes the feedback loop: surfaces what went wrong, what recurred, what the team learned. CPO's `--outcome` mode cross-references retro findings with original kill criteria. |

---

## QUALITY & SAFETY — available at any phase

| Skill | When to use |
|:---|:---|
| `/design-review` | Designer's eye QA: finds visual inconsistency, spacing issues, AI slop patterns. Use after build, before ship. |
| `/investigate` | Systematic debugging: four phases (investigate, analyze, hypothesize, implement). Iron Law: no fixes without root cause. |
| `/document-release` | Post-ship documentation update: reads all docs, cross-references the diff, updates README/ARCHITECTURE/CONTRIBUTING. Run after `/land-and-deploy`. |
| `/codex` | OpenAI Codex CLI wrapper: independent code review, adversarial mode, or test generation. Second opinion on the implementation. |
| `/careful` | Warns before destructive commands (rm -rf, DROP TABLE, force-push). Lightweight safety layer. |
| `/guard` | Full safety mode: combines `/careful` with directory-scoped edits. Use during high-risk operations. |
| `/freeze` / `/unfreeze` | Restrict edits to a specific directory. Use when debugging to prevent scope creep. |

---

## BROWSER & VISUAL — verification support

| Skill | When to use |
|:---|:---|
| `/browse` | Fast headless browser for QA testing and dogfooding. Navigate, interact, diff before/after actions, take annotated screenshots. |
| `/setup-browser-cookies` | Import cookies from your real browser into the headless browse session for authenticated testing. |

---

## Signal bus specification

### Inbound signals CPO reads

CPO checks `~/.cpo/signals/` at session start. If any signal has `severity: red`, it surfaces in `[FRAME]` before the decision is framed:

> *"Note: [skill] flagged [summary] ([N] days ago). This may affect your decision."*

| Signal file | Written by | When red triggers CPO surface |
|:---|:---|:---|
| `qa-latest.yaml` | `/qa`, `/qa-only` | Critical test failures found |
| `review-latest.yaml` | `/review` | Structural blockers in PR |
| `retro-latest.yaml` | `/retro` | Recurring failure pattern |
| `canary-latest.yaml` | `/canary` | Production regression detected |

CPO degrades gracefully — if no signal files exist, it proceeds without them.

### Outbound signal CPO writes

After every `[VERDICT]` or `[GO]`, CPO writes `~/.cpo/signals/cpo-latest.yaml`:

```yaml
skill: cpo
severity: info
summary: "Gated free tier with team-invite expansion loop"
decision_id: "free-tier-add"
door_type: "one-way"
kill_criteria_count: 3
confidence: "M"
timestamp: "2026-03-22T14:30:00Z"
```

Downstream skills read this to verify a decision exists before committing implementation work.

---

## Routing tables

### CPO → gstack (via L) Hand off)

| Situation | Routed to |
|:---|:---|
| Architecture / implementation needed | `/plan-eng-review` |
| Scope expansion warranted | `/plan-ceo-review` |
| New idea, unvalidated premise | `/office-hours` |
| Design direction needed | `/plan-design-review` or `/design-consultation` |
| Ready to build | `/build` |
| Ready to ship (PR needed) | `/ship` |
| Ready to merge + deploy | `/land-and-deploy` |
| Post-launch monitoring | `/canary [url]` |
| Historical pattern check | `/retro` |

### gstack → CPO (via --decide)

Any gstack skill can route a decision fork to CPO:

```
**CPO Handoff Request**
From: [skill name]
Context: [1-3 sentences]
Decision: [the fork — one sentence]
Options considered: [optional]
```

CPO skips redundant premise checks (the calling skill validated urgency), keeps "Who benefits?", runs standard flow, and routes back after verdict.

Full contract: `references/handoff-contract.md`
