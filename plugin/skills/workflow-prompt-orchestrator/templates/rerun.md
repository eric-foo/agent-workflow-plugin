# Rerun Prompt Template

Output mode: `[chat-only / paste-ready-chat / disposable-draft / saved-artifact / overlay-defined mode]`

Scoring note: If this prompt asks the receiver to score, rank, or compare options, include a smallest-complete pressure test: identify the smallest complete option that could satisfy the real objective, test under-scoping, under-fixing, validation loss, recurrence or downstream-cost risk, and compare broader or hybrid options against what their added scope materially improves.

Retry only the unresolved delta from prior work. Preserve frozen decisions unless the user and overlay explicitly reopen them. Do not regenerate the whole workflow unless scope-reset authority is bound.

If this is a patch recheck after a prior blocker or major review finding, run
a smallest-complete bounded blast-radius check:

1. Verify whether the patch closes the original finding or failed gate.
2. Scan only the touched patch scope for patch-caused or newly visible
   blocker/major issues that could invalidate closure or create serious
   downstream risk.

Do not reopen unrelated structural review, full-artifact review, minor/nit
findings, or pre-existing issues outside the touched scope unless a separate
second-pass review lane is explicitly requested and authorized.

Required context:
- Prior artifact path: `[path or explicit reason unavailable]`
- Prior artifact hash or revision: `[sha256 / commit / revision]`
- Unresolved finding or failed gate: `[finding / gate]`
- Frozen decisions: `[list]`
- Mutable fields: `[list]`
- Prior output frame: `[summary shape / verdict vocabulary / report destination]`
- New evidence or patch scope: `[scope]`
- Recheck scope rule: `[original finding only / original finding plus smallest-complete bounded blast-radius check of touched patch scope]`
- Validation expectation: `[gate / command / manual evidence]`
- Scope-reset authority: `[none / explicit user and overlay authority]`

Return:
- Whether prior evidence was reused, superseded, or rejected.
- Minimal work performed or recommended.
- Updated result for the unresolved issue.
- For patch rechecks, the smallest-complete bounded blast-radius check result:
  any patch-caused or newly visible blocker/major findings inside the touched
  scope, or an explicit statement that none were found.
- Validation plan, or validation result only when the gate was actually run and evidence is available.
- Any remaining blocker.
- Confirmation that frozen decisions stayed frozen, mutable fields were the only changed fields, and no scope reset occurred without explicit authority.
