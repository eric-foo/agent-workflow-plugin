---
name: workflow-implementation-scoping
description: Source-only workflow-kernel candidate for read-only implementation scoping. Consumes an accepted `READY_FOR_SCOPING` plan or other concrete accepted plan and produces a compact non-executing implementation route with ordered `STEP-*` steps, touch points, validation, stop conditions, and blockers. Never implements, edits, installs, deploys, stages, commits, pushes, claims resolver/plugin readiness, or produces prompts, handoffs, or wrappers; route prompt creation to `workflow-prompt-orchestrator`.
---

# Workflow Implementation Scoping

## Purpose

Convert an accepted and sufficiently concrete plan into a detailed
non-executing **Implementation Route** before source-changing work begins.

Core flow:

```text
accepted READY_FOR_SCOPING plan -> implementation scoping -> implementation route -> separate authorized implementation path
```

The skill answers:

```text
Given this accepted plan, what should an implementer inspect, change,
validate, avoid, stop on, and keep out of scope?
```

The primary output is the route, not the verdict. A readiness-only answer is
invalid unless route construction is blocked.

The route follows the accepted direction's output type. What gets implemented
may be code, an architecture or design artifact, a spec or doc, or similar;
scope and validate against what the direction actually produces, and do not
presume code or code-shaped validation unless the direction is code.

This skill never implements. It does not edit files, create runtime code,
write docs, scaffold systems, run migrations, update generated outputs,
install or deploy skills, stage, commit, push, create executable patch queues,
or claim implementation completion, validation success, deployment readiness,
resolver behavior, or plugin readiness.

The active project overlay owns source hierarchy, artifact roles, protected
paths, validation gates, review lanes, output destinations, edit permission,
implementation authority, rollback rules, lifecycle authority, and execution
contracts.

## Stage Boundary

Use this skill after feature planning or another planning path has produced an
accepted plan that is concrete enough to scope.

Preferred upstream input from an accepted feature plan or equivalent:

- verdict: `READY_FOR_SCOPING`;
- selected feature shape;
- selected proof slice;
- accepted assumptions;
- cut scope;
- validation-as-learning;
- documentation impact;
- source gaps or blockers;
- owner or user acceptance.

Other accepted plans may be scoped if they are concrete enough to imply
behavior, artifacts, interfaces, likely touch areas, acceptance criteria, and
validation expectations.

Do not reopen feature planning by default. If a feature-shape decision,
proof-slice decision, or product decision is still missing, return
`RETURN_TO_FEATURE_PLANNING` instead of inventing scope.

If the plan is accepted and the upstream direction is stable, but scoping would
need to invent required behavior, non-goals, interface or contract boundaries,
acceptance criteria, or handoff intent before it can build a safe route, return
`RETURN_TO_SPEC_WRITING` with the specific missing contract question instead of
inventing intent or producing a vague route. Use this return only when the
missing contract would materially change the route or would be reasonably
contested by a reviewer or downstream binding actor.

If the user only wants a prompt, wrapper, next-thread handoff, model handoff,
or formatting of existing work, route to prompt orchestration. If the user
asks for formal review, require a bound review lane.

## Direct-Entry Source And Context Intake

When this skill is invoked directly, do not require a prior repo-context
packet. Reorient from the current repository before scoping: read local
instructions, the accepted plan source, user-named targets, material workspace
facts, and local overlay or equivalent authority needed for the route claim.

If a repo-context packet, task-local context pack, repo map, summary, or prior
thread note is provided, use it only as orientation. It does not replace plan
intake, source-read ledger, source impact mapping, validation expectations,
route blockers, or final route judgment. Reread decisive current source or
bound authority before strict or actionable claims.

## Goal Handoff Intake

When a `goal_handoff` is supplied, use `anchor_goal` and `success_signal` as
plan-intake context: they help check whether the accepted plan and resulting
route still serve the current workstream outcome. Treat `long_term_goal` as
horizon context only.

A `goal_handoff` does not substitute for an accepted concrete plan,
acceptance criteria, validation expectations, source map, route authority, edit
permission, or implementation-start readiness. Do not mutate `anchor_goal` or
`success_signal` silently. If the accepted plan or route would optimize for a
different outcome, surface the conflict before producing the route.

## Operating Profiles

Select exactly one profile. Use `scoping_before_implementing` by default. Use
`standard` only when risk requires the fuller route surface, or when the user
explicitly asks for full scoping.

Profiles tune scoping depth only. They do not grant edit permission, patch
authority, artifact roles, validation gates, implementation readiness,
lifecycle authority, deployment, install, promotion, resolver behavior, or
plugin readiness.

Do not define aliases for `scoping_before_implementing`.

### Scoping Before Implementing

Use `scoping_before_implementing` for compact, low-risk, single-unit or
few-unit routes. This is the ordinary operating profile. Boundary prose should
stay terse because the stage is already read-only implementation scoping.

`scoping_before_implementing` required compact fields, in this order:

1. plan intake and `acceptance_basis`;
2. frozen decisions;
3. likely touch points;
4. validation expectations;
5. stop conditions;
6. ordered `STEP-*` route;
7. `route_status`;
8. `implementation_start_readiness`;
9. `current_turn_authorization`;
10. next authorized step.

Nothing else is required in the compact profile. Optional sections may appear
only under an explicit trigger and must be omitted otherwise:

- `Source-Read Ledger`: include only when a decisive source read changed a
  route decision and the route would be ambiguous without it.
- `Source Map`: include only when source impact spans multiple modules or
  conflicts with the accepted plan.
- `Frozen And Mutable Fields` (mutable side): include only when the
  implementer might reasonably misread which fields are still open.
- `Implementation Units` (separate from the `STEP-*` route): include only when
  unit grouping changes ordering, dependency, or rollback shape that the
  `STEP-*` route alone does not convey.
- `Rollback / Containment`: include only when containment is materially
  separable from the `stop conditions` already named.
- `Blindspot Matrix`: include only on explicit user request or when a named
  risk would silently corrupt the route.
- `Bloat-Cut Queue`: include only when scope cuts are material to the route
  decision; routine cuts may be a single line under `Next Authorized Step`.
- `Review Timing Advisory`: include only when adversarial review timing is
  material and would change the next move; otherwise omit. In compact routes,
  place it immediately after the ordered `Implementation Route` and before the
  final route/readiness/authorization fields.
- `Lifecycle Boundary`: include only when hard lifecycle is touched.

If the route is bounded to one file plus one validation command and no named
risk requires more, the compact profile should produce a short response. Do
not pad it with optional sections to look complete.

### Standard

Use `standard` only when at least one standard-escalation trigger is present:

- contract-bearing source changes;
- hard lifecycle-sensitive work;
- generated artifacts or paired artifacts;
- validation, evaluator, fixture, or fake-pass-prone behavior changes;
- multiple independent source domains;
- dirty-state ambiguity across target files or artifacts;
- protected paths, artifact-role uncertainty, or output-destination
  uncertainty;
- user-requested full scoping.

Do not escalate to `standard` merely because more sections could be filled in.

Standard output:

- full plan intake;
- frozen and mutable decision split;
- source-read ledger and source map;
- likely touch points and protected boundaries;
- implementation units with dependencies;
- ordered `STEP-*` route;
- dependency ordering;
- blindspot and risk matrix;
- review timing advisory;
- validation matrix;
- rollback and containment notes;
- bloat-cut queue;
- route status and implementation-start readiness.

## Conditional Lifecycle Boundary

Only `scoping_before_implementing` and `standard` are operating profiles for
this skill.

Classify lifecycle sensitivity before turning it into a readiness blocker.

Hard lifecycle includes install, deploy, publish, promotion, resolver behavior,
skill-root mutation, plugin readiness, package or plugin metadata that affects
distribution or resolver-visible behavior, and external side effects. When
scoped work touches hard lifecycle, keep the selected profile and add a
lifecycle boundary note:

```text
Lifecycle boundary detected. Implementation cannot begin without
explicit lifecycle authority and validation evidence.
```

Soft lifecycle includes rollback planning, containment, local cleanup, and
package-adjacent notes that do not change distribution, resolver-visible
behavior, install behavior, deployment state, or published artifacts. Soft
lifecycle may require a containment note, but it must not automatically block
implementation readiness unless a hard lifecycle authority, validation gate,
artifact role, write boundary, dirty-state policy, or output destination is
missing.

Do not convert hard or soft lifecycle work into a readiness claim.

## Authority Split

Separate these fields in every result:

- `route_status`: whether the non-executing route is source-backed and
  bounded.
- `implementation_start_readiness`: whether source-changing implementation may
  begin after scoping.
- `current_turn_authorization`: what this turn is allowed to do.

`route_authority` means permission and evidence to produce a read-only,
non-executing route. `implementation_authority` means permission to change
source, emit patches, create executor-ready patch queues, or claim
implementation-start readiness. Bind route authority before implementation
authority.

Default current-turn authorization:

```text
current_turn_authorization: read_only_scoping_only
```

Read-only implementation scoping does not require edit permission. Missing edit
or patch authority may block `implementation_start_readiness`, but it must not
suppress a route that can be bounded safely.

This skill never performs source changes itself. Implementation is always a
distinct phase with its own preflight and authority — even when it runs in the
same turn. By default, when the user asked only to scope, stop after producing
the route and leave the implementation phase to a separate, later turn.

If the user also explicitly authorizes implementation in the same task,
scoping may pass the cleared route to that implementation phase without a new
turn. Scoping alone grants no edit authority. Load spec writing or micro-decision
locking only when explicitly invoked or needed to resolve a material contract
or implementation decision; do not impose an ordered wrapper pipeline.

Hard authorization blockers are reserved for source-changing requests,
protected edits, patch execution, executable patch queues, saved artifacts,
durable output claims, and implementation-start readiness claims.

By default, an implementation route is `chat-output`. Any durable, workflow-run,
or temporary output must bind mode, role, destination, and write authority per
`overlays/binding-contracts/durable-artifact-output-binding-contract.md` before
route generation; missing bindings block with `BLOCKED_OUTPUT_MODE_MISSING`,
`BLOCKED_UNBOUND_ARTIFACT_ROLE`, `BLOCKED_OUTPUT_DESTINATION_UNBOUND`, or
`BLOCKED_BY_AUTHORIZATION`.

## Conditional Implementation Gate

A cleared scoping route is not implementation permission. The implementation
phase binds authority, current targets, and the material route gates before
editing. A source-changing request must block or pause when the route exposes
any of the following; use the existing `route_status`,
`implementation_start_readiness`, and blocker vocabulary:

- material, unclear contract or doctrine propagation across dependent surfaces;
- ambiguous authorized scope or unclear target files (`BLOCKED_SCOPE`,
  `BLOCKED_SOURCE_MAP_INCOMPLETE`);
- a source-loading fork that changes route shape and is not owner-resolved;
- a route size or blast radius that needs owner confirmation
  (`implementation_context_risk: VERY_HIGH`);
- a review or adjudication dependency with
  `adversarial_review: blocked_unbound_review_lane`, or a
  `required_by_bound_gate` checkpoint whose `highest_value_checkpoint` is
  missing, ambiguous, or cannot be reached safely from the route;
- unclear validation or unclear stop conditions
  (`BLOCKED_UNBOUND_VALIDATION_GATE`);
- missing overlay implementation authority, write boundary, artifact role, or
  dirty-state policy (`BLOCKED_BY_AUTHORIZATION` and the canonical authority
  blockers).

When the route clears, scoping remains `read_only_scoping_only`. `Next
Authorized Step` points to the work actually authorized: return the route for
a scoping-only request, or hand it to the authorized implementation phase.
Carry `adversarial_review`, `highest_value_checkpoint`, `review_target`, and
`why_this_checkpoint` without weakening them:

- `recommended` is advisory input to the repository's review policy; it does
  not itself mandate a review, stop implementation, or force a lane sequence.
- `required_by_bound_gate` guards its named checkpoint. The executor may
  proceed only up to that checkpoint, then routes required review under the
  repository's delivery policy and waits for review and home adjudication
  before the dependent step or closeout. A prompt is not completed review.

If a later decision changes the bound touch points, validation, or authority,
refresh the affected route bindings before implementation. Reuse still-current
bindings; no mandatory spec/micro cycle or new status family is introduced.
The overlay continues to own edit permission, validation, and write boundaries.

## Plan Intake Gate

Classify the input before scoping. Record `acceptance_basis` as one of:

- `accepted_explicit`: a current upstream plan carries owner/user acceptance, or
  the current user explicitly accepts the named plan as the source for scoping.
- `accepted_inferred_from_user_request`: the current user asks to scope a
  concrete plan but does not explicitly state that the plan is accepted.
- `not_accepted`: acceptance is missing, stale, superseded, contradicted, or
  owner acceptance is not available.

`accepted_inferred_from_user_request` may support `ROUTE_COMPLETE` when the plan
is concrete and source-backed, but it cannot by itself support
`READY_FOR_IMPLEMENTATION`. Use `READY_FOR_IMPLEMENTATION` only when acceptance
is explicit and every readiness binding is present.

A sufficiently concrete accepted plan includes objective, expected behavior
change, affected area or inferable touch points, acceptance criteria,
non-goals or cut scope, and enough constraints to avoid invention.

Required intake fields:

- plan source, acceptance status, and acceptance basis;
- objective and expected observable change;
- selected feature shape or equivalent accepted direction;
- selected proof slice or equivalent acceptance target when feature-originated;
- non-goals and explicitly rejected scope;
- constraints and assumptions;
- affected user, operator, or downstream consumer when relevant;
- likely touch areas or enough source context to infer them safely;
- acceptance criteria or success evidence;
- supplied `goal_handoff` fields when present, as context rather than plan
  acceptance;
- validation expectations, if known;
- frozen decisions and mutable fields.

Generic blockers (full semantics in [Status Vocabulary](#status-vocabulary)):

- `BLOCKED_INPUT_PLAN_UNACCEPTED`: plan unaccepted, stale, superseded, or owner-unsigned.
- `BLOCKED_PLAN_TOO_ABSTRACT`: plan lacks implementation-relevant behavior, artifacts, interfaces, touch areas, acceptance criteria, or validation expectations.
- `RETURN_TO_FEATURE_PLANNING`: missing feature-shape, proof-slice, product, or cut-scope decision.
- `RETURN_TO_ARCHITECTURE_PLANNING`: target architecture, core/satellite boundary, invariant, interface boundary, or deferred implementation implication must be decided first. Do not use spec writing to choose architecture.
- `RETURN_TO_PLANNING`: non-feature plans need upstream planning before scoping.

Use `RETURN_TO_SPEC_WRITING` when the plan is accepted and upstream direction is
stable enough, and all of these are true:

- the missing contract is not already stated in the accepted plan, architecture
  decision, feature plan, spec handoff, or other upstream basis;
- scoping would otherwise need to invent or choose intent;
- scoping's guess would materially change the route or be reasonably contested
  by a reviewer or downstream binding actor.

Qualifying implementation-facing contract elements:

- required behavior;
- non-goals or cut boundaries;
- interface, prompt, schema, artifact, error-shape, invariant, or handoff
  contracts;
- testable acceptance criteria;
- downstream handoff intent;
- review thresholds only when reviewers would otherwise argue the intent of the
  deliverable itself, not style, taste, or process.

Do not use `RETURN_TO_SPEC_WRITING` for missing product, feature, proof-slice,
architecture, or cut-scope decisions; return to the owning planning lane
instead. Do not use it when the plan is so vague that a spec could only invent
the missing direction; use `BLOCKED_PLAN_TOO_ABSTRACT` or the nearest planning
return.

Do not use `RETURN_TO_SPEC_WRITING` for uncertainty this skill owns, including
source-map gaps, artifact-location or section-location gaps, edit order,
section order, adjacent-artifact discovery, validation procedure, dry-run or
preview procedure, rollback or containment, file touch discovery, activation
coverage risk, drift risk, overlay conflict, implementation risk, dirty-state
handling, lifecycle authority, or missing validation commands.

Each `RETURN_TO_SPEC_WRITING` must carry a new specific reason. If a visible
prior spec-writing pass already attempted to resolve the same reason and the
same gap remains, do not bounce back to spec writing for the same gap. Return
to the owning upstream lane, such as `RETURN_TO_ARCHITECTURE_PLANNING`,
`RETURN_TO_FEATURE_PLANNING`, or `RETURN_TO_PLANNING`, or use
`BLOCKED_PLAN_TOO_ABSTRACT` when the direction is too thin for any spec to
stabilize safely.

When multiple blockers apply, choose the first applicable primary blocker in
this precedence order:

1. `BLOCKED_INPUT_PLAN_UNACCEPTED` for unaccepted, stale, or superseded input.
2. `RETURN_TO_FEATURE_PLANNING`, `RETURN_TO_ARCHITECTURE_PLANNING`, or
   `RETURN_TO_PLANNING` for missing upstream product, feature-shape,
   proof-slice, architecture, cut-scope, or non-feature planning decisions.
3. `RETURN_TO_SPEC_WRITING` for accepted, stable-direction plans that need
   contract stabilization before route construction.
4. `BLOCKED_PLAN_TOO_ABSTRACT` for plans too vague to imply implementation
   behavior, artifacts, touch points, acceptance criteria, or validation.
5. `BLOCKED_SOURCE_MAP_INCOMPLETE` when source impact cannot be bounded.
6. `BLOCKED_UNBOUND_VALIDATION_GATE` when route shape is otherwise bounded but
   required validation gates or pass/fail semantics are unbound.
7. `BLOCKED_UNBOUND_LIFECYCLE_AUTHORITY` for missing hard lifecycle authority.
8. Other authority, artifact-role, output-destination, or patch-authority
   blockers.

## Required Bindings

To write a non-executing route, bind or source:

- accepted plan source, owner, revision, freshness marker, acceptance status,
  and acceptance basis;
- authority order for conflicts between the plan, repository instructions,
  overlays, and current source;
- source hierarchy and source-read scope sufficient to inspect affected areas;
- target modules, files, artifact roles, interfaces, generated artifacts, or
  boundaries when a route step depends on them;
- validation expectations sufficient to define step-level verification, even
  when final runnable commands remain overlay-owned;
- frozen decisions, mutable fields, and decisions the implementer must not
  reopen.

Before returning `READY_FOR_IMPLEMENTATION` or `READY_WITH_WARNINGS`, also
bind:

- protected paths, generated-artifact rules, and write boundaries;
- artifact roles, read/write permissions, destinations, paired artifacts, and
  freshness markers for every touched role;
- validation gates, pass/fail/blocked/not-run semantics, required environment
  notes, and evidence locations;
- output mode and implementation permission for the next actor;
- dirty-state policy for target files or artifacts;
- hard lifecycle authority when hard lifecycle work is touched.

Missing project authority must not be converted into an assumption. Unknown
repo paths, overlays, validation gates, protected paths, artifact roles,
generated-artifact pairs, dirty-state constraints, or lifecycle boundaries
must be marked unknown or blocked, not inferred.

Use canonical blockers for missing authority, including
`BLOCKED_BY_AUTHORIZATION`, `BLOCKED_UNBOUND_PATCH_AUTHORITY`,
`BLOCKED_UNBOUND_VALIDATION_GATE`, `BLOCKED_UNBOUND_ARTIFACT_ROLE`,
`BLOCKED_OUTPUT_MODE_MISSING`, `BLOCKED_OUTPUT_DESTINATION_UNBOUND`,
`BLOCKED_SOURCE_MAP_INCOMPLETE`, `BLOCKED_AUTHORITY_ORDER_MISSING`, and
`BLOCKED_UNBOUND_LIFECYCLE_AUTHORITY` when applicable.

## Implementation Route Reasoning

Use this built-in route-construction discipline while building implementation
units and ordered `STEP-*` steps. It is part of this skill's normal behavior
and does not require a separate `workflow-deep-thinking` invocation.

- Use the smallest complete intervention by finding the smallest complete implementation route.
  Complete means the route has the fewest moving parts that still fix the
  route-reasoning gap and preserve source-map, accepted-decision, dependency,
  validation, rollback, dirty-state, authority, fake-pass, and
  return-to-planning boundaries.
- Treat the user's suggested implementation path, prior-thread route, obvious
  edit order, and nearest existing pattern as candidates, not defaults.
- Keep accepted upstream product, feature, architecture, proof-slice, and
  cut-scope decisions frozen unless the user explicitly reopens them.
- Compare materially different route shapes when real, including single local
  patch, contract-first, validation-first, source-map-first, narrow vertical
  slice, dirty-state preflight, validator/fixture before dependent behavior,
  defer or cut, return to scoping, and return to planning.
- Choose route shape and step order by implementation safety: source-map
  sufficiency, dependency order, validation placement, rollback or containment,
  dirty-state collision risk, fake-pass risk, lifecycle boundaries, authority
  gaps, and context-fracture risk.
- Put shared contracts, schemas, validation harnesses, generated pairs,
  migration-like surfaces, and source-map clarification before dependent target
  behavior when correctness depends on them.
- Keep fake-pass-prone validator, fixture, or test changes separate from
  dependent runtime or target behavior unless the route can justify a narrow
  mechanical coupling with observable validation.
- Do not underfix by choosing a route that is simpler only because it skips
  required reasoning, source mapping, validation, containment, dirty-state
  handling, authority boundaries, fake-pass prevention, or a precise
  return-to-planning/scoping blocker.
- Prefer narrower route slices only when they materially reduce source-loading,
  rollback, stale-source, validation ambiguity, authority collision, context
  bursting, or fake-pass risk. Do not split work solely to add ceremony, and do
  not collapse work so broadly that material risk is hidden.
- Preserve uncertainty and state what would change the route. If the accepted
  plan, source map, validation gate, authority, artifact role, dirty-state
  policy, lifecycle boundary, or upstream decision is too thin to route safely,
  return the precise scoping blocker instead of inventing steps.

## Scoping Workflow

1. **Bind route authority.** State loaded repository rules, overlay sources,
   accepted plan sources, authority order, source-read scope, output mode, and
   current-turn authorization.
2. **Run plan intake.** Confirm the plan is accepted, concrete, current, and
   within requested authority.
3. **Separate frozen and mutable fields.** Frozen decisions must not be
   reopened without explicit permission. Mutable fields may be clarified during
   scoping. State `acceptance_basis` and apply the readiness cap for inferred
   acceptance.
4. **Create a decision-backed source-read ledger.** Record only source reads
   that changed or bounded a route decision, authority claim, touch point,
   validation expectation, lifecycle classification, or blocker. Omit routine
   orientation reads unless they affected one of those decisions. Keep loading
   narrow until a missing source could change the route.
5. **Map source impact.** Identify likely touched files, modules, artifacts,
   artifact roles, interfaces, commands, generated outputs, or data boundaries.
   Mark unknowns, conflicts, protected paths, and generated-artifact pairs.
   Run a contract-impact gate when the route may change something downstream
   authors, reviewers, tests, tools, prompts, examples, or harnesses must
   conform to. File type is not the trigger. If the work is contract-sensitive,
   include or block on a source-backed contract map that names authority
   sources, source precedence, artifact roles, field or behavior hierarchy,
   invariants, mirrored surfaces, patch targets, readback expectations, and
   unresolved gaps.
6. **Run implementation route reasoning.** Compare plausible route shapes and
   step order using the Implementation Route Reasoning discipline. Keep accepted
   upstream decisions frozen, and return the precise blocker when a missing
   source, validation, authority, artifact, dirty-state, lifecycle, or upstream
   planning decision prevents a safe route.
7. **Build implementation units.** Define stable implementation units with purpose,
   dependencies, target areas, non-goals, validation expectations, rollback
   notes, and handoff expectations.
8. **Write the Implementation Route.** Turn units into ordered `STEP-*` route
   steps. State what to inspect before each edit, behavior-level edit intent,
   likely touched area, dependency, verification, stop condition, containment
   note, and deferred or cut work.
9. **Check dependency order.** Sequence units so shared contracts, schemas,
   generated artifacts, migrations, docs, tests, and cleanup do not hide
   correctness risk.
10. **Run a blindspot matrix.** Check authority gaps, stale assumptions, hidden
   coupling, protected paths, generated-artifact pairs, data/schema risk,
   migration risk, UX or docs drift, test gaps, security or privacy risk,
   rollback difficulty, lifecycle boundaries, and fake-success paths.
11. **Mark review timing advisory.** Identify whether adversarial review would
   materially reduce route risk and where it would be most valuable: before a
   risky step, after a risky step before dependents build on it, or after all
   steps before closeout. Keep this as advisory routing unless a bound review
   gate requires more. Do not run review, claim a review verdict, or use review
   to compensate for missing route evidence.
12. **Build a validation matrix.** Separate scoping validation from
    implementation validation, blocked validation, and intentionally not-run
    validation. Tie each validation row to the risk it covers, expected
    evidence, and what failure means.
13. **Write rollback and containment notes.** State checkpoints, stop
   conditions, reversible sequence, and blast-radius limits.
14. **Build the bloat-cut queue.** Separate must-keep, likely-keep, defer, and
   cut. Cut speculative refactors, broad redesign, extra review loops,
   unrelated cleanup, and nice-to-have tests unless required by the accepted
   plan or overlay.
15. **Check for VERY_HIGH context risk.** Only when the scoped route would
   likely overflow, destabilize, or confuse a single implementation context,
   mark `implementation_context_risk: VERY_HIGH`. This is an exceptional
   warning, not a routine sizing label. Use it for routes with multiple
   independent source domains, broad contract-bearing changes, validator,
   fixture, or test behavior plus dependent target behavior, large generated or
   paired artifacts, heavy source-reading requirements, lifecycle-sensitive
   surfaces, dirty-state ambiguity across target files, rollback ownership that
   must stay separate, or similar context-fracture risk. For all lower,
   ordinary, unknown, or merely broad risk, omit the context-risk output
   entirely.
16. **Return the route verdict and next step.** Give `route_status`,
   `implementation_start_readiness`, `current_turn_authorization`, blockers,
   warnings, and the next authorized step. The verdict must not replace the
   route.

## Implementation Units

Use the accepted plan's unit vocabulary when it is safe, unambiguous, and
accepted. Otherwise use short implementation-unit titles. The ordered
Implementation Route is the required execution sequence and must use `STEP-*`
identifiers.

Do not introduce a second required route-label family.

Each scoped unit should state:

- unit id and title;
- source-backed purpose;
- target files, modules, artifacts, or artifact roles;
- explicit non-goals;
- dependencies and ordering constraints;
- likely edit type and risk level;
- required validation expectations and evidence;
- rollback or containment notes;
- open questions or blockers;
- handoff notes for the implementation actor.

If the accepted plan already contains unit IDs, preserve them only when they
are unambiguous, safe, and accepted. If they are unsafe or ambiguous, map them
to revised implementation-unit titles and explain why.

## Implementation Route

The Implementation Route is the implementer's step-by-step path through scoped
work. It should be concrete enough that a separate executor can begin without
re-planning after implementation authority is granted, while still respecting
the active overlay's edit permission and validation gates.

Every non-blocked route must contain ordered `STEP-*` route steps. Each route
step should state:

- route step id, such as `STEP-01`;
- linked implementation unit, or reason the step is preflight, validation,
  containment, or deferred work;
- exact intent of the step;
- source or artifact area to inspect before editing;
- files, modules, commands, generated artifacts, or artifact roles likely
  touched, using exact paths or commands only when sourced or overlay-bound;
- behavior-level edit action or artifact-change intent, not code, patch hunks,
  generated output, or migration content;
- contract-impact gate result when the step touches contract-bearing source:
  `local_patch`, `contract_sensitive_with_map`, or
  `contract_sensitive_blocked`;
- dependency or prerequisite;
- local verification to run after the step, or the gate that must be bound
  before verification can be named;
- advisory review checkpoint when review timing is material:
  `none`, `useful_before_this_step`, `useful_after_this_step`, or
  `defer_to_post_implementation_scope_check`;
- stop condition that blocks continuing;
- rollback or containment note when relevant.

Do not add a per-step status for ordinary route steps just because the scoping
turn is read-only. Missing edit or lifecycle authority is not a route-step
blocker; state it once in `Implementation Start Readiness`,
`Current Turn Authorization`, `Blockers / Warnings`, or `Next Authorized Step`.
Use step-level blockers only for route-internal blockers such as missing source,
artifact role, validation gate, dependency, ownership, or a deliberate `defer`
or `stop`. Repeating `blocked_pending_authorization` under every `STEP-*` hides
the route and fails this skill.

Route steps should be ordered for implementation safety, not narrative
convenience. Prefer narrow vertical progress when that exposes risk early. Put
shared contract, schema, generated-artifact, migration, or validation harness
work before dependent edits when correctness requires it. Put cleanup, polish,
and optional refactors after correctness-bearing changes unless the accepted
plan requires otherwise.

Do not write code, patches, generated outputs, prompts, migrations, or docs
while creating the route. Do not include code snippets or command text unless
the command is an overlay-bound validation gate or a read-only inspection
command required to understand scope.

## Review Timing Advisory

Implementation scoping may include advisory routing for adversarial review when
review timing would materially reduce implementation risk. This advisory tells
the next actor where review has the highest leverage; it is not formal review,
approval, acceptance, validation success, source approval, implementation
readiness, or a substitute for missing route evidence.

Review lanes own formal review authority, reviewer permissions, verdict
vocabulary, output binding, and patch-queue routing. A scoping result may point
to a review target, but it must not run review or claim review completion.

Use these fields when review timing is material:

- `adversarial_review`: `not_needed`, `recommended`,
  `required_by_bound_gate`, or `blocked_unbound_review_lane`;
- `highest_value_checkpoint`: `before_STEP-*`, `after_STEP-*`,
  `after_all_steps_pre_closeout`, or `not_applicable`;
- `review_target`: route plan, `STEP-*` slice, completed diff, named source
  area, artifact role, or `not_applicable`;
- `why_this_checkpoint`: the specific risk this timing exposes earlier or more
  cleanly than alternatives;
- `boundary`: `Advisory routing only; not review, approval, validation,
  acceptance, or readiness.`

The authorized executor consumes this advisory under the Conditional
Implementation Gate above. A recommendation follows repository review policy;
a required checkpoint remains binding until review and home adjudication
resolve. Scoping itself does not run review, render its commission, apply a
patch, adjudicate, or claim review completion.

When emitted in `scoping_before_implementing`, place this advisory immediately
after the ordered `Implementation Route` so the review checkpoint remains
visible before the final status/readiness routing block.

Recommend adversarial review when the route or a route step touches broad or
cross-cutting scope, contract-bearing source, validation/evaluator/fixture or
fake-pass paths, reusable workflow-kernel behavior, authority/blocker/lifecycle
or review-routing semantics, non-code source artifacts whose acceptance would
need artifact review, dirty-state ambiguity affecting trusted source, hard
lifecycle-sensitive surfaces, or user-requested review.

Use `adversarial_review: required_by_bound_gate` instead of `recommended` only
when one of these source-backed gates applies:

- loaded repo-local authority explicitly requires review before the next route
  step, propagation, use, acceptance, package/deploy surface, or strict closeout
  claim;
- loaded workflow-kernel risk rules make review a checkpoint because the route
  touches source-critical or fake-pass-prone behavior whose mistakes would be
  propagated, accepted, packaged, deployed, or relied on by dependent work before
  a later review could prevent damage.

The gated move may be architecture-level or doctrine-level, such as a target
architecture, invariant, interface boundary, authority semantic, validation
contract, lifecycle rule, or deferred implementation implication. It may also
be reusable workflow-kernel behavior, authority or review-routing semantics,
validation/evaluator/fake-pass surfaces, hard lifecycle surfaces, or
source-critical parser/extraction semantics where downstream steps or strict
closeout would treat the result as trustworthy. An already observed delegated
review return with multiple blocker, major, or material findings in the same
workstream is risk evidence for this gate until home-model adjudication resolves
the returned findings.

Ordinary doctrine-change propagation, downstream-surface checks, route
importance, or possible reviewer usefulness are not enough by themselves. The
review requirement must cite the loaded authority or source-backed risk factor,
name the dependent step or claim the checkpoint protects, and choose a concrete
`highest_value_checkpoint` before that dependency or strict closeout.

Do not recommend adversarial review when the route is local, reversible, covered
by existing validation, and does not alter shared contracts, validation
behavior, authority semantics, lifecycle semantics, or reusable workflow-kernel
behavior. Use `adversarial_review: not_needed` for those routes.

Choose the checkpoint by risk shape:

- use `before_STEP-*` when route correctness, authority order, shared
  contracts, lifecycle boundaries, or validation semantics should be challenged
  before source-changing work builds on them;
- use `after_STEP-*` when review needs the changed slice but should happen
  before dependent route steps compound the risk;
- use `after_all_steps_pre_closeout` when the main risk is in the completed
  diff or artifact readback and earlier review would mostly add ceremony;
- use `not_applicable` only when the route is narrow, local, mechanically
  checkable, structurally boring, and no user request or bound gate requires
  review.

Do not force adversarial review after all steps when the highest-risk decision
occurs earlier. Post-implementation review is too late for mistakes in shared
contracts, authority semantics, lifecycle boundaries, or validation harnesses
that dependent steps rely on.

## Validation Matrix

The validation matrix must distinguish:

- **scoping validation:** checks that this pre-implementation route is sourced,
  bounded, and internally consistent;
- **implementation validation:** gates the next actor must run after source
  changes;
- **blocked validation:** gates required for implementation start but unbound
  or not runnable yet;
- **not-run validation:** gates intentionally deferred with owner-accepted
  rationale.

Each validation row must include the validation type, linked implementation
unit or `STEP-*` route step, risk covered, expected evidence, what failure
means, and whether the gate is scoping-only, implementation-time, blocked, or
intentionally not run.

Do not imply implementation validation has passed before implementation
occurs. Do not claim implementation start readiness when mandatory gates are
unbound.

## Recommended Implementation Model

Every non-blocked route ends with one footer line in this exact form:

```text
Recommended Implementation Model: <lane> — <why in 5-8 words>.
```

`<lane>` is either `mechanical_lane` or `judgment_lane`, resolved per
`overlays/binding-contracts/executor-lane-binding-contract.md`:

- When the active overlay binds the lane, emit the bound concrete model
  identifier (e.g., the project's mechanical model).
- When the lane is unbound, emit the abstract name (`mechanical_lane` or
  `judgment_lane`) verbatim. Do not invent concrete identifiers.

Pick `judgment_lane` when the route needs judgment: novel contract or
interface design, multi-step reasoning where one bad step compounds,
fake-pass-prone validation, subtle correctness on shared or contract-bearing
surfaces, or material tradeoffs the user did not pre-decide.

Pick `mechanical_lane` when the route is mechanical: well-bounded
single-file or single-test edits, routine rename/move/format passes,
straightforward fixture or harness updates, or patches where the route
already states what to do and where.

The WHY clause is 5-8 words and names the dominant reason. Examples:

- `task is highly mechanical, single file`
- `may need judgment on contract design`
- `fake-pass-prone validation needs care`
- `multi-step reasoning, compounding risk`
- `routine rename, well-bounded scope`

Place this line last in the message, after `Next Authorized Step` and after
any `**VERY HIGH CONTEXT BURSTING RISK**` note. Omit it for blocked routes
that have no downstream implementer.

This footer is advisory only — not a prompt, wrapper, routing instruction,
executor briefing, readiness claim, validation claim, or authority. Actual
executor selection, model routing, and prompt-level lane enforcement belong
to the active overlay and `workflow-prompt-orchestrator`.

## Minimum Valid Route

Every non-blocked route must include:

- at least one ordered `STEP-*` route step;
- touch points, or explicit unknowns when touch points cannot be safely named;
- validation expectation or a named blocked validation gate;
- stop condition;
- `route_status`, `implementation_start_readiness`, and
  `current_turn_authorization`;
- next authorized step;
- one-line `Recommended Implementation Model` footer (see
  [Recommended Implementation Model](#recommended-implementation-model)).

## Status Vocabulary

Return one `route_status`:

- `ROUTE_COMPLETE`: ordered `STEP-*` route steps are source-backed and
  internally bounded.
- `ROUTE_PARTIAL_BLOCKED`: some safe route steps are source-backed, but named
  steps or units are blocked by missing source or overlay bindings.
- `ROUTE_BLOCKED`: route steps cannot be bounded safely from the accepted plan
  and loaded sources.

Return one `implementation_start_readiness`:

- `READY_FOR_IMPLEMENTATION`: explicit acceptance, route, authority order,
  artifact roles, source scope, validation gates, write boundaries,
  dirty-state policy, and next implementation authority are bound.
- `READY_WITH_WARNINGS`: implementation can begin only under named warnings
  where each warning states why proceeding is allowed, containment, validation
  response, and stop condition.
- `BLOCKED_BY_AUTHORIZATION`: implementation start or source-changing work
  exceeds current authority.
- `BLOCKED_UNBOUND_VALIDATION_GATE`: required validation gates or pass/fail
  semantics are unbound.
- `BLOCKED_SOURCE_MAP_INCOMPLETE`: source impact cannot be bounded from loaded
  source.
- `BLOCKED_SCOPE`: scoped units or route steps cannot be bounded safely from
  the current plan and sources.
- `BLOCKED_PLAN_TOO_ABSTRACT`: the plan is too vague, strategic,
  product-level, or underspecified for implementation scoping.
- `BLOCKED_INPUT_PLAN_UNACCEPTED`: the plan is not accepted, stale,
  superseded, or lacks owner acceptance.
- `BLOCKED_AUTHORITY_ORDER_MISSING`: source precedence or conflict rules are
  missing.
- `BLOCKED_UNBOUND_ARTIFACT_ROLE`: a needed artifact role, permission,
  destination, freshness marker, or paired-artifact rule is missing.
- `BLOCKED_UNBOUND_PATCH_AUTHORITY`: patch execution or an executable patch
  queue is requested without bound patch authority.
- `BLOCKED_OUTPUT_DESTINATION_UNBOUND`: a durable artifact or output
  destination required for the strict claim is missing.
- `BLOCKED_UNBOUND_LIFECYCLE_AUTHORITY`: hard lifecycle-sensitive
  implementation authority is missing.
- `RETURN_TO_FEATURE_PLANNING`: feature-shape, proof-slice, product, or
  cut-scope decisions must be made before scoping.
- `RETURN_TO_ARCHITECTURE_PLANNING`: target architecture, core/satellite
  boundary, invariant, interface boundary, or deferred implementation
  implication must be decided before scoping.
- `RETURN_TO_SPEC_WRITING`: accepted upstream direction exists, but behavior,
  non-goals, contract boundary, acceptance criteria, or handoff intent must be
  stabilized before scoping can build a route without inventing intent.
- `RETURN_TO_PLANNING`: non-feature upstream decisions must be made before
  scoping.

Return one `current_turn_authorization`, normally:

- `read_only_scoping_only`

Do not propagate `read_only_scoping_only` into each `STEP-*` as a blocked
status. A bounded source-changing route may be `ROUTE_COMPLETE` while
`implementation_start_readiness` remains `BLOCKED_BY_AUTHORIZATION`.

Use a more specific blocked value only when material:

- `source_change_requested_but_blocked`
- `patch_queue_requested_but_blocked`
- `implementation_start_claim_requested_but_blocked`

Valid combinations include:

- `ROUTE_COMPLETE` with `BLOCKED_BY_AUTHORIZATION` when the route is bounded
  but implementation is not authorized.
- `ROUTE_PARTIAL_BLOCKED` with `BLOCKED_SOURCE_MAP_INCOMPLETE` when some steps
  are safe to describe and others are not.
- `ROUTE_BLOCKED` with `RETURN_TO_FEATURE_PLANNING`,
  `RETURN_TO_ARCHITECTURE_PLANNING`, or `RETURN_TO_PLANNING` when upstream
  decisions are missing.
- `ROUTE_BLOCKED` with `RETURN_TO_SPEC_WRITING` when upstream direction is
  accepted but the implementation-facing contract is too unstable to route.
- `ROUTE_COMPLETE` with `READY_FOR_IMPLEMENTATION` only when explicit
  acceptance, authority, write boundaries, validation gates, dirty-state policy,
  and implementation permission are all bound for a separate implementation
  phase.

False-success guards:

- `ROUTE_COMPLETE` is invalid without a route body.
- `READY_FOR_IMPLEMENTATION` is invalid with unbound validation gates,
  artifact roles, write boundaries, dirty-state policy, or implementation
  permission.
- `READY_FOR_IMPLEMENTATION` is invalid when acceptance is only inferred from
  the current user request.
- `direct_implementation` is invalid as an implementation-scoping result; this
  skill stops after the route and points to a separate implementation phase.
- A verdict footer must not replace the Implementation Route.

## Boundary With Adjacent Methods

Product planning owns customer, buyer, product promise, product shape, proof,
and readiness for feature planning.

Feature planning owns feature shape, proof slice, validation-as-learning,
feature-origin cut scope, and the `READY_FOR_SCOPING` verdict.

Architecture planning owns target architecture, option comparison,
core/satellite boundaries, invariants, interface boundaries, and deferred
implementation implications before route construction.

Implementation scoping owns non-executing route construction after an accepted
plan exists. It may reject vague plans, but it must not silently redo feature
planning.

Spec writing owns required behavior, non-goals, boundary contracts, acceptance
criteria, open-question disposition, and downstream handoff when a binding
downstream actor would otherwise invent intent. Implementation scoping may
return `RETURN_TO_SPEC_WRITING`, but it must not write the spec itself.

Prompt orchestration owns every prompt-shaped artifact: paste-ready prompts,
wrappers, handoffs, next-thread prompts, patch prompts, review prompts, rerun
prompts, executor-ready prompts, output frames, adversarial naming, and saved
prompt destinations. Implementation scoping must not produce any of these,
even in compact, advisory, or "seed" form. The one-line `Recommended
Implementation Model` footer (`mechanical_lane` or `judgment_lane`, resolved
to a concrete model only via the overlay's executor-lane binding) is the
only lane-shaped output scoping emits — advisory only, not routing
authority. If a prompt is the next move, the route's `Next Authorized Step`
must point to `workflow-prompt-orchestrator` and stop.

Review lanes own formal review authority, reviewer permissions, verdict
vocabulary, and patch-queue routing.

Execution contracts own source-changing execution after implementation begins,
including idempotency, residual work, completion evidence, and post-edit
validation.

Postmortem review owns completed-work integrity checks after implementation,
validation, release, or transition claims exist.

## Output Contract

Return the shortest useful route that still includes the compact required
fields. In `scoping_before_implementing`, lead with `Plan Intake`, then
`Frozen Decisions`, then `Likely Touch Points`, then `Validation Expectations`,
then `Stop Conditions`, then the `Implementation Route` (ordered `STEP-*`
steps), then triggered `Review Timing Advisory` when material, then the final
fields (`Route Status`, `Implementation Start
Readiness`, `Current Turn Authorization`, `Blockers / Warnings`, `Next
Authorized Step`). Do not emit the full `standard` section set for compact
routes.

Compact required section labels:

- `Plan Intake`: plan source, acceptance status, and `acceptance_basis`.
- `Frozen Decisions`: decisions the implementer must not reopen.
- `Likely Touch Points`: likely touched files, modules, artifacts, interfaces,
  commands, or artifact roles.
- `Validation Expectations`: validation tied to the route, including
  intentionally not-run validation when material.
- `Stop Conditions`: conditions that block continuing.
- `Implementation Route`: ordered `STEP-*` steps.
- `Route Status`: `route_status`.
- `Implementation Start Readiness`: `implementation_start_readiness`.
- `Current Turn Authorization`: `current_turn_authorization`.
- `Blockers / Warnings`: blockers and warnings (omit when empty).
- `Next Authorized Step`: a one- to two-line routing instruction (e.g.,
  `Prepare implementation prompt with workflow-prompt-orchestrator.`,
  `Start a separate implementation phase for STEP-01 through STEP-03.`,
  `Authorize a separate implementation phase under STEP-01.`,
  `Return to feature planning to resolve <gap>.`). Never a prompt,
  fenced block, wrapper preflight, executor briefing, or `proceed to execute
  STEP-*` phrasing. Under a cleared conditional implementation gate this step may
  instruct same-turn continuation to the next pre-implementation lane (e.g.,
  `Continue to spec writing this turn; implement only if spec clears and
  micro-decision locking returns route_ready: true.`); it still must not issue
  the execute-the-edits go, which the pipeline grants only at the micro tail.
- `Recommended Implementation Model`: required final footer line for
  non-blocked routes — `<lane> — <why in 5-8 words>.` where `<lane>` is
  `mechanical_lane` or `judgment_lane`, resolved to a concrete model via the
  overlay's executor-lane binding when bound. See
  [Recommended Implementation Model](#recommended-implementation-model).

Optional section labels, included only under their own triggers (see
[Scoping Before Implementing](#scoping-before-implementing)):

- `Loaded Bindings`: loaded source and overlay bindings, when material.
- `Profile`: operating profile, when `standard` is selected or when material.
- `Mutable Fields`: mutable fields and reopen-permission notes, when the
  implementer might misread which fields are still open.
- `Source-Read Ledger`: decisive current source reads that changed a route
  decision.
- `Source Map`: source impact map.
- `Implementation Route Reasoning`: smallest complete route shape, material
  rejected route shapes, ordering rationale, and blocker/return-to-planning
  rationale when route reasoning changes the route.
- `Rollback / Containment`: containment notes materially separable from
  `Stop Conditions`.
- `Implementation Units`: implementation units with dependencies, when unit
  grouping changes ordering or rollback shape beyond the `STEP-*` route.
- `Dependency Order`: cross-unit ordering.
- `Blindspot Matrix`: blind spots and risks.
- `Review Timing Advisory`: advisory adversarial review timing, target, and
  boundary when material. In compact routes, place immediately after
  `Implementation Route`, not in the final routing block.
- `Bloat-Cut Queue`: scope cuts or deferred bloat.
- `Lifecycle Boundary`: lifecycle boundary note when material.

These labels are a menu, not a mandatory template. Omit any optional section
whose trigger is not met. The message ends with the `Recommended
Implementation Model` footer (required for non-blocked routes), placed after
`Next Authorized Step` and after the context-risk note when present.

When and only when `implementation_context_risk: VERY_HIGH` is present, append
an eye-catching section after `Next Authorized Step`:

```text
**VERY HIGH CONTEXT BURSTING RISK**
```

That section must briefly state why one-shot implementation is likely to
overflow, destabilize, or confuse context and point to the route's next bounded
implementation move or return-to-scoping/planning move. Omit it completely for
all non-VERY HIGH routes.

## Constraints

- Do not implement code, write downstream artifacts, run migrations, update
  generated outputs, create patches, or modify project files while using this
  skill.
- Do not install, deploy, promote, rename, shadow, stage, commit, or push
  skills.
- Do not treat installed skill copies as source authority.
- Do not import another project's paths, lifecycle rules, validation commands,
  review labels, product facts, artifact destinations, or downstream skill
  names.
- Do not claim implementation start readiness when overlay authority, artifact
  roles, validation gates, source scope, output mode, write boundaries,
  dirty-state policy, or implementation permission are unbound.
- Do not turn Implementation Route Reasoning into product planning, feature
  planning, architecture planning, generic deep thinking, or a required separate
  reasoning-support invocation.
- Do not run adversarial review, claim review completion, or treat advisory
  review timing as approval, acceptance, validation success, source approval,
  implementation readiness, or a replacement for missing route evidence.
- Do not produce prompts, wrappers, handoffs, next-thread prompts, patch
  prompts, review prompts, model-routed executor instructions, or any
  prompt-shaped artifact; defer all prompt creation to
  `workflow-prompt-orchestrator`. The required one-line `Recommended
  Implementation Model` footer is the only exception and is advisory only.
- Do not pad a compact route with optional sections when no named risk
  justifies them; the trigger list under
  [Scoping Before Implementing](#scoping-before-implementing) is binding.
- Do not add extra process steps when a smaller scoped route or direct return
  to feature planning is safer.

## Quality Bar

A valid implementation-scoping result must:

- leave the next actor with a precise authorized step or blocked result;
- keep accepted upstream product, feature, architecture, proof-slice, and
  cut-scope decisions frozen unless the user explicitly reopens them;
- check the accepted upstream basis before returning to spec writing, so a
  missing reread is not converted into ceremony;
- only return `READY_FOR_IMPLEMENTATION` when acceptance is explicit and every
  readiness binding listed in [Required Bindings](#required-bindings) is bound;
- emit the one-line `Recommended Implementation Model` footer for non-blocked
  routes and no other prompt-shaped artifact.
