# Flag: `--verify` — Consequence Verification Loop

**Purpose:** Close the feedback loop on past decisions. Surface consequence predictions past their check date. Turn the decision journal into a prediction market about your own judgment.

## Trigger

`/cpo --verify` or `/cpo --verify #decision-id`

## Behavior

**Step 1 — Load active decisions (reuse coherence pattern):**

```bash
_TODAY=$(date +%Y-%m-%d)
_ACTIVE_FILES=$(grep -rl "status: active" ~/.cpo/decisions/*.yaml 2>/dev/null | xargs ls -t 2>/dev/null)
echo "VERIFY_SCAN:"
for _f in $_ACTIVE_FILES; do echo "---"; cat "$_f"; done
```

In prose: scan loaded entries for consequences where `status: pending` AND `check_date <= today`. Build the due list.

If `#decision-id` is passed, filter to that decision only.

**Step 2 — Present verifications:**

**If 1-3 due consequences:** Present each via individual AskUserQuestion — fast enough to be inline.

**If 4+ due consequences:** Present a numbered summary, then walk through each one sequentially via AskUserQuestion (one per consequence). Each question takes 3 seconds to answer — keep it fast:

```
Consequence verification — [N] predictions due. Walking through each:
```

Each AskUserQuestion:
```
[X/N] From [decision slug] ([date]):
"[marker text]"
```
Options:
- **A) Confirmed** — prediction came true
- **B) Disconfirmed** — didn't happen or opposite occurred
- **C) Inconclusive** — push +30 days

**Step 3 — Update journal entries:**

For each verified consequence, update the specific consequence block in the YAML. Target the exact consequence by matching the `marker:` text, then update only that consequence's `status:` field. Never update the top-level `status:` or `outcome:` fields.

For Confirmed: set consequence `status: confirmed`. For Disconfirmed: set consequence `status: disconfirmed`. For Inconclusive: advance `check_date` by 30 days, keep `status: pending`.

**Step 4 — Summary:**

```
Verification complete:
· [N] confirmed · [N] disconfirmed · [N] pushed
· Prediction accuracy: [confirmed / (confirmed + disconfirmed)]%

[If accuracy < 50%]: Your predictions are running below 50% — consider whether your framework over-indexes on [most common Dominant Truth in disconfirmed decisions].
[If accuracy > 75%]: Strong calibration. Your [most common Dominant Truth] calls are particularly reliable.
```

## Integration with `--brief`

When `--brief` runs and 1-3 consequences are due: present the verification inline — don't redirect to `--verify`. Make it effortless. When 4+: add one line: *"[N] consequence predictions due — run `/cpo --verify` to batch-verify."*

## No output if nothing due

*"No consequences due for verification. Next check: [earliest upcoming check_date] for [decision summary]."*
