# CPO Skill — Changelog

All notable changes documented here. Follows [Keep a Changelog](https://keepachangelog.com/en/1.0.0/) conventions.

---

## [1.0.0] — 2026-03-17

First public release.

### Added

**Core advisor framework**
- 20 modes covering the full product lifecycle: `blue-ocean`, `ceo`, `sequence`, `gtm`, `discovery`, `narrative`, `launch-os`, `investor-story`, `red-team`, `premortem`, `postmortem`, `org-design`, `board-memo`, `board-story`, `eng-brief`, `eng-translate`, `advisory-roundtable`, `boardroom`, `investor-roundtable`, `upward-pitch`
- Five Truths assessment framework (User · Strategic · Economic · Macro-Political · Execution)
- Three Paths on every recommendation (Bold · Balanced · Conservative) with kill criteria
- Evidence labeling on all outputs (*fact / assumption / inference / judgment*)
- Stage-aware doctrine (pre-PMF / post-PMF / Series B+)
- Role detection — decision-maker vs. influencer framing

**Decision journal**
- Persistent YAML journal at `~/.cpo/decisions/` — auto-written after every major analysis
- `--brief` — proactive weekly intelligence brief: kill criteria alerts, unresolved decisions, pattern warnings
- `--trail` — 90-day strategic diary view
- `--history` — full journal with keyword filtering
- `--since` — temporal delta mode: diff current analysis against a prior baseline
- `--outcome` — close the loop on a prior decision, detect Bold/Balanced/Conservative path patterns

**Roadmap & prioritization**
- `--roadmap` — comparative prioritization across N bets: compressed Five Truths per bet, stage-aware scoring (3 dims pre-PMF, 6 dims post-PMF), dependency mapping, bias detection, ASCII output with kill criteria per bet

**Pitch-building**
- `--sell-up [audience]` — reframes any decision as an internal pitch for CEO, board, eng-lead, or custom audience. Produces 1-minute elevator pitch, 5-minute meeting version, objection pre-emption, concession strategy, and explicit ask

**Simulations**
- `boardroom` — live multi-turn board simulation with governance dynamics, per-member sentiment, post-meeting debrief, transcript saved to `~/.cpo/simulations/`
- `investor-roundtable` — live 5-round investor debate, Tier 1 + Tier 2 panel, M&A framing, transcript saved

**Integrations & exports**
- `--setup-integrations` — detect MCP data sources, map to Five Truths, static fallback via `~/.cpo/integrations.md`
- `--export` — write any output to `~/.cpo/exports/` as shareable Markdown
- `--schedule-brief` — set up recurring weekly brief via `anthropic-skills:schedule`

**Stack & discovery**
- `--stack` — show full product workflow with coverage status, detect complementary skills by description keywords
- Workflow handoffs — after every major output, suggest the logical next step (once per session, only if skill is installed)
- First-session discovery — on first invocation with no state, surface `--stack` in one line

**Persistence**
- Company context at `~/.cpo/context.md` — loads automatically each session
- `--save-context` — full bootstrap to collect and save all 5 context fields
- `--context [name]` — named context switching for multiple companies
- Version tracking at `~/.cpo/.version` — surfaces mismatches between skill and saved state

**Cursor edition**
- `CURSOR.md` — full Cursor-compatible version: same bash blocks, same `~/.cpo/` state, `cat`-based mode loading, plain question format replacing `AskUserQuestion` tool
- Shared decision journal between Claude Code and Cursor environments
