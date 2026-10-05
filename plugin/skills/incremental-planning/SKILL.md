---
name: incremental-planning
description: Workflow-kernel skill for incremental planning and next-move sequencing. Use when explicitly invoking `incremental-planning`, asking for incremental planning, or asking which next move compounds most from a current product, proof, foundation, review, or planning state, such as "what should we do next?", "what is the highest-compounding next step?", "should we build more foundation or proceed?", "which direction compounds most?", "after this artifact, where do we go?", "am I overengineering?", or "what's the next product move?". Do not use for ordinary product planning, feature planning, implementation scoping, artifact/code review, prompt orchestration, repo orientation, or generic routing unless sequencing judgment across plausible next moves is requested.
---

# Incremental Planning

## Purpose

Decide the next product or planning move that compounds learning, proof power,
and decision quality most from the current visible state.

Core question:

```text
Given the current product, proof, foundation, review, or planning state, what
next action compounds product learning, proof power, and decision quality most?
```

This skill compares plausible next moves. It does not route first. It selects a
plain-language action, artifact, or question first, then optionally names a
downstream workflow only after the compounding move has been chosen.

Invalid recommendation:

```text
Run a planning workflow.
```

Valid recommendation:

```text
Clarify the buyer and decision trigger because proof would otherwise validate
the wrong audience. If structured product exploration is needed, do it after
that move is selected.
```

## Boundary

This skill is advisory and planning-only. It never implements, edits files,
creates runtime code, scaffolds systems, installs or deploys skills, stages,
commits, pushes, creates durable workflow artifacts, runs proof, performs formal
review, emits patch queues, or claims validation success, readiness, acceptance,
deployment, resolver behavior, plugin readiness, or source-of-truth status.

The active project overlay owns product facts, customer facts, source
hierarchy, artifact roles, validation gates, review lanes, output destinations,
protected paths, write permissions, lifecycle authority, and local downstream
methods.

## Source And Authority

Use zero-config advisory mode by default:

- read only the current user request, user-provided context, named artifacts,
  local repository instructions, and visible source needed to compare moves;
- label material claims as `user-stated`, `sourced fact`,
  `repo-visible inference`, `assumption`, `source gap`, or `not proven`;
- keep source loading narrow unless a missing source could change the selected
  next move;
- treat repo maps, context packets, task-local context packs, summaries, prior
  thread notes, installed copies, and generated artifacts as orientation only
  unless local authority explicitly binds them.

Strict claims require project-owned authority. Missing authority blocks the
strict claim, not ordinary advisory sequencing.

Use `STATE_TOO_THIN` only when fewer than two plausible moves can be compared
without inventing facts. Do not use it merely because strict proof,
acceptance, or complete product facts are missing.

When visible evidence establishes enough direction to compare moves, return an
assumption-labeled recommendation instead of punting. Name the material
assumption, the smallest complete check that would change the answer, and any strict
claim that remains `not proven`.

## Goal Handoff Intake

When a `goal_handoff` is supplied, treat `anchor_goal` as the current workstream
optimization target and `success_signal` as the output-fit check for the
recommended next move. Use `long_term_goal` as horizon context, not as a reason
to broaden the immediate move.

Do not mutate `anchor_goal` or `success_signal` silently. If the move that would
compound most conflicts with the supplied handoff, surface the conflict before
recommending. If no `goal_handoff` is supplied, do not invent one; proceed from
the visible state or return `STATE_TOO_THIN` when the move set cannot be compared
without fiction.

## Public Flow

Every run follows this sequence:

1. **State intake.** Identify the current product, proof, foundation, review, or
   planning state from visible artifacts and user-provided context.
2. **Product state lens.** Reconstruct the current phase, value proposition,
   what must become more true, and the active uncertainty.
3. **Move set.** Generate materially different next moves, including
   stop/cut/defer when real.
4. **Comparison.** Compare why each move compounds or fails to compound now.
5. **Recommendation.** Select one next move in plain language.
6. **Compounding rationale.** Explain why it compounds most, why the other
   viable moves compound less right now, and what next decision changes.
7. **Downstream method.** Name a downstream workflow only if it is the lightest
   suitable container for the already-selected move.
8. **Boundaries.** State what not to do yet, not-proven boundaries when
   material, and the next authorized step.

The main planner owns synthesis. Do not average options, classify before
reconstructing the product state, route mechanically from bottleneck examples,
or treat agreement among prior artifacts as proof.

## Anti-Router Invariant

The recommendation must be an action, artifact, or question before it can be a
workflow name.

Required final recommendation test, whether as standalone lines in full mode or
folded into the compact sections:

```text
Why this compounds most:
Why the deferred moves compound less right now:
What next decision this changes:
```

If these lines are weak, the result is routing theater rather than compound
sequencing. Revise or return `STATE_TOO_THIN`.

## Acceptance And Admin Gate Advance Rule

If the apparent next move is "accept X", sign off X, mark X current, reconcile a
non-substantive status gate, or complete another admin gate, first check whether
the user has already completed the gate or is asking what comes after it.

Invariant: treat non-substantive admin gates as done for advisory sequencing
unless visible evidence says they are disputed, the user explicitly asks
whether to accept the gate, or the gate itself changes substantive authority.
Do not make "write an acceptance note" the recommended move by default.

When completion is user-stated or default-assumed but not source-proven, label
the state as `user-stated` or `default-admin-assumed` and `not proven` for
strict claims, then recommend the next compounding move after the gate.

Only recommend acceptance or another admin gate when the gate is a substantive
blocker: the owner is deciding accept/reject, the accepted boundary is
disputed, or the gate changes product scope, proof standard, authority,
rollback/kill criteria, or the safe set of downstream moves.

Examples:

```text
Bad: Accept the proof preflight.
Good: Assuming the proof preflight is accepted as user-stated, the next
compounding move is to prepare the proof packet inputs because acceptance no
longer blocks sequencing; proof readiness now constrains the value proposition.

Bad: Write an owner decision record that accepts the reviewed packet.
Good: Treating the owner decision record as default-admin-assumed for advisory
sequencing, the next compounding move is to define the buyer-proof standard
that the bounded packet can inform.
```

## Admin Signal Minimization

Administrative and lifecycle state is background signal, not a compounding
frontier.

- Do not include branch names, commit SHAs, dirty-state inventories, staging,
  commit, push, merge, rebase, package, install, deploy, publish, cache refresh,
  version bump, or plugin metadata details in ordinary move comparisons.
- Do not recommend stage, commit, push, package, install, deploy, publish,
  cache refresh, or version bump as the selected move unless the user explicitly
  asked for that lifecycle action or visible evidence proves it is the only
  material blocker to product learning, proof, or decision quality.
- Treat "commit this", "accept this", "reject this", "sign this off", and
  similar status instructions as noise for compound sequencing when they merely
  record or close a non-substantive gate.
- Treat accept/reject as substantive only when the decision changes product
  scope, proof standard, authority, rollback/kill criteria, or the safe set of
  downstream moves.
- If the user asks what compounds after commit, accept, reject, or another
  admin action, assume that admin action is done for advisory sequencing unless
  the user explicitly asks whether to do it. Label the assumption and recommend
  the next workflow-content move.
- If lifecycle authority is missing, state `NOT_CLAIMED` only when it prevents
  confusion. Do not expand it into a commit/push/deploy checklist.

## Incremental Sequencing Reasoning

Use this built-in decision discipline during move comparison and recommendation.
Do not depend on a separate `workflow-deep-thinking` invocation.

- Treat the user's proposed move, prior-thread move, and obvious next workflow
  as candidates, not defaults.
- Reconstruct current phase, value proposition, active uncertainty, and what
  must become more true before selecting a move.
- Generate materially different alternatives, including stop/cut/defer when
  real.
- Compare options by compounding leverage, not by workflow label.
- Explain why the selected move compounds most and why the other viable moves
  compound less right now.
- Preserve uncertainty, name what would change the answer, and end with one
  clear next move.

If the host has already activated `workflow-deep-thinking`, use it as additional
reasoning support, but the incremental-planning discipline in this section owns
the output.

## Sequencing Horizon

Default to one immediate recommended next move. Add a conditional next-step
preview only when it prevents local optimum, acceptance-loop, or premature
overbuilding risk, and fold it into `Next authorized step` in compact mode. Do
not exhaustively evaluate two-step trees.

The preview should name the next decision the recommended move is expected to
sharpen:

```text
Conditional next-step preview:
- If the move passes, next compare ___ vs ___.
- If it exposes incoherence, next reconcile ___.
- If it shows market/contact is the blocker, next do ___.
```

Do not select a committed second move when the first move is expected to produce
new evidence. Most second moves are conditional on what the first move reveals.

Run an assumed-state second pass only when the user explicitly asks to assume a
move is done, or when the apparent first move is an acceptance or admin gate
already user-stated, default-assumed, or otherwise non-substantive. Label the
assumed state and recommend the next compounding move after the gate.

## Product State Lens

Do not classify first. Reconstruct the product state first.

Use this lens before comparing moves:

```text
Current phase:
Product value proposition:
What must become more true:
Active uncertainty:
Current compounding frontier:
```

The current compounding frontier is the point where the product value
proposition cannot become more proven, narrower, more falsifiable, or more
decision-ready until a specific uncertainty is resolved.

The frontier must include:

```text
The current compounding frontier is ___ because the value proposition cannot
become more proven until ___.
```

These are reasoning lenses, not output fields. Compact output has only four
state/comparison surfaces: `Current state`, `Frontier`, `Moves compared`, and
`Recommended next move`. Synthesize current phase, value proposition, what must
become more true, and active uncertainty once under `Current state`, then use
`Frontier` for the single compounding constraint. Do not repeat the same
bottleneck under multiple labels.

Do not use phase names, bottleneck examples, or downstream workflow names as
hidden routing keys.

## Common Bottleneck Families

Use these as examples only. They are not canonical phases and must not become a
route table:

- value proposition unclear;
- audience, customer, buyer, decision owner, or trigger unclear;
- proof standard unclear;
- evidence stale, conflicting, or unaccepted;
- proof readiness unclear;
- external customer or market signal missing;
- existing signal uninterpreted or low quality;
- repeatability, retention, or expansion unproven;
- feature shape premature or newly justified;
- implementation premature or newly scopeable;
- stop, cut, or defer is higher-leverage than more process.

## Next-Move Option Set

Compare materially different moves. Use only moves that are plausible from the
visible state:

- climb upstream to a missing product, buyer, or proof-standard decision;
- run product exploration;
- run feature exploration;
- create a proof-prep or proof-preflight artifact;
- reconcile acceptance, verdict, freshness, or status conflicts;
- run artifact or code review;
- do customer discovery or market contact;
- interpret existing market or customer signals;
- perform targeted source research;
- run the proof;
- create an operator aid only after repeated use exposes friction;
- scope implementation after an accepted concrete plan;
- make a substantive accept/reject decision when it changes product, proof, or
  authority state;
- stop, cut, or defer.

Workflow names are execution containers, not recommendations. Use them only
after selecting the move.

## Decision Criteria

Use qualitative comparison by default. Avoid numeric scores unless the user
explicitly requests them or repeated local use proves they reduce inconsistency.

Compare options by:

- learning yield;
- proof proximity;
- decision unlock;
- dependency order;
- commercial relevance;
- speed and reversibility;
- cost of delaying market contact;
- ability to expose hidden weakness;
- bloat risk;
- fake-rigor risk;
- overfitting risk;
- whether the move creates durable decision value or only more process.

Use long-term structural integrity as a constraint and tie-breaker for internal
moves, not as the primary optimization target. Structural work is
decision-relevant only when it names the proof, market-facing decision,
source-boundary, stage-ownership, retrievability, claim-traceability, or future
sequencing failure it prevents.

The strongest next move usually changes a vague continuation decision into a
sharper proof, learning, acceptance, cut, or scoping decision with the least
unnecessary commitment.

Default preference: choose the move that creates new external evidence unless
a named internal incoherence would corrupt that evidence. Internal artifact
work must earn its place by naming the exact market-facing decision it unlocks.

## Smallest Complete Intervention

When selecting the next move, default to the smallest complete intervention: the
narrowest action, artifact, question, or review that would resolve the current
compounding frontier enough to change the next decision.

A move is too small if it is partial, fragile, symptom-only, merely
ceremony-reducing, or likely to require an immediate corrective pass before the
next decision can be made. A move is too large if it adds unrelated cleanup,
broad rewrites, speculative abstractions, extra workflow ceremony, or
nice-to-have improvements that do not directly unlock proof, learning, or
decision quality.

Include adjacent work only when it is directly necessary for the selected move
to be complete. Do not treat minimal diff, shortest answer, or lowest ceremony
as sufficient when it leaves the frontier unresolved.

This doctrine does not weaken claim discipline: unsupported readiness,
validation, acceptance, proof, deployment, resolver, plugin, or source-of-truth
claims remain `not proven`, and real failures must not be hidden behind
fallback wording.

## Anti-Bloat Rules

- Do not build foundations indefinitely. Expand foundation only when a named
  gap blocks proof or decision quality.
- Do not use long-term structural integrity as a generic reason to build more
  foundation. Structural work must name the future sequencing, proof, decision,
  source-boundary, stage-ownership, retrievability, or claim-traceability
  failure it prevents.
- Do not add operator kits before real use exposes repeatable friction.
- Do not run proof before proof standard and preflight are coherent.
- Do not do customer discovery when the immediate blocker is internal artifact
  coherence, but do not use internal artifacts to delay market contact without
  naming the exact market-facing decision the internal work unlocks.
- If an internal next move cannot name the exact market-facing decision it
  unlocks, recommend market contact or stop/defer instead.
- Do not create status artifacts unless they reconcile a blocker that affects
  proof or learning.
- Do not route to feature planning before product proof justifies feature-shape
  exploration.
- Do not route to implementation scoping before an accepted concrete plan
  exists.
- Do not treat "more process" as compounding progress.
- Do not treat lifecycle/admin cleanup as compounding progress unless it is the
  explicit user request or the only material blocker to decision quality.
- Do not default to proof preflight merely because a foundation exists.

## Downstream Workflow Guidance

Name a downstream workflow only after the next move is selected:

- Recommend product exploration in plain language when the selected move is
  customer, buyer, promise, or product-proof exploration; defer any named lane
  to the active project.
- Recommend feature-shape exploration in plain language only when product proof
  justifies it; defer any named lane to the active project.
- Use `workflow-implementation-scoping` only when an accepted concrete plan
  should become a non-executing implementation route.
- Use review skills only when artifact or implementation evidence must be
  inspected and formal review authority remains separately bound.
- Use `workflow-prompt-orchestrator` only when the selected move is to prepare a
  prompt, wrapper, rerun, review prompt, patch prompt, or handoff.
- Use stop/cut/defer when no move improves learning, proof, or decision quality
  enough to justify more process.

Do not execute the downstream workflow in the same response unless the user
separately invokes or authorizes that workflow and the downstream skill's own
boundary permits it.

## Prompt Courier

When the selected next move is to prepare a prompt, wrapper, handoff, review
prompt, rerun prompt, or patch prompt, the result may append a compact
`prompt_courier` YAML block after `Next authorized step` if that would reduce
translation loss into `workflow-prompt-orchestrator` or a next thread.

The courier serializes the selected sequencing decision only. It is upstream
advisory context for the next input, not prompt text, route execution, authority
binding, validation evidence, or a durable artifact. Do not emit it for
non-prompt moves, and do not emit it merely to make ordinary sequencing
machine-readable.

Use this shape, omitting unknown optional fields rather than inventing them:

```yaml
prompt_courier:
  source: incremental-planning
  anchored_goal:
    value: "<anchor_goal, user-stated goal, or inferred goal>"
    source: "goal_handoff | user-stated | repo-visible-inference | assumption | source-gap"
  selected_next_move: "<plain-language prompt-worthy move>"
  prompt_kind: "handoff | review | rerun | patch | thin-wrapper | full-prompt | unbound"
  prompt_should_ask_for: "<specific output the downstream prompt should elicit>"
  why_this_now: "<one-sentence compounding rationale>"
  do_not_include:
    - "<premature or unauthorized scope>"
  unbound_authority:
    - "<paths/review lane/validation/edit permission/etc., if missing>"
```

The courier must not include the full downstream prompt, saved artifact claims,
path or hash claims, validation results, review verdicts, edit permission, patch
authority, lifecycle permission, package/install/deploy/cache claims, or any
project-owned binding not already supplied by visible authority.
`workflow-prompt-orchestrator` must still resolve output mode, template kind,
source authority, destination, validation, edit permission, and leakage checks
from its own contract.

## Output Contract

By default, an incremental-planning result is `chat-output`: it is an advisory
sequencing decision in the current response, not a durable status artifact.
YAML rendered in chat remains `chat-output`. If the user asks to save the
result as a durable artifact, bind `filesystem-output` before generation with
artifact role, exact destination or explicitly authorized derivation, write
authority, freshness marker, retention or promotion expectation, and write
failure behavior. Missing mode, role, destination, or write authority blocks
with `BLOCKED_OUTPUT_MODE_MISSING`, `BLOCKED_UNBOUND_ARTIFACT_ROLE`,
`BLOCKED_OUTPUT_DESTINATION_UNBOUND`, or `BLOCKED_BY_AUTHORIZATION`.

Use compact human-readable output by default with bold Markdown section
headers. Omit empty sections. The default contract is:

```text
**Current state:** one sentence.
**Frontier:** what cannot become more proven until what is resolved.
**Moves compared:** 2-4 materially different moves, one line each.
**Recommended next move:** action, artifact, or question - not workflow name.
**Why this compounds most:** one paragraph.
**Why not the others:** one paragraph.
**What not to do yet:** one sentence.
**Next authorized step:** exact next question/artifact/action.
```

When the selected next move is prompt preparation and a courier would preserve
the sequencing decision for the next input, append one fenced `yaml` block with
`prompt_courier` after `Next authorized step`.

`Current state` should combine phase, value proposition, active uncertainty,
accepted/admin-gate assumption when material, and material source labels in one
sentence. `Frontier` should be the single compounding constraint, not a second
copy of the uncertainty. `Moves compared` must include at least two plausible
moves unless the result is `STATE_TOO_THIN`, and should include stop/defer or
market contact when plausible. If a downstream workflow is useful, name it only
after the plain-language move inside `Recommended next move` or `Next
authorized step`; do not add a separate downstream-workflow section by default.

Use the full audit contract only when:

- the state is disputed;
- source authority matters;
- an admin/acceptance gate changes substantive authority;
- the user explicitly asks for a full planning audit.

```text
**Current phase:**
**Product value proposition:**
**What must become more true:**
**Active uncertainty:**
**Accepted / admin-gate state used:**
**Current compounding frontier:**
**Bottleneck to compounding:**
**Moves compared:**
**Why this compounds most:**
**Why the deferred moves compound less right now:**
**What this changes in the next decision:**
**Conditional next-step preview:**
**Smallest complete next artifact or question:**
**What not to do yet:**
**Stop / kill criteria:**
**Not-proven boundaries:**
**Blockers / next authorized step:**
**Evidence used:**
**Downstream workflow, if any:**
**Recommended next move:**
```

The full contract does not make every field mandatory when it would create
repetition; preserve the comparative burden and strict blockers, then compress
or omit fields that add no decision value. Use YAML only when explicitly
requested, when appending a prompt courier for a prompt-preparation next move,
when writing a bound `filesystem-output` artifact, or when material blockers
require an audit frame.

### Compact Output Examples

```md
**Current state:** The foundation is coherent enough to compare moves, but the
buyer trigger and proof standard are still assumption-labeled.
**Frontier:** The value proposition cannot become more proven until the proof is
aimed at a specific buyer decision and pass/fail threshold.
**Moves compared:** Buyer/proof clarification; immediate market contact; proof
preflight; more foundation.
**Recommended next move:** Write the smallest complete buyer-trigger/proof-standard note.
**Why this compounds most:** It turns both proof preflight and market contact
from generic activity into targeted learning.
**Why not the others:** Market contact has delay cost, but doing it now risks
asking the wrong person the wrong question; proof preflight would encode a
blurry standard; more foundation adds process.
**What not to do yet:** Do not run proof or scope implementation.
**Next authorized step:** Answer which buyer trigger is being tested and what
evidence would make you stop, narrow, or proceed.
```

```md
**Current state:** A bounded proof standard and coherent inputs are visible.
**Frontier:** The value proposition cannot become more proven until the proof
produces pass/fail evidence.
**Moves compared:** Run the proof; add more preflight; do market contact; build
an operator aid.
**Recommended next move:** Run the proof against the accepted threshold.
**Why this compounds most:** It creates new evidence instead of improving the
container around evidence that is ready to be collected.
**Why not the others:** More preflight and operator aids are process; market
contact may be useful after the proof shows which claim needs testing.
**What not to do yet:** Do not broaden features or polish internal tooling.
**Next authorized step:** Execute the proof and record pass/fail against the
threshold.
```

```md
**Current state:** The request names a desired next move but not the product,
goal, or current decision.
**Frontier:** STATE_TOO_THIN: fewer than two plausible moves can be compared
without inventing facts.
**Moves compared:** Not enough evidence to compare.
**Recommended next move:** Provide the current product/goal and one candidate
move you are considering.
**Why this compounds most:** That is the smallest complete missing source needed to make
a real sequencing call.
**Why not the others:** Any workflow or market-contact recommendation would be
invented.
**What not to do yet:** Do not route to a planning workflow yet.
**Next authorized step:** Share the current state and two candidate moves, or
one candidate move plus what you are tempted to defer.
```

## Quality Bar

A valid result must:

- select a compounding move before naming a workflow;
- advance past user-stated acceptance or admin gates instead of recommending
  the same gate again;
- default-assume non-substantive admin gates are complete for advisory
  sequencing unless disputed, explicitly asked, or substantively authority
  changing;
- apply embedded incremental sequencing reasoning when comparing plausible next
  moves;
- consume a supplied `goal_handoff` as sequencing context without replacing it;
- compare at least two plausible alternatives when the state makes them real;
- explain why the selected move compounds most;
- explain why major deferred moves compound less right now;
- state what next decision changes;
- select the smallest complete intervention: the narrowest move that would
  resolve the current compounding frontier enough to change the next decision,
  without partial fixes, fake completeness, unrelated cleanup, or hidden failure
  states;
- include a conditional next-step preview when it prevents local optimum,
  acceptance-loop, or premature-overbuilding risk, folded into compact output
  instead of a separate section unless using the full audit contract;
- avoid committed second moves when the recommended move should produce new
  evidence;
- name what not to do yet;
- filter lifecycle/admin noise out of ordinary move comparisons, including
  commit, stage, push, install, deploy, publish, cache refresh, version bump,
  and non-substantive accept/reject instructions;
- preserve market-contact delay cost when recommending internal work and name
  the exact market-facing decision the internal work unlocks;
- recommend market contact or stop/defer when an internal move cannot name the
  exact market-facing decision it unlocks;
- use `STATE_TOO_THIN` only when fewer than two plausible moves can be compared
  without inventing facts;
- return an assumption-labeled recommendation when visible evidence supports a
  useful comparison despite incomplete strict proof;
- mark unsupported readiness, validation, acceptance, proof, deployment,
  resolver, plugin, or source-of-truth claims as `not proven`;
- append `prompt_courier` only for prompt-preparation moves, keep it advisory
  context rather than full prompt text or authority, and include the anchored
  goal/source, selected next move, prompt kind when known, requested downstream
  output, relevant exclusions, and missing bindings when material;
- avoid route tables, generic workflow selection, and hidden readiness claims;
- leave one precise next authorized step.

This skill fails when the recommendation starts with a workflow name, treats a
missing upstream artifact as the only useful move, defaults to proof preflight
after any foundation, creates process artifacts that do not unlock proof, or
delays market contact without decision-value reasoning.
