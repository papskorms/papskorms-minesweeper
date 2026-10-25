# Procedure: implement an approved requirement

Use this procedure for a feature or bug fix.

1. Read INDEX.md, AGENTS.md, the relevant requirement IDs in 01_REQUIREMENTS.md, and related open questions in DECISIONS.md.
2. Confirm the request fits approved behavior. If it depends on an open material decision, do independent work and ask before implementing that behavior.
3. Check 02_ARCHITECTURE.md and existing code before selecting a pattern. If a technical choice is unrecorded, choose the smallest reversible option only when it does not change product behavior; document material choices.
4. Make the narrowest complete change satisfying the requirement. Keep unrelated polish, modes, and refactors out unless requested.
5. Update source-of-truth documents if the user approved a behavior change.
6. Verify only when the user asks. Use 05_TESTING_STRATEGY.md and report results honestly.
