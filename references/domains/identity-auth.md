# Domain Overlay: Identity / Auth / Proof-of-Human

Apply these Five Truths calibrations when analyzing identity, authentication, or verification product decisions.

| Truth | Identity/Auth Calibration |
|-------|--------------------------|
| **User Truth** | Friction is the product's enemy. Every additional auth step is measurable churn. Users accept friction once (signup), tolerate it occasionally (re-auth), and abandon when it's repeated. The UX bar is set by consumer apps (Face ID, passkeys), not enterprise SSO. |
| **Strategic Truth** | Trust infrastructure is winner-take-most. Being second in a customer's identity stack is dangerous — switching costs are high but so is the cost of being a redundant layer. The moat is becoming the identity primitive that everything else builds on. |
| **Economic Truth** | CAC is high; LTV is long if you become infrastructure. Don't undercharge infrastructure. The mistake is pricing as a feature ($5/seat) when you're building infrastructure ($0.01/verification × millions of verifications). |
| **Macro-Political** | Biometric data laws add 6–12 months to enterprise deployment. GDPR Article 9 (EU), BIPA (Illinois), CCPA/CPRA (California) all treat biometric data as special category. Build data minimization and consent flows before your first enterprise deal, not after. |
| **Execution Truth** | Security incidents are existential. A breach doesn't just lose customers — it destroys the core product promise. Threat modeling must precede every ship. Assume breach in your architecture. |

## Identity/Auth-Specific Kill Criteria

- If the product stores biometric data without a clear legal basis in every target jurisdiction → regulatory exposure is existential
- If a single security breach would make the product unsellable → the security architecture must be reviewed before launch
- If friction at the critical auth step is higher than alternatives → adoption will stall regardless of downstream value
- If the business model depends on data monetization from identity data → regulatory and trust risk is extremely high
