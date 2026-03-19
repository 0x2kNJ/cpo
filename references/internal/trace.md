# Execution Trace — Full Checkpoint Spec

A hidden, machine-readable checkpoint log that fires at each spec-mandated step. Verifies spec compliance at runtime — not output quality (that's Contributor Mode), but whether the model followed the required flow.

**Trace file:** Written once per invocation to `~/.cpo/.trace/`. Never shown to the user.

```bash
mkdir -p ~/.cpo/.trace
_TRACE_FILE=~/.cpo/.trace/$(date +%Y-%m-%d-%s).json
echo '{"checkpoints":[]}' > "$_TRACE_FILE"
```

**Checkpoints — append one JSON object per step:**

| Step | Fires after | Required fields |
|------|-------------|-----------------|
| `preamble` | Bash block completes | `context_state`, `decisions_state`, `integrations`, `decision_object` |
| `route` | Mode determined | `mode`, `flags[]`, `role`, `is_returning_decision` |
| `frame` | Action 1 output | `decision_statement`, `inferences[]`, `corrections_invited` |
| `grounding` | Grounding question delivered | `options_count`, `options_are_risk_tolerances` (must be `false`), `recommendation_present` |
| `paths` | Action 3 output | `path_count` (must be `3`), `recommended_path`, `framing_sentence_present` |
| `verdict` | Action 4 output | `chosen_path`, `kill_criteria_count` (MUST be >= 3), `kill_criteria_are_measurable` (MUST be true — each criterion must name a metric, a threshold, and a timeframe), `confidence`, `elevation_loop_triggered`, `d_i_menu_blocked_until: kill_criteria_count >= 3` |
| `journal` | Decision journal written | `file_written`, `decision_id`, `revision`, `has_placeholders` (must be `false`) |

**Write the trace silently.** Do not announce it. Do not interrupt. If a self-check assertion fails, fix the output before delivering — do not surface the trace to the user.
