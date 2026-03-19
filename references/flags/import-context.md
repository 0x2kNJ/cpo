# `--import-context` Flag

**Trigger:** `/cpo --import-context [path]` — import an external document into this project's `.claude/strategy/` directory so CPO can reference it as strategic context on future invocations.

**When to use:** You have a strategy doc, investor memo, pitch deck transcript, or context file saved outside this project that you want CPO to include in its strategic posture synthesis.

**Execution (prose-to-bash pattern):** Extract the source path from the user's prompt in prose. Then run a bash block with the literal path substituted:

```bash
_IMPORT_SOURCE="REPLACE_WITH_FULL_PATH"
_IMPORT_DATE=$(date +%Y-%m-%d)
_IMPORT_BASENAME=$(basename "$_IMPORT_SOURCE")
mkdir -p ".claude/strategy"
cp "$_IMPORT_SOURCE" ".claude/strategy/${_IMPORT_DATE}-${_IMPORT_BASENAME}"
echo "Imported: .claude/strategy/${_IMPORT_DATE}-${_IMPORT_BASENAME}"
```

Replace `REPLACE_WITH_FULL_PATH` with the actual extracted path, fully quoted (use `"$_IMPORT_SOURCE"` to handle spaces). After running, confirm: *"Imported [filename] → `.claude/strategy/[dated-name]`. I'll use it as strategic context starting next invocation."*

**Error handling:** If the file doesn't exist or `cp` fails, surface the error verbatim and ask the user to verify the path. Do not proceed silently.

**Size note:** If the imported file is >50,000 characters (~10,000 tokens), note: *"This file is large — I'll read the first 2,000 tokens per session."*
