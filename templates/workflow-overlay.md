# Workflow overlay template

An overlay is the part of your repo's instructions that tells Agent Workflow
skills how *this* repo wants them to behave. It turns on delegated
review-and-patch and lets the prompt orchestrator save prompts to files.
Everything else in the plugin works without one.

## Setup

1. Copy the section below into your repo's `AGENTS.md` (or `CLAUDE.md`).
2. Replace each `<...>` placeholder. Delete optional parts you don't need.
3. Commit it. The skills read it the next time they run.

Keep it in your instructions file while it's short. If it grows, move it to its
own file (for example `.agents/workflow-overlay/delegated-review-patch.md`),
leave a one-line pointer in `AGENTS.md`, and update
`operating_contract_pointer` to the new path.

---

```markdown
## Delegated review and patch

This section is the active project overlay for `workflow-delegated-review-patch`
and the repository review policy for `success-implement`. The installed skills
own the commission, review and adjudication mechanics; these are local bindings.

**Entry policy.** An explicit `delegate patch` request commissions a prompt for
named targets, a reason read-only review is insufficient, and a bounded patch
scope. Bind these from the request and current context; ask only for material
missing choices. Ordinary edits do not enter this lane automatically.

An instruction to `success implement` is a conditional commission: after
validation, delegate a material behavioral boundary that lacks independent
evidence or is supported only by the implementation's own assumptions.
Otherwise report `not_needed`, naming the affected behavior and independent
evidence. Size, novelty and importance alone do not trigger delegation.

- `opt_in/status`: `bound_opt_in_commission`, under the entry policy above.
- `operating_contract_pointer`: `AGENTS.md#delegated-review-and-patch`.
- `prompt_routing`: use the installed `workflow-prompt-orchestrator` and its
  bundled review/patch templates; output one `paste-ready-chat` prompt,
  delivered by the owner (`operator_courier_only`). Prepare the complete
  prompt or report a precise blocker; do not probe controllers, dispatch
  agents or execute review.
- `prompt_orchestrator_available`: exposed by the installed plugin; confirm
  the skill is available when used.
- `model_ladder`: record the actual author/home model and its vendor. The
  reviewer ("controller") is owner-selected, from a different vendor/model
  lineage, with read/write access to this repository. Another model tier from
  the same vendor is ineligible. Controller identity may be `receiver_to_bind`
  during prompt preparation; the receiver verifies identity and access before
  executing.
- `protected_path_list`: a commission authorizes the reviewer to patch only its
  named targets within the bounded scope; everything else is read-only. Never
  change <Git internals, installed skills, global settings, secrets, ...>.
- `preflight_schema`: each commission records the absolute worktree, branch,
  HEAD commit, named targets and relevant dirty state. Uncommitted targets are
  pinned with SHA256 hashes; the reviewer re-checks them before patching and
  stops on drift.
- `source_context_fields`: carry the requested outcome, patch scope, reason
  for delegation, this file, relevant source files and the validation to run
  (<your test/lint commands>).
- `output_destinations`: the commission, the reviewer's findings, diff,
  citations, verdict and residual risks, and the accept/modify/reject
  adjudication all stay in chat. Missing validation is reported as not run,
  never as a pass.
- `lifecycle_authority`: the reviewer leaves an uncommitted patch for
  adjudication; it does not commit, push, merge or deploy. The home agent
  adjudicates, verifies accepted changes, and follows this repo's normal rules
  for anything after that.

### Prompt files (optional)

Only needed if you want the prompt orchestrator to save prompts as files
instead of returning them in chat.

- Saved prompts go in `<docs/prompts/>`; name files
  `<topic>_<purpose>_prompt_v0.md`. Review prompts include
  `adversarial_review` in the name.
- Disposable drafts go in `<.workflow-runs/prompt-drafts/>` and may be deleted
  at any time.
```

---

## What each field controls

| Field | What it decides |
|---|---|
| `opt_in/status` | Whether the lane runs at all. Without it the skill only describes what it would do. |
| `operating_contract_pointer` | Where the rules for this lane live (this section, or a separate file). |
| `prompt_routing` | How the review prompt is produced and delivered (pasted by you into the other tool). |
| `model_ladder` | Who may review: always a different vendor from the model that wrote the work. |
| `protected_path_list` | What the reviewer may and may not edit. |
| `preflight_schema` | What state is pinned so the reviewer can detect that the code moved under it. |
| `source_context_fields` | What the reviewer is told to read and which checks to run. |
| `output_destinations` | Where findings, diffs and the final decision are recorded. |
| `lifecycle_authority` | That the reviewer never commits, pushes or deploys. |
