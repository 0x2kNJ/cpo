# CPO Signals Contract v1

**For skill authors who want their skill to contribute to `/cpo --status`.**

## Overview

CPO's `--status` command produces an executive snapshot — one red line, one action, grounded in the founder's actual data. By default, it reads the decision journal, company context, strategy files, and git activity.

Skills can optionally contribute structured signals that enrich this synthesis. A QA skill might flag open regressions. A retro skill might report declining velocity. A review skill might surface unresolved findings.

**This is opt-in.** Skills that don't write signals are not penalized — `--status` works fine without them. Skills that do write signals make `--status` smarter.

## The contract

Drop a YAML file into `~/.cpo/signals/` when your skill runs. That's it.

### File naming

```
~/.cpo/signals/{skill-name}-latest.yaml
```

Use your skill's name as the prefix. Always overwrite with `-latest` — `--status` reads the most recent state, not a history. Examples:
- `~/.cpo/signals/retro-latest.yaml`
- `~/.cpo/signals/qa-latest.yaml`
- `~/.cpo/signals/review-latest.yaml`

### Schema (v1)

```yaml
schema_version: 1
skill: retro                    # your skill name (lowercase, no spaces)
timestamp: 2026-03-19T17:00:00  # ISO 8601, when the signal was generated
signal_type: velocity           # one of: velocity | quality | review
severity: yellow                # one of: green | yellow | red
summary: "3 commits this week, down from 12 last week"  # ≤20 words, plain English
raw: {}                         # optional — skill-specific structured data (--status ignores this; for future use)
```

### Field definitions

| Field | Required | Type | Description |
|:---|:---|:---|:---|
| `schema_version` | yes | integer | Always `1` for this contract version. `--status` ignores files with unrecognized versions. |
| `skill` | yes | string | Your skill name. Used for attribution in `--status --verbose`. |
| `timestamp` | yes | ISO 8601 | When the signal was generated. `--status` uses this for staleness checks. |
| `signal_type` | yes | enum | What kind of signal: `velocity` (shipping pace, commit trends), `quality` (bugs, regressions, test failures), or `review` (unresolved code review findings, architecture concerns). |
| `severity` | yes | enum | `green` (healthy — no action needed), `yellow` (notable — worth monitoring), `red` (urgent — potential red line candidate). |
| `summary` | yes | string | ≤20 words. Plain English. What a founder needs to know in one sentence. |
| `raw` | no | object | Arbitrary structured data specific to your skill. `--status` does not read this field in v1 — it's reserved for future use and for other tools that might consume signal files. |

### Signal types and how `--status` uses them

| Signal type | Feeds into synthesis step | Example |
|:---|:---|:---|
| `velocity` | Step 5 (ship-to-decision alignment) + Step 8a (git velocity) | Retro reports declining shipping pace |
| `quality` | Step 3 (kill criteria proximity) | QA finds 3 open regressions on the feature tied to a kill criterion |
| `review` | Step 4 (decision dependencies) | Code review surfaces an unresolved architecture concern blocking a ship |

### Severity and how `--status` uses it

| Severity | Standard mode | Verbose mode |
|:---|:---|:---|
| `red` | Becomes a red line candidate (priority 5 in the force-rank order) | Full attribution in synthesis trace |
| `yellow` | Modifies confidence of existing red line candidates | Noted with source |
| `green` | Ignored | Noted as positive signal |

### Staleness thresholds

`--status` drops stale signals silently in standard mode (flags them in verbose mode):

| Signal type | Stale after |
|:---|:---|
| `velocity` | 7 days |
| `quality` | 3 days |
| `review` | 1 day |

**Why?** Velocity trends are slow-moving (weekly is fine). Quality issues are urgent (3 days means it's probably been triaged or resolved). Review findings are ephemeral (code changes daily — a 2-day-old review finding may already be addressed).

## Writing a signal (example)

At the end of your skill's execution, write:

```bash
mkdir -p ~/.cpo/signals
cat > ~/.cpo/signals/retro-latest.yaml << 'EOF'
schema_version: 1
skill: retro
timestamp: 2026-03-19T17:00:00
signal_type: velocity
severity: yellow
summary: "3 commits this week, down from 12 last week"
raw:
  commits_this_week: 3
  commits_last_week: 12
  top_contributor: "founder"
  stale_prs: 2
EOF
```

## Discovery

When a founder runs `/cpo --stack`, skills that write signals are shown with their signal type:

```
Retro  /retro  ✓  [signals: velocity]
QA     /qa     ✓  [no signals]
```

This creates visibility into which skills enrich `--status` and which don't — without pressuring skill authors.

## Rules for skill authors

1. **Always overwrite with `-latest`.** `--status` reads current state, not history.
2. **Always include `schema_version: 1`.** Future versions may change the schema.
3. **Keep `summary` under 20 words.** It may appear in a 120–150 word output.
4. **Be honest about severity.** `red` means a founder should potentially change course. Don't cry wolf.
5. **Write silently.** Don't announce the signal write to the user — it's infrastructure.
6. **Graceful if `~/.cpo/` doesn't exist.** `mkdir -p` before writing.
