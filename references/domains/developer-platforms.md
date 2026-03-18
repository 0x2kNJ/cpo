# Domain Overlay: Developer Platforms

Apply these Five Truths calibrations when analyzing developer platform and tooling decisions.

| Truth | Developer Platform Calibration |
|-------|-------------------------------|
| **User Truth** | Developer experience IS the product. Docs, DX, and time-to-first-value are table stakes, not differentiators. A developer who can't get a working example in 15 minutes won't come back. The competitor is always one tab away. |
| **Strategic Truth** | Distribution is harder than building. Community and ecosystem are the moat, not the SDK. A platform with 10K GitHub stars and 500 active contributors is more defensible than one with a technically superior API and no community. |
| **Economic Truth** | Developer tools often monetize indirectly. Usage-based pricing aligns best with developer workflows — metered by API calls, compute, or seats. Flat pricing fights developer instinct to minimize cost before they trust the product. |
| **Macro-Political** | Open-source vs. proprietary is a strategic choice, not a technical one. Open core (OSS core + commercial features) is the dominant model. Going fully proprietary sacrifices distribution; going fully open sacrifices monetization. Choose deliberately and don't drift. |
| **Execution Truth** | Breaking changes destroy trust permanently. A developer who hit a breaking change once will mention it in every evaluation for years. Version carefully. Deprecate publicly. Give 6+ months of notice. The cost of backwards compatibility is much lower than the cost of reputation damage. |

## Developer Platform-Specific Kill Criteria

- If time-to-first-value (working example) is >15 minutes → adoption will be slower than the business plan assumes
- If the platform has no community strategy → ecosystem moat is absent, defensibility is low
- If the monetization model creates friction before developers see value → the funnel is broken at the top
- If breaking changes have been shipped without notice → trust has been damaged and must be explicitly rebuilt
- If the open-source strategy hasn't been decided → you will be forced into a reactive decision at the worst possible time
