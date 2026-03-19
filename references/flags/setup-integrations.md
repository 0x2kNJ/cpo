# `--setup-integrations` Flag

**Trigger:** `/cpo --setup-integrations`

**What it does:** Detects available data sources — first via MCP tools (live), then via `~/.cpo/integrations.md` (static fallback). No fixed integration list. Works with whatever tools are actually connected.

**Step 1 — MCP detection (model-layer):**

Inspect your available tools. Map by what they return, not by name:

| If a tool returns… | Maps to Five Truth |
|---|---|
| Financial, billing, revenue, subscription data | Economic Truth |
| User behavior, analytics, retention, funnel data | User Truth |
| Code activity, deployments, incidents, PRs | Execution Truth |
| Project status, OKRs, roadmap, team updates | Context for all Truths — infer per field |

For each tool detected, surface it explicitly: *"I see [tool name] connected — that looks like [Truth] data. Want me to pull [specific metric]?"*

If multiple tools map to the same Truth, use all of them and label each data point with its source.

If a single tool returns data that spans multiple Truth types (e.g., revenue AND retention in one response), split it: inject each metric under the Truth it belongs to and label individually. Never aggregate cross-Truth data into a single block.

**Step 2 — Static fallback (if no MCP data sources detected):**

Tell the user: *"No live data sources detected in this session. I can work from manually pasted metrics instead."*

Guide them to export their key numbers and paste them. Then save as structured Markdown:

```bash
mkdir -p ~/.cpo
cat > ~/.cpo/integrations.md << 'EOF'
# CPO Integrations
Last updated: REPLACE_WITH_DATE

## REPLACE_WITH_SOURCE_NAME [REPLACE_WITH_TRUTH: Economic / User / Execution / All]
REPLACE_WITH_METRIC_NAME: REPLACE_WITH_VALUE
REPLACE_WITH_METRIC_NAME: REPLACE_WITH_VALUE

## REPLACE_WITH_SOURCE_NAME [REPLACE_WITH_TRUTH]
REPLACE_WITH_METRIC_NAME: REPLACE_WITH_VALUE
EOF
echo "Integrations saved to ~/.cpo/integrations.md"
```

Add as many sections as needed — one per data source. The model will map each section to the correct Truth on read.

**Data freshness rule:** If `Last updated` in the file is >7 days old, flag at the top of Five Truths assessment: *"[Integration data from [date] — may be stale. Run `--setup-integrations` to refresh.]"*

**Updating:** Re-run `--setup-integrations` to re-detect MCP tools, or edit `~/.cpo/integrations.md` directly.
