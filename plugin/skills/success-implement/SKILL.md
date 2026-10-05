---
name: success-implement
description: "Bind falsifiable success signals for an authorized implementation, make the smallest complete change, validate the owner-visible outcome, and route delegated review-and-patch after validation under repository policy, using the non-trivial default only without a bound policy. Trigger when the user explicitly invokes /success-implement, gives the standalone instruction 'success implement' or 'success-implement' for an authorized change, explicitly asks to use the success-implement workflow, or uses 'success implement' or 'success-implement' as an instruction anywhere inside a planning request -- not only as a standalone command or formal co-invocation -- so the plan carries proposed success signals, near-miss and wrong-cause checks, observability gaps, and a review checkpoint that binds a later executor to run and report them. Do not trigger when the phrase is merely quoted or discussed, for ordinary planning that never uses the instruction, for review-only work, or for implementation without authority."
---

# Success Implement

## Purpose and authority

Bind the goal, authority, invariants, falsifiable acceptance signals, and review
checkpoint for an already-authorized implementation; make the smallest complete
change; validate the owner-visible outcome; and apply the active repository
review policy, with de-correlated delegated review-and-patch as the generic
default for non-trivial work when no repository policy is bound.

This skill supplies reusable implementation mechanics only. It does not supply
repository facts, implementation permission, protected-action permission,
validation commands, review lanes, or lifecycle claims. Defer those to the
active repository instructions and owning sources. If the requested outcome or
implementation authority is missing, stop and name the gap.

Invocation authorizes no work beyond the user's bound request. Do not widen the
change merely to improve confidence or make review easier.

### Planning-workflow composition

When the user uses the instruction "success implement" or "success-implement"
anywhere inside a planning request -- not only as a standalone command, and
regardless of whether it is formally described as co-invocation -- the
planning workflow remains the controlling lane. Contribute only the proposed
goal, authority, invariants, non-goals, success signals, plausible near-miss,
wrong-cause checks, observability gaps, and delegated-review checkpoint needed
by the plan. Mark the signals `PLANNED_NOT_OBSERVED`. Do not edit, execute,
validate, or dispatch review, and do not require implementation authority for
this planning contribution. Never auto-dispatch delegated review-and-patch
during this planning-only contribution. Merely quoting, discussing, or asking
about the phrase does not trigger this contribution.

Bind the plan itself to require a later executor to run and report on the
bound signals; the accepted plan carries that obligation forward, not a
repeat invocation of this skill.

A later implementation turn does not need to invoke this skill again to
consume that planned contract. It must fresh-check the plan's authority,
assumptions, target revision, and observability before editing; run the
bound signals and report the results; and revise any stale or unprovable
signal explicitly. Re-invoke this skill only when the planned contract is
unavailable, stale, unprovable, or displaced by implementation changes made
after the plan was accepted.

## Failure prevented

Prevent an implementation from being called successful because its code ran,
its tests passed, or its expected files exist when the owner's outcome was not
actually demonstrated. Also prevent implementation-family blind spots from
being treated as rare after an explicitly high-assurance invocation, while
keeping mechanical changes out of the delegated-review lane.

## Entry gate

Outside the explicit planning-workflow composition above, proceed only when all
are true:

- the owner-requested outcome and the condition under which it must hold are
  clear;
- implementation is authorized;
- the controlling behavior sources and protected boundaries are known;
- the edit can be bounded; and
- the outcome can be observed or a missing observation can be reported
  honestly.

If a material product or design choice remains open, return that choice instead
of inventing it.

## Bind the success contract

Before editing, write a compact `SUCCESS_CONTRACT` containing:

- **Goal:** the owner-visible outcome, not the proposed implementation.
- **Authority:** the sources and instructions that control behavior.
- **Invariants:** what must remain true.
- **Non-goals:** adjacent work that is outside this unit.
- **Signals:** the observations that will distinguish success from a plausible
  near-miss.
- **Review checkpoint:** the repository-owned review predicate and delivery
  mode, or the generic fallback below when no policy is bound. Preserve any
  carried `required_by_bound_gate` checkpoint, target, and reason; it may
  require review and adjudication before a dependent step, not just closeout.

Define each signal with:

```yaml
- name:
  given:
  when:
  then:
  forbidden:
  evidence:
  wrong_cause_check:
  repeat:  # only when retries, recovery, or idempotency matter
```

For a small bounded task, an equivalent compact table or paragraph is enough;
do not expand every signal into YAML when the same fields remain explicit.

Use the smallest set that covers the real outcome. A robust set normally has:

1. a positive observation at the boundary the owner cares about;
2. a negative or forbidden-path observation;
3. a perturbation that would expose a plausible false positive; and
4. a repeat, recovery, or idempotency observation when state persists or work
   may be retried.

Reject signals that only say "tests pass," "no exception," "file exists," or
"the command reported success." Those may be evidence carriers, but they are
not the owner outcome.

Turn every load-bearing outcome qualifier into a direct observation. In
particular, assert required cardinality, envelope/type, identity, ordering,
precedence/conflict handling, and persistence boundaries; repeating words such
as "one", "exact", or "set" in the goal does not make them observed.

## Pressure-test the signals

Before implementation:

1. Name the most plausible implementation that would pass while still failing
   the goal.
2. Ensure at least one signal rejects that near-miss.
3. Seed or simulate the target violation when practical and confirm the signal
   fails.
4. Make the wrong-cause check prove the intended boundary fails first. Avoid a
   test that passes because an earlier, unrelated guard rejected the input.
5. Capture a pre-change baseline. Prefer showing that a new signal fails before
   the fix. When the requested surface does not exist yet, record that absence
   and require a controlled post-build mutation to prove fail capability; do
   not fabricate a pre-change red result.

For an inventory, detector, matcher, or source census, also:

- name the semantic unit being counted or classified;
- seed a violation inside an already-admitted file or unit, not only as a new
  file;
- challenge at least one plausible behavior family outside the initial token
  or API vocabulary; and
- when tracked-source membership is claimed, prove ignored or untracked scratch
  cannot change the result.

If no affordable signal distinguishes the outcome from the near-miss, do not
pretend the implementation is strongly validated. Narrow the claim or stop for
the missing observability.

Once the contract and implementation-controlling seams are bound, stop source
loading and implement. Continue reading only for a concrete unresolved
authority, invariant, or validation dependency.

## Implement

Make the smallest complete intervention that satisfies the success contract.
Keep every changed line traceable to the goal, an invariant, or required
validation. Preserve real failure visibility; do not add fallback behavior that
turns an unknown or failed state into apparent success.

Follow the repository's isolation, editing, generated-file, and protected-action
rules. Do not add a registry, framework, required checklist, or standing review
step unless the bound outcome would otherwise remain false or materially
fragile.

## Validate

Cover the bound success signals, focused behavior, affected integration or
contract checks, and the repository's required gates. Start with the smallest
check that can expose the target failure. Reuse one observed result for every
obligation it actually covers; these are coverage needs, not four mandatory
command runs. The repository owns which checks run locally or in CI. Do not
repeat valid evidence on unchanged inputs merely to satisfy another heading;
rerun affected checks after a change, failure, stale input, or unresolved gap.

Preserve each command's exit status and actual output. For long-running checks,
set the timeout from observed or historical duration, add in-command timestamps
or elapsed-time measurement, and prefer progress-visible output. Silence from a
tool wrapper is not evidence of a deadlock; distinguish command-body runtime
from tool or orchestration latency.

Re-run the near-miss or seeded violation after implementation. Record signals
that were not run and why. Structural validation does not prove deployment,
resolver activation, production readiness, or owner acceptance.

## Apply the delegated safety net

First bind the active repository's review policy. It owns whether review is
required, its timing, and whether entry means execution or operator-courier
prompt preparation. Apply that predicate even when it permits `not_needed`
for non-trivial work; this skill must not install a stricter standing review
rule over it. A carried `required_by_bound_gate` checkpoint remains binding:
do not cross the guarded dependent step until review and home-model
adjudication resolve. A recommendation is routed under the repository policy,
not silently upgraded to a required checkpoint.

When the bound policy calls for review, invocation already authorizes its
permitted entry mode. Never ask again whether to prepare an already-required
review. Courier-only entry renders the complete prompt without probing or
launching a controller when the repository forbids those actions.

Only when no repository review policy is bound, use the following fallback:
non-trivial completed work enters de-correlated delegated review-and-patch
after validation; mechanical work may skip under the conditions below.

Treat the work as non-trivial when it changes or materially depends on one or
more of these:

- a parser, detector, matcher, or serializer has a broad input space that the
  authored signals sample rather than exhaust;
- executable behavior, validation semantics, a schema or public contract,
  external I/O, stored state, or a cross-module invariant;
- security, authority, privacy, destructive action, or durable-data behavior
  depends on the change;
- migration, retry, recovery, concurrency, or idempotency paths can corrupt or
  strand state;
- generated fixtures, snapshots, baselines, or proof tests are vulnerable to
  wrong-cause success;
- a material exception, authority, identity, ordering, precedence, or
  persistence boundary;
- cross-module invariants or a high-lock-in public contract changed; or
- validation exposed a meaningful residual that a different implementation
  family could independently attack and patch.

Under this fallback, skip entry only when all are true:

- the change is mechanical, local, and readily reversible;
- it changes no runtime behavior, validation meaning, authority, schema,
  identity, ordering, precedence, persistence, external I/O, or cross-module
  contract;
- direct signals exercise both the intended result and its relevant failure
  path; and
- no alternate-input, exception, integration, or wrong-cause residual remains
  for another implementation family to attack.

Only under this fallback, when it is unclear whether every skip condition holds, a cheap
read-only subagent may challenge only the mechanical classification. Give it
the exact diff, intended transformation, and observed checks; ask whether every
skip condition is proven. Any `no` or `unknown` answer reclassifies the work as
non-trivial. The subagent must not patch or substitute for de-correlated review.
Do not launch this check when the classification and deterministic evidence are
already clear, and do not pin a runtime model in this skill.

When review is required, enter delegated review-and-patch using the active
repository's delivery and prompt-rendering rules; do not substitute self-review.
Report exactly one `review_routing_status`:

- `routed`: the authorized delivery completed. For operator courier, the
  complete paste-ready prompt was rendered with the reviewer who-constraint;
  a concrete receiver may remain pending if the overlay allows it. For
  execution, an eligible different-vendor reviewer and route were verified
  and the pass was dispatched. Neither status claims review or adjudication
  completed; say which delivery occurred and what remains.
- `blocked`: required entry could not complete its authorized delivery because
  a real binding or gate is missing. Name that blocker. A receiver identity
  deferred by the courier policy is not a prompt-preparation blocker, and a
  blocked execution must never silently become `not_needed`.
- `not_needed`: the bound repository predicate does not require review, or,
  when no policy is bound, all generic mechanical skip conditions hold. Give
  one sentence identifying the applicable rule and decisive reason. A review
  obligation carried into this unit closes under the repository's own closure
  rule for carried obligations; where that rule allows only `routed` or
  `blocked`, do not report `not_needed`.

## Closeout

Report only observed facts:

```yaml
SUCCESS_CONTRACT:
IMPLEMENTATION:
VALIDATION:
WRONG_CAUSE_CHECKS:
RESIDUALS:
REVIEW_ROUTING_STATUS:
NEXT_OPERATOR_ACTION:
```

## Trigger and evidence boundary

- Positive triggers: `/success-implement`; `success implement this authorized
  fix`; `success-implement this authorized fix`; "use the success-implement
  workflow for this authorized fix"; "success implement" or "success-implement"
  used as an instruction anywhere inside a planning request, such as "plan
  this feature and success implement the rollout" or "success-implement the
  signals for this plan" -- it need not be a standalone command or formally
  described as co-invocation.
- Negative triggers: ordinary planning that never uses the instruction;
  diagnosis or review without a requested patch; quoting, discussing, or asking
  about the phrase (for example "what does success-implement do?" or "should
  we use success-implement here?"); ordinary implementation that does not
  explicitly invoke this skill.
- Packaging, installation, resolver behavior, deployment, and readiness remain
  separate lifecycle claims.
