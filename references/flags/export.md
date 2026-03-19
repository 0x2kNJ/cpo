# `--export` Flag Behavior

**Trigger:** When `--export` is passed alongside any other command.

**What it does:** After producing any output, writes the full output as a formatted Markdown file to `~/.cpo/exports/`. The file is clean Markdown — paste into Notion, email to your board, share in Slack. No terminal formatting, no ANSI, no prompts.

**Execution:**

1. Construct a slug from the mode and topic (same logic as field reports).
2. Write the full output as a formatted Markdown file.

Before running: set `_CURRENT_MODE` and `_PROMPT_SUMMARY` (same as Decision Journal and Contributor Mode).

```bash
mkdir -p ~/.cpo/exports
_DATE=$(date +%Y-%m-%d)
_TS=$(date +%s | tail -c 5)
_MODE="${_CURRENT_MODE:-unknown}"
_TOPIC=$(printf '%s' "${_PROMPT_SUMMARY:-output}" | tr '[:upper:]' '[:lower:]' | tr ' ' '-' | tr -cd '[:alnum:]-' | cut -c1-40)
_EXPORT_FILE=~/.cpo/exports/${_DATE}-${_MODE}-${_TOPIC}-${_TS}.md
cat > "$_EXPORT_FILE" << 'EOF'
---
date: REPLACE_WITH_DATE
mode: REPLACE_WITH_MODE
company: REPLACE_WITH_STAGE_OR_UNKNOWN
---

# REPLACE_WITH_TOPIC — REPLACE_WITH_VERDICT_OR_MODE_LABEL

REPLACE_WITH_FULL_OUTPUT_FORMATTED_FOR_HUMAN_READING
EOF
echo "Exported: $_EXPORT_FILE"
```

Replace every `REPLACE_WITH_*` value with actual content. Do not write literal placeholder text.

3. Confirm at the end of the response: *"Export saved: `~/.cpo/exports/[filename]`"*

**For simulation transcripts (`boardroom`, `investor-roundtable`):** The transcript write (see mode stubs) always happens regardless of `--export`. `--export` is redundant for simulations but harmless.

**Combinable with `--roadmap` and `--sell-up`:** The export file is the full output of whichever flag produced it — paste directly into a doc or Notion page.
