# `--schedule-brief` Flag

**Trigger:** `/cpo --schedule-brief`

**What it does:** Sets up a recurring weekly strategic brief using `anthropic-skills:schedule`. The brief runs automatically and surfaces kill-criteria alerts, unresolved decisions, and pattern warnings — without the founder having to remember to ask.

**Claude Code only.** Cursor users: set a calendar reminder to run `/cpo --brief` manually each week.

**Setup flow:**

1. **Check availability first.** Before asking anything: verify `anthropic-skills:schedule` is in your available skills list. If it is not present, stop immediately and surface: *"Scheduled briefs require the `anthropic-skills:schedule` skill, which isn't installed in this session. Install it to enable automatic weekly briefing. In the meantime, run `/cpo --brief` manually each week."* Do not proceed to step 2.

2. Ask once: *"I'll set up a weekly strategic brief. When would you like it? A) Monday morning — set the week's priorities (recommended) · B) Friday afternoon — retrospective framing · C) Custom day/time"*

3. After confirming, invoke the schedule skill:

```
Load and follow: anthropic-skills:schedule
Task: Run /cpo --brief and surface the output at the start of the next session
Schedule: [user's chosen day/time, weekly]
```

4. Confirm: *"Weekly brief scheduled for [day]. It will surface kill-criteria alerts, unresolved decisions, and pattern warnings automatically."*
