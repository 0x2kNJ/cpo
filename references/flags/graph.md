# `--graph` Flag — Decision Dependency Graph

**Trigger:** `/cpo --graph`

**What it does:** Computes and visualizes the dependency graph across all active decisions. Identifies bottleneck decisions that block the most downstream work, and surfaces orphaned decisions with no connections.

## Data Collection

```bash
_DJ=~/.cpo/decisions
if [ -d "$_DJ" ] && ls "$_DJ"/*.yaml 2>/dev/null | head -1 | grep -q .; then
  echo "GRAPH_DATA:"
  grep -l "status: active" "$_DJ"/*.yaml 2>/dev/null | while read -r _f; do
    echo "---"
    cat "$_f"
  done
else
  echo "NO_DECISIONS"
fi
```

**Schema version gate:** Only include entries with `schema_version: "1.5"` or higher in the graph analysis. Older entries may lack `truth_fingerprint`, `three_paths`, or `consequences` fields required for dependency detection. If entries are excluded, note: *"[N] entries excluded — pre-v1.5 schema. Run decisions through `/cpo` to upgrade."*

## Dependency Detection (prose analysis)

For each active decision, scan these fields to infer dependencies:
- `recommendation:` — does it reference another decision's topic or outcome?
- `open_questions:` — does a question map to another decision's verdict?
- `kill_criteria:` — does a kill criterion depend on another decision's outcome?
- `three_paths:` — does a path assume another decision has been resolved?
- `delta_from_prior:` — does it reference a shift caused by another decision?

Build a directed graph: `decision_A → decision_B` means A depends on B being resolved.

**Edge confidence:** Classify each edge as:
- **Strong** `→` — explicit reference to another decision by name, `decision_id`, or direct topic mention in `recommendation:` or `open_questions:`
- **Weak** `→?` — inferred from topic overlap, shared keywords, or indirect references in `three_paths:` or `kill_criteria:`

In the output, strong edges show as `#A → #B [explicit]` and weak edges as `#A →? #B [inferred]`.

## Bottleneck Scoring

For each node in the graph:
- **Fan-out score:** Number of decisions that directly or transitively depend on this node. Strong edges contribute 1.0 to fan-out; weak edges contribute 0.3.
- **Staleness penalty:** Days since last revision × 0.1 (added to fan-out)
- **Confidence discount:** Low confidence nodes get 1.5× multiplier on their bottleneck score (they're blocking AND uncertain)

**Bottleneck** = node with highest adjusted fan-out score.

## Output Format

```
DECISION GRAPH — [N] active decisions
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Bottleneck: #[decision_id] — [one sentence why this blocks the most]
  ↓ blocks: #[dep1], #[dep2], #[dep3]
  ↓ confidence: [H/M/L] · last touched: [date] · fan-out: [N]

Dependencies:
  #[id-A] → #[id-B] (A depends on B)
  #[id-C] → #[id-B] (C depends on B)
  #[id-D] → #[id-A] (D depends on A)
  [... all edges]

Orphaned (no connections):
  #[id-E] — [topic] — may be independent or missing explicit links

Transitive chains (longest):
  #[id-B] → #[id-A] → #[id-D] (3 hops — resolve B first)

Critical path: Resolve #[bottleneck] → unblocks [N] decisions → next bottleneck becomes #[next]
```

## Rules

- If fewer than 2 active decisions: *"Need at least 2 active decisions to build a graph. Log more decisions to unlock dependency analysis."*
- **Small graph (≤5 active decisions):** Skip bottleneck scoring math. Show a flat dependency list with one bottleneck callout: `Bottleneck: #[id] — [why]. Dependencies: [flat list of edges].` Reserve the full scoring (fan-out, staleness penalty, confidence discount) for 6+ decisions where the math produces meaningful differentiation.
- Dependencies are inferred from content analysis — they may not be explicit. Flag uncertain edges: `#A →? #B (inferred, not explicit)`
- If no dependencies found across all decisions: *"All [N] decisions appear independent — no dependency edges detected. This could mean decisions are well-isolated, or that cross-references aren't explicit enough in your prompts."*
- Bottleneck identification always names one decision — even if the graph is sparse
- After output, prompt: *"Want to dig into the bottleneck? Run `/cpo #[bottleneck-id]` to revisit it, or `/cpo --status` for the executive view."*
