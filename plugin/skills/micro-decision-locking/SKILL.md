---
name: micro-decision-locking
description: Pre-implementation discipline for locking the few implementation-critical micro-decisions needed after a concrete route, scoped plan, accepted review finding, or patch direction already exists and before source-changing edits begin. Use when the pain is semantic drift, scope inflation, false-success paths, vocabulary ownership ambiguity, validation ambiguity, claim inflation, doctrine-bearing edits, fake-pass-prone work, or tempting adjacent artifacts; do not use for trivial patches, initial architecture planning, implementation scoping, prompt orchestration, or broad planning.
---

# Micro-Decision Locking

## Purpose

Use this skill to convert a concrete accepted route into a small set of frozen execution decisions before implementation starts. The goal is to prevent semantic drift, scope inflation, false-success claims, and accidental reopening of settled plan boundaries.

This skill does not implement patches, write files by itself, invent architecture, or replace `workflow-implementation-scoping`.

## Pain Link

Trigger from pain, not ceremony. The lock is useful when the next patch could plausibly fail by:

- Drifting from the accepted route while still producing plausible-looking text.
- Expanding into adjacent cleanup, taxonomy, architecture, or artifact creation.
- Moving vocabulary into the wrong owning source.
- Treating weak evidence as stronger proof.
- Choosing validation that only shows a file changed, not that the patch stayed bounded.
- Making a final claim stronger than the executed patch supports.

If none of these pains are present, return `not_needed`.

## Trigger Test

Use this skill only when all are true:

- A concrete route, scoped plan, accepted review finding, or bounded patch direction already exists.
- A source-changing or doctrine-bearing implementation step is about to begin.
- Small ambiguities could materially alter touched files, vocabulary, validation, stop conditions, or final claims.

Prefer `not_needed` when the task is obvious, local, reversible, low-risk, and has no meaningful semantic or validation ambiguity.

Prefer `blocked` when a required user-owned decision cannot be safely inferred.

## Smallest Complete Intervention Guard

Before locking decisions, verify that the lock itself is the smallest complete intervention.

Return `blocked` if the route is not concrete enough to identify the likely touched files or artifacts, validation focus, stop conditions, and final claim boundaries.

Lock only decisions whose answers could materially change the next implementation step. Do not add questions to satisfy a numeric target.

Default to excluding adjacent cleanup, taxonomy changes, architecture changes, new artifacts, examples, or broader doctrine edits unless they are directly necessary for the accepted route to be complete.

Completeness includes failure visibility. Do not lock fake fallbacks, fake validation, silent degradation, or final claims stronger than the executed patch can support.

## Micro Question Discipline

Before locking answers, run one bounded quality pass:

- Identify 2-5 fake-success paths for the implementation.
- Identify vocabulary or ownership drift risks.
- Ask: could any new wording create a validation, readiness,
  downstream-usability, or completeness overclaim?
- Remove questions that would not change implementation.
- Classify each remaining decision as user-owned or agent-owned.
- Lock final-claim boundaries.

If the overclaim answer is yes, do not return `not_needed`. Lock the final
claim boundary explicitly. When the active route can carry review timing, mark
`adversarial_review: recommended`, or preserve the stricter carried value when
one was received (`required_by_bound_gate` is never downgraded), as a routing
signal for the next lane or implementation closeout; if adding that signal would
invent route authority outside the accepted route, return `blocked` and route
back to implementation scoping instead.

## Good Fits

Use for:

- Doctrine-bearing source edits where wording, ownership, or final claims matter.
- Review patches where accepted findings must not expand into unrelated cleanup.
- Source-capture, receipt, validation, or evidence changes that could imply stronger proof than exists.
- Changes with tempting adjacent artifacts, documents, taxonomy, or architecture questions.
- Patches where pass/fail can be faked by broad wording, misplaced vocabulary, or weak validation.

Do not use for:

- Creating the original implementation route or architecture.
- Replacing implementation scoping.
- Prompt drafting or handoff construction.
- Ceremony around tiny typo fixes or mechanical edits.
- Broad checklists that do not affect execution.

## Decision Budget

Ask the fewest questions that materially change execution.

- Minimum useful output: all and only the decisions needed to make the next implementation step complete.
- Small bounded patch: often 1-3 locked answers.
- Non-trivial bounded patch: often 3-8 locked answers.
- Maximum: 15 locked answers.
- Too many: more than 15, or any question whose answer would not change touched files, constraints, validation, stop conditions, or final claims.

Do not invent extra questions to satisfy a target count. If there are no material ambiguities, return `not_needed`.

If more than 15 material decisions seem necessary, the route is probably not concrete enough; return `blocked` and name the missing scope layer instead of expanding the lock.

## Ownership Rules

Classify each question before answering:

- User-owned: product intent, doctrine meaning, artifact authority, accepted finding boundaries, risk tolerance, or claim strength. Ask the user or block if the answer is not already explicit.
- Agent-owned: local execution choices that follow from the route, such as exact edit order, narrow wording mechanics, validation command selection, or how to preserve existing style. Infer these conservatively and state the reason.

Never convert a user-owned decision into an agent-owned assumption just to keep moving.

When an ambiguity is only whether to include adjacent work not present in the accepted route, default to excluding it. Treat this as agent-owned boundary preservation, not as a user-owned product decision.

If excluding adjacent work would make the accepted route incomplete, classify the ambiguity as user-owned and ask or return `blocked`.

## Lock Workflow

1. Restate the concrete route or patch direction in one short phrase.
2. Run the Micro Question Discipline pass.
3. Identify only ambiguity points that could change execution.
4. Remove ceremonial questions and anything already settled by the route.
5. Lock each answer with a reason and execution effect.
6. Include stop conditions, validation focus, and non-claims.
7. Pass the resulting YAML directly into the implementation phase.

Frozen answers are binding for the next implementation phase. Implementation may not reopen them unless new evidence invalidates a locked answer; if that happens, stop and report why the lock broke.

## Output

Return concise prose if useful, then include this machine-readable block:

```yaml
micro_decision_lock:
  status: locked | blocked | not_needed
  applies_to: ""
  route_ready: true | false
  blocked_reason: ""
  locked_answers:
    - question: ""
      answer: ""
      owner: user | agent
      reason: ""
      execution_effect: ""
  stop_conditions:
    - ""
  validation_focus:
    - ""
  non_claims:
    - ""
  review_timing_carry_forward:
    adversarial_review: not_needed | recommended | required_by_bound_gate | not_applicable
    highest_value_checkpoint: ""
    review_target: ""
    why: ""
```

The carried `review_timing_carry_forward.adversarial_review` value must equal
the strictest review obligation received from scoping or spec writing
(`required_by_bound_gate` over `recommended` over `not_needed`), never a
downgrade. Carry `highest_value_checkpoint`, `review_target`, and the upstream
`why_this_checkpoint` (recorded here as `why`) verbatim from
scoping or spec writing so a standalone implementation handoff
knows where implementation must stop, not just that review is required; when
more than one checkpoint was received, carry the earliest/strictest one, never
a later or looser checkpoint.

For `not_needed`, explain why no lock is useful.

For `blocked`, include the unresolved micro-questions and why they are user-owned or route-owned.

## Scope Control

Use the lock to narrow implementation:

- Name what is allowed to change.
- Name what must not be reopened.
- Defer adjacent architecture, taxonomy, artifact creation, or ownership questions unless the accepted route explicitly includes them.
- Make validation prove the patch stayed inside the lock, not that the whole system is complete.
- Make final claims no stronger than the locked validation and source-state evidence.

## Examples

```yaml
question: Do we create the new operating-model doc now?
answer: no
owner: user
execution_effect: patch ladder first; defer operating-model spine
```

```yaml
question: Where do closeout states belong?
answer: evidence ladder
owner: user
execution_effect: do not put claim-tier vocabulary in a new doc
```

```yaml
question: Do we resolve reveal/calibration ownership?
answer: no
owner: user
execution_effect: mark deferred; do not invent ownership
```

```yaml
question: Are we fixing only accepted findings?
answer: yes
owner: user
execution_effect: do not broaden to unrelated cleanup
```

```yaml
question: Can current-body capture be called pre-cutoff identity proof?
answer: no
owner: agent
execution_effect: preserve weaker source-state claim
```
