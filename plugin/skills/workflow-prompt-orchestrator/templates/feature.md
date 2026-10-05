# Feature Prompt Template

Output mode: `[chat-only / paste-ready-chat / disposable-draft / saved-artifact / overlay-defined mode]`

Scoring note: If this prompt asks the receiver to score, rank, or compare options, include a smallest-complete pressure test: identify the smallest complete option that could satisfy the real objective, test under-scoping, under-fixing, validation loss, recurrence or downstream-cost risk, and compare broader or hybrid options against what their added scope materially improves.

Use the active project overlay to bind accepted product direction, source hierarchy, artifact roles, validation gates, edit permission, and implementation authorization when the requested output claims or needs them. In chat-only mode, keep missing project-owned fields as placeholders and do not claim implementation readiness. If product direction is missing, stop and route back to product-direction planning.

Task:
Plan a feature from the accepted product direction `[accepted product bet or decision source]`.

Required context:
- Workspace and expected revision: `[workspace / revision / dirty-state allowance]`
- Product direction source: `[accepted local source]`
- Target artifact role: `[role / destination or chat-only]`
- Planning mode: `[discuss / explore / decide / handoff / review-rerun]`
- Model lane status: `[bound / unbound warning / blocked if required]`
- Output frame: `[feature plan / handoff summary / chat-only result]`
- Validation gates: `[gate names and pass/fail semantics]`
- Edit and implementation authority: `[explicit authority]`

Return:
- Loaded sources and discuss-gate result.
- Source map.
- Option ledger and scoring rationale.
- Recommended feature plan or blocked result.
- Feature-planning units, validation plan, bloat-cut queue, and next step. Claim implementation authorization only when it is bound.
