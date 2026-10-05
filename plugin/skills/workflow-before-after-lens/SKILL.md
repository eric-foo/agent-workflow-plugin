---
name: workflow-before-after-lens
description: "Workflow-kernel skill for explicit before/after comparison: compare an original request, prior state, plan, scope, handoff, review, or artifact against a proposed or actual output so the user can evaluate drift, hidden assumptions, tradeoffs, and acceptance risk. Use only when explicitly invoking `workflow-before-after-lens`, asking for before/after, asking what changed between a plan/scope/handoff/review/artifact and the original intent, or asking to compare a plan or scope against the original intent. Do not use for generic planning, scoping, review, validation, implementation, artifact mutation, lifecycle claims, installation, deployment, staging, commits, or pushes."
---

# Workflow Before/After Lens

## Purpose

Provide a compact alignment lens after planning, scoping, handoff, review, or
artifact work when the user wants to judge what changed between an original or
prior frame and a proposed or actual output.

Core question:

```text
What was the starting frame, what is the proposed or actual ending frame, and
what changed enough that the user should inspect it before accepting?
```

This skill is a non-executing evaluation lens. It does not produce the plan,
scope, review, implementation route, patch queue, validation result, or artifact
being compared.

This skill evaluates alignment drift only: whether the proposed or actual
output still matches the original or prior frame. It does not judge correctness,
implementation quality, validation status, or readiness unless the comparison
target explicitly claims those things.

## Skill Boundary

`workflow-before-after-lens` supplies reusable workflow-kernel mechanics. It is
not a router, planner, scoper, reviewer, validator, implementation lane,
artifact authoring lane, acceptance authority, or lifecycle lane.

Installed, user-level, plugin, project-local, or global skill copies are not
source authority for this skill.

## Trigger Gate

Use this skill only when explicitly invoked by name or when the user directly
asks for:

- before/after;
- a before and after lens;
- what changed between a plan, scope, handoff, review, artifact, or proposal
  and the original intent;
- a delta from the original intent;
- a comparison between an original request, prior state, plan, scope, handoff,
  review, or artifact and a proposed or actual output.

Both sides of the comparison must be explicit or safely inferable:

1. original or prior frame;
2. proposed or actual output.

If either side would require guessing, return a blocker.

Do not infer activation from generic requests to plan, scope, review, implement,
validate, summarize, or decide a next move.

## Input Discipline

Use the smallest complete named or active comparison anchors.

Original side:

- When a `goal_handoff` is supplied, use `anchor_goal` and `success_signal` as
  comparison anchors for intent and output-fit unless the user names a more
  specific original side.
- Prefer the latest explicit user request, named prior artifact, or
  user-accepted frame that the comparison target depends on.
- Do not use older conversation context unless it contains a named constraint,
  accepted frame, or explicit comparison anchor.

Target side:

- Prefer the immediately preceding plan/output, unless the user names another
  target.

Inference discipline:

- If either side is inferred, name the exact anchor used.
- If multiple plausible anchors exist and the comparison would materially
  change, return a blocker instead of blending them.

Use narrow repo-visible source only when the user points to it or source
authority affects the comparison.

Return a blocker instead of inventing either side when the comparison would
materially change based on missing input.

## After Label Discipline

Use `Proposed After` when the compared output is a plan, scope, handoff,
proposal, intended future state, or unexecuted recommendation.

Use `After` only when the compared output is an actual completed state,
accepted artifact, implemented change, or user-confirmed outcome.

If the status is unclear, use `Proposed After` and mark the status uncertainty
inside the comparison.

## Drift Check

Before returning the lens, check for material drift:

- goal drift: the proposed or actual result optimizes for a different outcome;
- scope drift: boundaries expanded, narrowed, or changed without being named;
- assumption drift: new assumptions entered the work silently;
- constraint drift: stated constraints became weaker, disappeared, or changed;
- artifact drift: the output type or acceptance surface changed;
- authority drift: a proposal is presented as accepted state, proof, validation,
  readiness, or implementation success without authority.

Do not call every difference a problem. Distinguish useful clarification from
risky drift.

Classify changes by action level:

- useful clarification: harmless or helpful precision that preserves intent;
- material drift: a meaningful change the user should notice before accepting;
- acceptance risk: a material change that may need approval, rejection, or
  revision before the output is treated as accepted;
- blocker: missing or ambiguous comparison input that prevents a fair lens.

For long plans, handoffs, reviews, or artifacts, include tiny evidence anchors
for material changes. Use short anchor phrases, field names, headings, or
paraphrases; do not quote more than needed to identify the source of the
comparison.

If multiple proposed or actual outputs are plausible targets, require the user
to name the target instead of blending them.

## Output Contract

For ordinary chat, use this mandatory field order:

```text
Material Changes:
Acceptance Risks:
Not Drift:
Blocker:
Before / After:
```

Use `After` instead of `Proposed After` only when the after label discipline
permits it.

Human readability is the first priority. Write ordinary chat output for a human
trying to decide whether to accept the compared result, not for a parser. Use
plain sentences, concrete nouns, and short bullets. Avoid schema-like fragments
unless the user asks for structured output.

Field meanings:

- `Material Changes`: up to five changes that matter, labeled as added,
  removed, reframed, assumed, deferred, or unchanged when useful. For each
  material change, include a compact before/after snapshot from the actual
  compared anchors when possible. The snapshot may be a value, phrase, field,
  heading, output shape, constraint, commitment, or short paraphrase. Prefer
  `Before:` and `Proposed After:` / `After:` lines under the change. Do not
  invent illustrative examples.
- `Acceptance Risks`: what the user should inspect, approve, reject, or change
  before treating the output as accepted.
- `Not Drift`: useful clarifications or harmless changes that should not be
  overtreated as problems.
- `Blocker`: include only when one side is missing or materially ambiguous.
- `Before / After`: the bottom section. Restate the compared frames in the
  clearest human-readable language:
  - `Before`: starting frame, key constraints, and named or inferred original
    anchor.
  - `Proposed After` / `After`: ending frame, new or changed assumptions, and
    named or inferred target anchor.

Keep the result compact. Prefer grouped bullets over narration. Include at most
five material changes unless the user asks for an exhaustive comparison. Omit
empty fields, but do not omit a material gap.

The bottom `Before / After` section should be the easiest part to read. Prefer
one or two complete sentences per side over dense lists. It should let the user
understand the starting point and ending point without rereading the whole
comparison.

## Structured Rendering

Use structured rendering when the user asks for YAML, when a handoff requires
machine-readable fields, or when blockers depend on exact comparison inputs:

```yaml
before_after_lens:
  comparison_status: proposed | actual | unclear
  before:
    goal:
    starting_state:
    constraints: []
    user_intent:
    open_questions: []
  after:
    label: Proposed After | After
    target_or_completed_state:
    new_commitments: []
    boundaries: []
    expected_result:
    validation_or_success_signal:
  material_changes:
    - change:
      action_level: useful_clarification | material_drift | acceptance_risk
      change_type: added | removed | reframed | assumed | deferred | unchanged
      evidence_anchor:
  acceptance_risks:
    inspect_before_accepting: []
  not_drift:
    useful_clarifications: []
blocked_states:
  - state:
    blocked_action:
    blocking_source_or_rule:
    smallest_complete_next_input:
```

In structured rendering, keep empty lists or `none` values only when their
absence would hide a material gap.

## Blocked States

Use `BLOCKED_BEFORE_INPUT_MISSING` when the original request, prior state, or
starting frame is not stated or safely inferable.

Use `BLOCKED_AFTER_INPUT_MISSING` when no proposed or actual output is stated
or safely inferable.

Use `BLOCKED_COMPARISON_TARGET_AMBIGUOUS` when multiple plausible before or
after targets exist and choosing one would materially change the comparison.

Blocked missing-input response:

```text
BLOCKED_BEFORE_INPUT_MISSING

I need the original request, prior state, or starting frame to compare against.
```

Ask only the smallest complete direct question needed to unblock the comparison.

## Adjacent Ownership

This skill may name adjacent ownership only as a boundary:

- goal choice belongs to goal framing or the user;
- route mapping belongs to cartography;
- next-move selection belongs to incremental planning;
- product, feature, implementation, prompt, and review artifacts belong to
  their own lanes;
- validation, acceptance, deployment, and lifecycle claims require their own
  source authority.

Do not execute adjacent workflow work.

## Constraints

- Do not edit files while using this skill unless the user separately
  authorizes source implementation of the skill itself.
- Do not install, deploy, shadow, promote, package, rename, stage, commit, or
  push skills.
- Do not create runtime code, build systems, plugin metadata, saved artifacts,
  validation records, review verdicts, implementation routes, patch queues,
  acceptance records, or resolver claims.
- Do not import project-specific paths, lifecycle labels, validation commands,
  review labels, domain facts, artifact destinations, protected paths, or local
  downstream policy.
- Do not make this skill a mandatory front door for planning or scoping.

## Quality Bar

A valid before/after lens result must:

- trigger only on explicit before/after comparison intent;
- require both an original or prior frame and a proposed or actual output;
- compare two sides rather than creating a new plan or scope;
- label inferred before or after inputs;
- use `Proposed After` for unexecuted plans, scopes, handoffs, proposals, and
  intended future states;
- use `After` only for actual completed or user-confirmed state;
- evaluate alignment drift only, not correctness or implementation quality;
- use supplied `goal_handoff` fields as intent and output-fit anchors without
  treating them as acceptance or validation authority;
- surface material drift without treating every difference as a defect;
- separate useful clarifications, material drift, acceptance risks, and
  blockers;
- put the plain-language `Before / After` section at the bottom for ordinary
  chat output;
- include compact before/after snapshots from the actual compared anchors for
  material changes when possible;
- make the bottom comparison extremely human readable, with complete sentences
  preferred over dense fields;
- include tiny evidence anchors for material changes when comparing long plans,
  handoffs, reviews, or artifacts;
- cap ordinary output at five material changes unless the user asks for an
  exhaustive comparison;
- tell the user what to approve, reject, or change before acceptance;
- avoid validation, implementation, readiness, lifecycle, deployment, install,
  packaging, staging, commit, or push claims.
