# Handoff Prompt Template

Output mode: `[chat-only / paste-ready-chat / disposable-draft / saved-artifact / overlay-defined mode]`

Scoring note: If this prompt asks the receiver to score, rank, or compare options, include a smallest-complete pressure test: identify the smallest complete option that could satisfy the real objective, test under-scoping, under-fixing, validation loss, recurrence or downstream-cost risk, and compare broader or hybrid options against what their added scope materially improves.

Prepare a handoff to another agent, worktree, or phase. Use only active overlay authority for artifact roles, destinations, source hierarchy, validation gates, and review routing. For chat-only inline handoffs, mark unbound repo or artifact fields as placeholders instead of inventing them.

Put the workflow instruction in the generated courier instead of asking the
operator to type another message:

- If the bound next act explicitly authorizes implementation, begin with
  `Success implement the commissioned work in:` followed by the handoff path.
- If the bound next act is planning-only, begin with `Plan the commissioned work
  in the packet with success implement. Do not implement.` followed by the
  handoff path.
- For review, diagnosis, read-only, or otherwise non-implementation handoffs,
  retain a neutral continuation opening.

These openings select an already-bound workflow. They never grant missing
authority or convert planning into execution.

Task:
Continue or take over `[work unit / artifact / phase]`.

Required context:
- Source-of-truth workspace: `[path or repo identifier]`
- Expected branch and revision: `[branch / hash / dirty-state allowance]`
- Current status: `[completed / partial / blocked]`
- Target files or artifact roles: `[scope]`
- Model lane status: `[bound / unbound warning / blocked if required]`
- Output frame: `[summary shape / artifact mode / report destination]`
- Edit permission: `[not applicable / read-only / patch-only / write scope]`
- Required validation: `[commands or evidence gates]`
- Frozen decisions and mutable fields: `[list]`
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

Return:
- Confirmed preflight.
- Work to perform.
- Constraints and protected paths.
- Validation evidence to collect.
- Required final report shape and blocker semantics.
