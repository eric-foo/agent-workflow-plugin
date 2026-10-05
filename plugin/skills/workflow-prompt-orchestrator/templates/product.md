# Product Prompt Template

Output mode: `[chat-only / paste-ready-chat / disposable-draft / saved-artifact / overlay-defined mode]`

Scoring note: If this prompt asks the receiver to score, rank, or compare options, include a smallest-complete pressure test: identify the smallest complete option that could satisfy the real objective, test under-scoping, under-fixing, validation loss, recurrence or downstream-cost risk, and compare broader or hybrid options against what their added scope materially improves.

Use the active project overlay to bind source hierarchy, artifact roles, validation gates, product authority, edit permission, and output destination when the requested output claims or needs them. In chat-only mode, keep missing project-owned fields as placeholders instead of inventing project policy.

Task:
Plan product direction before feature planning begins for `[product decision or bet]`.

Required context:
- Workspace and expected revision: `[workspace / revision / dirty-state allowance]`
- Product authority: `[accepted local product source or blocker]`
- Artifact role and destination: `[role / destination or chat-only]`
- Model lane status: `[bound / unbound warning / blocked if required]`
- Output frame: `[product decision report / handoff summary / chat-only result]`
- Validation gates: `[gate names and pass/fail semantics]`
- Edit permission: `[read-only / patch-only / docs-write / other bound mode]`

Return:
- Loaded sources and unresolved authority blockers.
- Product decision framing.
- Option ledger.
- Recommended product direction or blocked result.
- Bet units, non-goals, kill criteria, validation plan, and readiness for feature planning.
