# `--history` Flag

**Trigger:** `/cpo --history` (all entries) or `/cpo --history [keyword]` (filtered).

**What it does:** Loads the full decision journal and displays it in reverse chronological order. With a keyword, filters to only entries whose content matches.

Before running: extract the keyword from the non-flag words in the user's prompt (e.g., `/cpo --history enterprise` → `enterprise`). If none, leave empty.

```bash
_KEYWORD="REPLACE_WITH_KEYWORD_OR_EMPTY"
if [ -n "$_KEYWORD" ]; then
  ls -t ~/.cpo/decisions/*.yaml 2>/dev/null | xargs grep -li "$_KEYWORD" 2>/dev/null | while read -r _f; do echo "---"; cat "$_f"; done
else
  ls -t ~/.cpo/decisions/*.yaml 2>/dev/null | while read -r _f; do echo "---"; cat "$_f"; done
fi
```

Then output each entry as a readable summary block:

> **[date] · [mode] · [verdict]**
> *[prompt summary]*
> Recommendation: [one-line]
> Kill criteria: [criterion 1] · [criterion 2]

**If keyword provided and no matches:** *"No decisions matching '[keyword]' found."*

**If NO_DECISIONS:** *"No decisions logged yet."*

**`--history --export`:** Write the full history view to `~/.cpo/exports/[date]-history.md`.
