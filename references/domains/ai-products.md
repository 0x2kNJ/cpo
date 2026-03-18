# Domain Overlay: AI Products

Apply these Five Truths calibrations when analyzing AI product decisions.

| Truth | AI-Specific Calibration |
|-------|------------------------|
| **User Truth** | Utility collapses without reliability. Benchmark-beating means nothing if trust erodes after one bad output. Users tolerate 80% accuracy in demos; they abandon at 80% accuracy in production. |
| **Strategic Truth** | The model commoditizes; workflow lock-in and data flywheel are the moat. Ask: "If OpenAI ships this feature in 6 months, what's left?" If the answer is "nothing," you don't have a strategy. |
| **Economic Truth** | Inference cost is a variable cost that must be in unit economics from day one. A product that's profitable at 1K users can be underwater at 100K if inference costs scale faster than price. |
| **Macro-Political** | Regulation is 12–24 months out in every major jurisdiction (EU AI Act, US executive orders, China CAC rules). Build compliance hooks early — retrofitting is 3x the cost. |
| **Execution Truth** | AI product iteration is 3–5x faster than traditional software. Your roadmap must reflect this — 6-month plans are too long. Your competitive disadvantage can appear in 90 days. |

## AI-Specific Kill Criteria

- If the product's core value depends on model capabilities that don't exist reliably in production → flag as execution risk
- If there's no data flywheel or workflow lock-in → the product is a wrapper, not a business
- If inference costs at target scale produce negative gross margin → financial model is broken
- If the core use case is in a regulated domain (healthcare, legal, financial advice) without compliance strategy → regulatory risk is existential
