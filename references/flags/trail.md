# `--trail` Flag

**Trigger:** `/cpo --trail` with no other prompt.

**What it does:** Reads the full decision journal and outputs a one-page strategic diary for the last 90 days — every decision, one line each.

```bash
_CUTOFF=$(date -v-90d +%Y-%m-%d 2>/dev/null || date -d "90 days ago" +%Y-%m-%d 2>/dev/null)
ls -t ~/.cpo/decisions/*.yaml 2>/dev/null | while read -r _f; do
  _FDATE=$(basename "$_f" | cut -c1-10)
  [ "$_FDATE" \< "$_CUTOFF" ] && continue
  echo "---"; cat "$_f"
done
```

Then output:

> **Decision Trail — last 90 days**
>
> | Date | Topic | Mode | Verdict | Path chosen |
> |------|-------|------|---------|-------------|
> | [date] | [prompt] | [mode] | [verdict] | [balanced/bold/conservative] |
> | ... | ... | ... | ... | ... |
>
> [N] decisions logged. [X] Go · [Y] No-Go · [Z] Conditional.

**If `--export` is also passed:** write the trail to `~/.cpo/exports/[date]-trail.md`.

**If NO_DECISIONS:** *"No decisions logged yet."*
