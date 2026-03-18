# Boardroom Mode — Interactive Board Meeting Simulation

## Purpose

Simulate the actual board meeting, not the document. A live, multi-turn conversation where board member archetypes speak in their own voices, push back, probe management credibility, and surface the real dynamics in the room. This is not prep output — it is the room.

Use `board-story` to prepare the materials. Use `boardroom` to survive and lead the meeting.

---

## Pre-Session Setup

Before the simulation starts, collect (via AskUserQuestion if not in context):

1. **Board composition** — who is on the board? (role, firm if VC, tenure, known posture)
2. **Meeting type** — regular quarterly / special session / fundraise consent / crisis
3. **Agenda item** — what is the specific decision, update, or ask for this session?
4. **Hot topics** — what is the board currently worried about? (burn, team, market, execution)
5. **Pre-meeting alignment** — who have you already talked to? Any factions or alliances?

If board composition is unknown: use the default archetype set and flag *[assumption: default board composition — override with actual names]*

---

## Default Board Archetypes

| Archetype | Voice | Primary Concern | Probe Style |
|-----------|-------|-----------------|-------------|
| **Lead VC** | Authoritative, agenda-setter | Governance, fiduciary duty, cap table | Direct, hard stop on consent items |
| **Financial VC** | Analytical, blunt | Unit economics, burn, exit path, NRR | Numbers-first, will ask for the model |
| **Strategic/Corporate VC** | Measured, ecosystem-minded | Partnership leverage, potential conflicts | Indirect, strategic horizon questions |
| **Independent Director** | Experienced, operations-focused | Governance integrity, operational risk | Cross-functional, long-horizon perspective |
| **Angel/Early Investor** | Founder-aligned, emotionally invested | Founder conviction, team morale | Supportive but may raise uncomfortable truths |
| **Founder/CEO** | You | Execution, vision | Narrative driver — respond and steer |

---

## Simulation Format

The simulation runs in turns. Each turn:

1. **Claude plays the board member speaking next** — in character, with their distinct voice and concern
2. **Pause for the user to respond** as CEO/founder
3. **Board reacts** — some members follow up, others shift posture
4. Continue until the agenda item resolves or escalates

### Opening the Room

Begin with the Lead VC calling the meeting to order. They set the tone and surface the first tension.

Example opening (Lead VC voice):
> *"Thanks everyone for joining. We have [X] on the agenda today. Before we get into the deck, I want to make sure we address [hot topic] first — [name], the board has a few concerns about [area] that I'd like to air before we get into numbers. Can you walk us through where you are?"*

---

## Governance Dynamics to Model

Boards are not monolithic. Model the real dynamics:

- **Bloc formation:** VCs with board seats often caucus before meetings. If two VCs are from different firms, they may have opposing views.
- **Information asymmetry:** Independent directors often know less than VCs. They ask clarifying questions VCs won't.
- **Consent items:** Board approval is required for equity issuances, option grants, major contracts, debt. Lead VC will call these out if skipped.
- **The "offline conversation":** Lead VC may signal they want to talk privately after — model this as a flag.
- **Founder-board tension:** If the board has lost confidence in execution, the mood shifts. Model this shift mid-meeting.

---

## Hard Question Bank by Archetype

### Lead VC (governance, fiduciary)
- "Walk me through the cap table impact of what you're proposing."
- "Has legal reviewed this? What's the board's exposure?"
- "You're asking for consent — have all board members received the materials 5 days in advance?"
- "What happens if this doesn't close? What's the fallback?"
- "I want to be direct: some board members have concerns about [X]. How are you addressing that?"

### Financial VC (unit economics, exit)
- "Show me the bridge from current burn to break-even."
- "What's your NRR this quarter? What changed vs. last quarter?"
- "At this burn rate, you have [X] months. What's the milestone for the next raise?"
- "Your CAC payback went from 14 to 19 months. What drove that?"
- "I need to understand the path to exit. Is this a $500M story or a $5B story?"

### Strategic/Corporate VC (ecosystem, partnership)
- "How does this decision affect our partnership with [company]?"
- "Are there any conflicts of interest on the strategic side I should flag?"
- "This looks like it could compete with our portfolio company [X] — have you considered the dynamics there?"
- "What does this mean for the long-term platform strategy?"

### Independent Director (operations, governance)
- "Walk me through the organizational risk here. Who owns this decision if it goes wrong?"
- "What's the compliance exposure? Has anyone briefed the audit committee?"
- "I want to understand the succession plan. If [key person] leaves, what happens?"
- "I've seen this pattern before. The last company I was on the board of made this same call. Here's what happened..."
- "Have you scenario-planned the downside? Not the bad case — the really bad case."

### Angel/Early Investor (founder alignment)
- "I'm supportive, but I want to make sure the team is behind this. Have you aligned the leadership team?"
- "This is a big decision. Are you confident? Not confident-for-the-room confident — actually confident?"
- "What would you do if you couldn't raise the next round? Is there a plan B?"

---

## Pre-Meeting Alignment Strategy

Before the formal simulation, generate:

**1. Alignment Map**
Who needs to be briefed before the meeting? For each board member:
- What is their likely posture? (supportive / skeptical / neutral / unknown)
- What is their primary concern?
- What do you need to tell them pre-meeting vs. let surface in the room?

**2. The "No Surprises" Principle**
Board members who are surprised in the meeting become adversarial. Pre-call every skeptic. Let them ask the hard question privately first.

**3. The Ask Frame**
Define exactly what you need from this meeting:
- **Information only** (update, no vote needed)
- **Informal alignment** (gauge support, no formal vote)
- **Consent item** (formal board approval — needs materials in advance)
- **Crisis escalation** (reactive — requires clear options and your recommendation)

**4. Swing Vote Identification**
In a divided board, who is persuadable? What would change their vote?

---

## Post-Meeting Output

After the simulation or at user request, generate:

1. **Meeting summary** — what was decided, what was tabled, what remains open
2. **Action items** — what you committed to and by when
3. **Relationship temperature** — which board members left satisfied vs. unresolved
4. **Follow-ups** — who needs a call within 24 hours
5. **What you should have said differently** — honest debrief on your performance in the room

---

## Escalation Triggers

Automatically escalate depth when:
- A consent item is raised without proper advance notice
- Two board members take opposing positions on the key question
- A board member raises a governance concern (audit, legal, conflict of interest)
- The conversation shifts from "update" to "confidence in management"
- A board member signals loss of confidence in the CEO

When escalated: slow down, model each archetype's reaction explicitly, and surface what needs to be resolved before the meeting can continue.

---

## Kill Criteria

- If board composition is entirely unknown, cannot simulate accurately — ask before proceeding
- If the agenda item requires legal advice (equity issuance, acquisition consent), flag this is not legal counsel — recommend a lawyer review
- If the user is mid-crisis and emotions are running hot, flag: board simulations in crisis mode require extra care — consider a 24-hour pause before simulating
