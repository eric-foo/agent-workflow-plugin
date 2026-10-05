# Review Prompt Template

Output mode: `[chat-only / paste-ready-chat / disposable-draft / saved-artifact / overlay-defined mode]`

Review output binding for the receiving reviewer:
- Review output mode: `[filesystem-output / chat-output]`
- `required_output_path`: `[exact path for filesystem-output]`
- Path derivation: `[not allowed / explicitly authorized convention]`
- Chat after successful write: compact human summary plus courier YAML only.
- Write failure behavior: return `FAILED_REVIEW_OUTPUT_WRITE`; do not claim chat
  is equivalent to the missing durable artifact.

Scoring note: If this prompt asks the receiver to score, rank, or compare options, include a smallest-complete pressure test: identify the smallest complete option that could satisfy the real objective, test under-scoping, under-fixing, validation loss, recurrence or downstream-cost risk, and compare broader or hybrid options against what their added scope materially improves.

Run a read-only review of `[artifact or source scope]`. Do not edit files unless a later prompt explicitly changes the mode and the overlay permits it.

Required context:
- Source-of-truth worktree: `[absolute path or repo identifier]`
- Expected branch and revision: `[branch / HEAD or commit / dirty-state allowance]`
- Commission: `[review request, decision question, or accepted review purpose]`
- Review target: `[files / artifact role / prompt artifact]`
- Review lane and decision criteria: `[overlay-bound lane / criteria]`
- Model lane status: `[bound / unbound warning / blocked if required]`
- Output frame: `[findings-first report / overlay-bound reviewer verdict / destination]`
- Required review report path: `[required_output_path / not applicable for explicit chat-output]`
- Strict-shaped outputs: `[overlay-bound verdict/severity/blocked-ready/patch authority or NOT_CLAIMED]`
- Validation evidence to inspect: `[commands / logs / artifacts]`
- Worktree handling: `[inspect this existing worktree in place; if launched elsewhere, change directory to the pinned worktree when accessible; do not create, clone, request, or switch to a different worktree unless this prompt explicitly authorizes that]`

Before reviewing, confirm that you are reading the source-of-truth worktree and
expected revision above. Review target and purpose are commission-bound; do not
silently retarget the review because adjacent evidence is visible. If the path
is unavailable, the revision does not match, or the dirty-state allowance is
violated, return a blocked result using the nearest existing blocker instead of
reviewing a fresh or substitute checkout.

Before full review, confirm the review output binding above. If formal or
durable review is requested and the mode is missing, return
`BLOCKED_OUTPUT_MODE_MISSING`. If `filesystem-output` is selected and no valid
`required_output_path` or explicitly authorized derivation convention is bound,
return `BLOCKED_OUTPUT_DESTINATION_UNBOUND`. Do not invent a path or fall back
to a full chat transcript.

For `filesystem-output`, write the full review report to
`required_output_path`. After a successful write, return only a compact human
summary and this courier YAML:

```yaml
review_courier:
  output_mode: filesystem-output
  report_path: <required_output_path>
  commission: <review request or NOT_CLAIMED>
  target: <reviewed target or blocked state>
  authority: <bound authority summary or NOT_CLAIMED>
  decision_criteria: <bound criteria summary or NOT_CLAIMED>
  evidence_summary: <short source-backed summary>
  reviewer_verdict: <overlay-bound verdict, blocked state, or NOT_CLAIMED>
  finding_ids: []
  minimum_closure_conditions: []
  next_authorized_action: <smallest complete next authorized step>
  non_claims: []
```

Inside the durable report for `filesystem-output`, return findings first,
ordered by decision-relevant materiality unless an overlay-bound severity
taxonomy exists, with file or artifact references where applicable. Each
finding should include `minimum_closure_condition` and
`next_authorized_action`; emit a `patch_queue_entry` only when overlay-bound
patch or executor-handoff authority exists. For explicit `chat-output`, use the
same findings-first shape in chat. In compact chat after a successful filesystem
write, do not repeat the full findings list; return only the summary and courier
YAML above. Include open questions and residual risk. Return readiness, formal
verdicts, "needs patch", severity labels, blocked/ready status, or patch-queue
routing only when the review lane, decision criteria, verdict/severity
vocabulary, readiness criteria, and patch authority are bound; otherwise label
those claims `NOT_CLAIMED` or `not proven`.
