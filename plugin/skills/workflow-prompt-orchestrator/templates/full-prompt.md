# Full Prompt Template

Output mode: `[chat-only / paste-ready-chat / disposable-draft / saved-artifact / overlay-defined mode]`

Scoring note: If this prompt asks the receiver to score, rank, or compare options, include a smallest-complete pressure test: identify the smallest complete option that could satisfy the real objective, test under-scoping, under-fixing, validation loss, recurrence or downstream-cost risk, and compare broader or hybrid options against what their added scope materially improves.

Create a standalone prompt containing the context needed for the receiving agent to act within the requested output mode without importing outside project policy. Use placeholders for unbound project-owned fields in chat-only mode.

Include:
- Goal.
- Workspace and revision preflight.
- Active overlay authority and source hierarchy.
- Target files, artifact roles, and edit permission.
- Required source reads.
- Constraints, non-goals, and protected paths.
- Template kind or task mode.
- Model lane status: `[bound / unbound warning / blocked if required]`.
- Validation gates and evidence expectations.
- Output mode, output frame, and final report shape.
- Blocker states that must stop the task.

Do not include project-specific rules unless they are supplied by the active overlay or user prompt.
