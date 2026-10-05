# Thin Wrapper Template

Output mode: `[chat-only / paste-ready-chat / disposable-draft / saved-artifact / overlay-defined mode]`

Scoring note: If this prompt asks the receiver to score, rank, or compare options, include a smallest-complete pressure test: identify the smallest complete option that could satisfy the real objective, test under-scoping, under-fixing, validation loss, recurrence or downstream-cost risk, and compare broader or hybrid options against what their added scope materially improves.

Wrap an existing source artifact, template, or prompt with the minimum applicable preflight and output contract needed for the receiving lane.

Required context:
- Title: `[launch title]`
- Wrapped source: `[path / hash / inline artifact]`
- Worktree or repository identifier: `[path or repo id / not applicable for chat-only inline prompt]`
- Expected branch, HEAD, or commit: `[branch / hash / detached revision / not applicable]`
- Prompt path: `[path or chat-only inline prompt]`
- Prompt SHA256: `[hash when a file exists / not applicable for chat-only]`
- Dirty-state allowance: `[clean required / modified allowed / untracked in scope / not applicable]`
- Authority source: `[overlay file or explicit user instruction]`
- Target files or directories: `[scope]`
- Output mode: `[chat-only / paste-ready-chat / disposable-draft / saved artifact / project-bound mode]`
- Edit permission: `[not applicable / read-only / patch-only / write scope]`
- Required summary shape or output frame: `[summary / findings / patch queue / artifact]`
- Validation expectation: `[gate / evidence]`
- Preflight failure behavior: `[return BLOCKED on applicable wrong worktree, stale revision, hash mismatch, or edit-permission failure]`
- Supplied goal handoff: `[verbatim goal_handoff / not supplied / owner omitted]`
- Thread operating target continuity:
```yaml
thread_operating_target_continuity:
  carried_forward: yes | no
  reason: same_workstream | different_workstream | no_visible_active_target | retired_or_blocked | owner_omitted | conflict
  changed_from_input: no | yes
  lifecycle_status:
  if_changed_reason:
```

Prompt:
Use the wrapped source as the task authority. Before acting, verify the applicable worktree, branch or revision, prompt hash when present, dirty-state allowance, scope, edit permission, output mode, and validation gates above. If any binding required by the requested output is missing, stale, or mismatched, return a blocked result before editing, validating, executing, or claiming readiness. Preserve the wrapped source's intent and report only the result requested by the output frame.
