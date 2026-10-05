# Patch Prompt Template

Output mode: `[chat-only / paste-ready-chat / disposable-draft / saved-artifact / overlay-defined mode]`

Scoring note: If this prompt asks the receiver to score, rank, or compare options, include a smallest-complete pressure test: identify the smallest complete option that could satisfy the real objective, test under-scoping, under-fixing, validation loss, recurrence or downstream-cost risk, and compare broader or hybrid options against what their added scope materially improves.

Design a bounded patch prompt within the target scope authorized by the active project overlay and user prompt. Ask the receiving actor to apply or execute the patch only when patch or executor authority is explicitly bound.

Run a contract-impact gate before asking for executor-ready patching. The gate
asks whether the patch changes or depends on something downstream authors,
reviewers, tests, tools, prompts, examples, or harnesses must conform to. File
type is not the trigger. If the patch is contract-sensitive, the prompt must
provide a source-backed contract map or instruct the receiver to block
executor-ready patching and route back to scoping or review.

Required context:
- Workspace and expected revision: `[workspace / branch / hash / dirty-state allowance]`
- Target files or directories: `[explicit scope]`
- Protected paths: `[overlay-bound list]`
- Contract-impact gate: `[local_patch / contract_sensitive_with_map / contract_sensitive_blocked]`
- Contract map, when contract-sensitive: `[authority sources / source precedence / artifact roles / field-or-behavior hierarchy / invariants / mirrored surfaces / patch targets / readback expectations / unresolved gaps]`
- Required change: `[behavioral goal]`
- Non-goals: `[out of scope]`
- Model lane status: `[bound / unbound warning / blocked if required]`
- Output frame: `[patch summary / patch queue / validation report]`
- Validation commands or evidence: `[gates]`
- Edit or patch authority: `[not bound advisory-only / exact files / patch-only / executor authority]`

Return:
- Preflight result.
- Contract-impact gate result, including any blocker for missing source precedence, artifact role, validation expectation, or contract map.
- Proposed patch summary by file or artifact.
- Executable patch queue only when patch or executor authority is bound and any contract-sensitive impact has a sufficient source-backed contract map.
- Validation plan, or validation result only when the gate was actually run and evidence is available.
- Residual risks, blockers, and next authorized step.
