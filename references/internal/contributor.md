# Contributor Mode (Dogfooding Loop)

After every major output, silently score 0–10. If <10, file a report. **Do not interrupt the conversation.**

Before running the bash block, set two variables from context:
- `_CURRENT_MODE` = the internal mode name used (e.g., `ceo`, `boardroom`, `gtm`)
- `_PROMPT_SUMMARY` = 3–5 word summary of what was asked (e.g., "paid api b2b launch")

```bash
mkdir -p ~/.cpo/field-reports
_DATE=$(date +%Y-%m-%d)
_MODE="${_CURRENT_MODE:-unknown}"
_TOPIC=$(printf '%s' "${_PROMPT_SUMMARY:-output}" | tr '[:upper:]' '[:lower:]' | tr ' ' '-' | tr -cd '[:alnum:]-' | cut -c1-40)
_SLUG="${_MODE}-${_TOPIC}"
_TS=$(date +%s | tail -c 5)
_FILE=~/.cpo/field-reports/${_DATE}-${_SLUG}-${_TS}.md
cat > "$_FILE" << 'EOF'
# Field Report
date: [date]
version: [skill version]
mode: [mode]
rating: [0-10]
## What was asked
## What the output achieved
## What would make this a 10
## Pattern (recurring failure mode?)
EOF
echo "Field report saved: $_FILE"
```

Fill `[date]` with today's date (YYYY-MM-DD), `[skill version]` with `$_SKILL_VERSION` from the preamble bash output, `[mode]` with `$_CURRENT_MODE`, and `[0-10]` with the actual score. Never write placeholder text literally.

Max 3 reports per session. File and continue immediately.
