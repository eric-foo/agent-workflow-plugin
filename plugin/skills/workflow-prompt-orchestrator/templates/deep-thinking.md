# Deep-Thinking Prompt Template

Output mode: `[chat-only / paste-ready-chat / disposable-draft / saved-artifact / overlay-defined mode]`

Scoring note: If this prompt asks the receiver to score, rank, or compare options, include a smallest-complete pressure test: identify the smallest complete option that could satisfy the real objective, test under-scoping, under-fixing, validation loss, recurrence or downstream-cost risk, and compare broader or hybrid options against what their added scope materially improves.

Use this for deeper option comparison and verification. Respect the active project overlay for local source hierarchy, protected paths, formatting conventions, validation commands, and review lanes. In chat-only mode, keep missing project-owned fields as placeholders and avoid validation, readiness, or review-lane claims.

Task:
Deep think about `[decision, problem, or failure mode]`.

Required context:
- Workspace or decision context: `[source]`
- Candidate approaches already proposed: `[list or none]`
- Constraints and success criteria: `[bound sources]`
- Model lane status: `[bound / unbound warning / blocked if required]`
- Output frame: `[problem framing / options / verification / recommendation]`
- Validation expectations: `[gate or evidence expectation]`

Return:
- Problem framing.
- Decision criteria.
- Options considered.
- Eliminated or downgraded options with reasons.
- Best approach or hybrid.
- Verification notes, assumptions, and final recommendation.
