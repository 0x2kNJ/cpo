# `--save-context` Flag / Company Context Bootstrap

**Trigger:** `/cpo --save-context` explicitly passed, OR when enough answers have emerged to fill all 5 fields after the first analysis.

**When this runs:** Only on explicit `--save-context`, or after first analysis reveals all 5 fields. Never interrupt the first interaction. Never run if any `CONTEXT_LOADED_*`. Never run if context was established earlier in this session.

**Trigger guard — check before asking anything:**
1. Any `CONTEXT_LOADED_*`? → Skip. Use loaded context.
2. Follow-up in ongoing session? → Skip. Use established context.
3. `--save-context` passed? → Run immediately, collect all 5 fields, save.
4. First interaction? → Infer from prompt first, ask ONE question max. Infer remaining fields mid-analysis. Do not ask for them — only `--save-context` triggers full collection.

**Collection:** One field per AskUserQuestion: Stage → Business model → Core constraint → Top 3 priorities → Biggest open question.

After all 5, save:

```bash
mkdir -p ~/.cpo
cat > ~/.cpo/context.md << 'EOF'
# Company Context
Stage: [answer]
Business model: [answer]
Core constraint: [answer]
Top priorities: [answer]
Open question: [answer]
Last updated: [date]
EOF
echo "$_SKILL_VERSION" > ~/.cpo/.version
echo "Context saved. Will load automatically next session."
```

**To update:** run `/cpo --save-context` or edit `~/.cpo/context.md` directly.
