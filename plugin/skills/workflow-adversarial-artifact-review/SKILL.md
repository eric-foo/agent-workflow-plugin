---
name: workflow-adversarial-artifact-review
description: Workflow-kernel skill for adversarial review of non-code artifacts. Use when explicitly invoking `workflow-adversarial-artifact-review`, asking for adversarial artifact review, source-backed artifact review, or two-phase artifact review. Run one operator-facing adversarial artifact review flow; advisory findings may proceed from repo-visible evidence, while formal artifact-role verdicts, patch queues, and validation claims require strict overlay bindings. Do not use for implementation/code review, installed-copy review, generic review, postmortem review, prompt wrapping, patch execution, or project-local review lanes without declared overlay precedence.
---

# Workflow Adversarial Artifact Review

## Purpose

Run adversarial review of a non-code artifact without editing the artifact. The
only operator-facing flow is adversarial artifact review. Within that flow,
advisory findings may proceed from repo-visible evidence, while formal artifact
review claims remain strict and require overlay-bound artifact authority.
Review target and purpose are commission-bound; do not silently retarget the
artifact, widen the role, or convert advisory critique into a formal result
because adjacent evidence is visible.

This skill supplies reusable artifact-review mechanics only. The active
project overlay owns artifact roles, source hierarchy, concrete artifact
categories, review lane names, result vocabulary, report destinations, template
registry, validation gates, review output mode, required output paths, write
permissions, model routing, and patch execution authority.

## Deep Thinking

All adversarial artifact reviews use deep-thinking discipline: identify hidden
assumptions, contradictions, bypass paths, authority leaks, source gaps,
failure modes, and review-routing errors before writing findings.

This discipline is part of this skill; do not load `workflow-deep-thinking`
separately for the review.

Casual readback and targeted scoped diff review are not owned by this skill.
When an authorized adjacent bridge or implementation method requires targeted
scoped diff review to decide escalation or final closeout, that review may use
deep-thinking discipline through that method without becoming adversarial
artifact review.

## Source Preflight

Before reviewing, run the smallest complete source preflight that can identify
claim-level authority handling and prove any strict requested outcome is
authorized:

1. Verify active workspace, branch or revision, repository rules, workspace
   overview, target artifact or artifact role when provided, edit permission,
   and expected dirty-state allowance when strict claims depend on it.
2. Load the active overlay or equivalent authority for artifact roles, review
   lane binding, validation gates, source hierarchy, and template registry only
   when strict formal claims or project-specific templates are required.
3. Build a claim-appropriate source-read ledger. For strict-required claims,
   record each authoritative source's path or role, why it was read, the
   decision it supports, its authority role, revision or freshness marker, and
   whether it is clean, dirty, untracked, stale, or otherwise unanchored. For
   advisory findings, record reviewed source, cited evidence, known gaps, and
   unavailable freshness or dirty-state limits when they affect confidence.
4. Name every dirty or unanchored source relied on when it affects strict
   authority or advisory confidence. For strict-required claims, a dirty source
   is any source used as authority whose repository status is modified,
   untracked, deleted, renamed, or not anchored to the expected revision.
5. If a strict-required claim depends on dirty source that is not allowed, if
   the expected revision is mismatched, or if the dirty source could change
   review authority, return a blocked result before reviewing.

Before full review for any strict, formal, durable, verdict-bearing, or
patch-queue-bearing claim, preflight review output binding. Bind either
`filesystem-output` with a valid `required_output_path`, or explicit
`chat-output` for transcript-only review. Deterministic path derivation is
valid only when the prompt, wrapper, template, or overlay states the convention.
Missing mode returns `BLOCKED_OUTPUT_MODE_MISSING`; missing filesystem
destination returns `BLOCKED_OUTPUT_DESTINATION_UNBOUND`. Do not run full review
and then fall back to a large chat transcript.

Use only minimal preflight when authorization is already blocked. Do not run
full review, broad source mapping, validation dry runs, collision checks, or
patch-queue planning after an authorization block is clear unless the user
explicitly asks for source-backed planning inside the allowed scope.

## Direct-Entry Source And Context Intake

When this skill is invoked directly, do not require a prior repo-context
packet. Reorient from the current repository before review: read local
instructions, the target artifact or artifact role, cited source context,
material workspace facts, and local overlay or equivalent authority needed for
any strict artifact-review claim.

If a repo-context packet, task-local context pack, repo map, summary, or prior
thread note is provided, use it only as orientation. It does not replace source
preflight, trigger checks, lane-collision checks, artifact-role checks,
two-phase critique, formal-review blockers, or final result authority. Reread
decisive current source, target artifact, validation evidence, or bound
authority before strict or actionable claims.

## Adversarial Review Flow

Use one operator-facing flow: adversarial artifact review. Do not ask the user
to choose between zero-config advisory review and strict formal review as
separate modes.

Apply the zero-config source-loading contract at the claim level:
advisory findings may proceed from visible artifact evidence and must use
repo-visible evidence labels, a source-read ledger, source gaps, and `not
proven` boundaries when material. Strict-shaped claims such as formal
artifact-role verdicts, severity taxonomies, overlay-bound blocked/ready status,
source-of-truth status, validation pass/fail, approval, readiness, mandatory
remediation, patch queue authority, executor-ready handoff, deployment,
resolver behavior, or plugin readiness require bound artifact, lane, decision
criteria, gate, patch, readiness, or lifecycle authority. Reading more source
can improve critique evidence, but it cannot create artifact-role or
review-lane authority.

Use strict-required handling for formal artifact-review verdicts, artifact-role
claims, overlay-bound result vocabulary, severity taxonomies, overlay-bound blocked/ready
status, protected edits, patch queues, mandatory remediation, mandatory
validation, pass/fail claims, deployment or readiness claims, and
executor-ready remediation handoff. Missing artifact role, review lane, source
authority, decision criteria, validation gate, collision precedence, template
registry, dirty-state allowance, output mode, or output destination blocks that
strict claim.

Do not convert advisory critique into strict claims. Missing strict authority
blocks only the strict claim; it does not suppress advisory critique when
visible evidence supports it. When no source hierarchy is bound, cite only the
reviewed artifact and supplied or repo-visible sources; do not rank them as
canonical unless that authority is supplied.

## Trigger Gate

Use this skill only when the user explicitly asks for this lane, such as:

- `workflow-adversarial-artifact-review`;
- `adversarial artifact review`;
- `source-backed artifact review`;
- `two-phase artifact review`;
- an equivalent request naming an artifact-review lane or artifact role.

Do not infer this lane from generic `review`, `look this over`, `audit this`,
or `check this` language unless the overlay declares that wording as an
artifact-review trigger and collision precedence is bound.

If the request identifies a repo-visible non-code artifact but does not bind an
artifact role or review lane, proceed with advisory critique inside the
adversarial artifact review flow. Do not convert advisory critique into strict
claims.

## Lane Collision Resolver

Resolve adjacent review scope before reading the artifact deeply.

- If the request reviews implementation behavior, code, tests, runtime changes,
  or a change packet, require the implementation-review lane instead.
- If the request reviews installed copies, source resolution, resolver
  visibility, deployment drift, or rollback evidence, require the installed-copy
  boundary lane instead.
- If the request reviews completed work integrity or process learning after a
  finished change, require postmortem review instead.
- If the request asks to create, format, wrap, or route a prompt, require prompt
  orchestration instead.
- If the request asks to edit the artifact, apply patches, or execute
  remediation, require a separate execution or patch authority. Artifact review
  may only report findings; patch queues require strict overlay authorization
  and are never patch execution.
- For mixed artifacts, review only the non-code artifact claims, instructions,
  templates, or workflow text in this lane. Route implementation correctness to
  the implementation-review lane.

Return `BLOCKED_LANE_COLLISION` when more than one lane could answer and the
overlay has not declared precedence. For strict-required formal review claims,
return `BLOCKED_UNBOUND_REVIEW_LANE` when no artifact-review lane is bound.

## Required Bindings

Before making strict formal review claims, bind:

- artifact role, source authority, read permission, freshness marker, and paired
  artifact rules when applicable;
- artifact-review lane purpose, scope, excluded scope, reviewer write
  permission, and no-write or destination policy;
- review output mode: `filesystem-output` with bound `required_output_path` for
  durable review artifacts, or explicit `chat-output` for transcript-only
  review;
- local result vocabulary and escalation routing;
- collision notes for adjacent review tools or skills;
- validation gates with pass, fail, blocked, and not-run semantics;
- source hierarchy and conflict rules;
- template registry only when project-specific template routing affects the
  artifact or review request;
- patch-queue routing only when executor-ready entries are requested.
- Red-green proof expectations for testable remediation claims, including when
  same-check proof is mandatory, accepted as weaker evidence, or not
  applicable to non-executable artifact findings.

Missing authority must not become an assumption. Use the most precise blocked
state available.

Advisory critique may proceed without these bindings when repo-visible artifact
text and any cited source are available, but the missing bindings must be named
as strict-only blockers or `not proven` boundaries.

## Kernel Boundary States

These reusable advisory or blocking labels are not project-local result
vocabulary unless an active overlay adopts them.

- `BLOCKED_BY_AUTHORIZATION`: the requested action exceeds repository, prompt,
  overlay, role, or approval authority.
- `BLOCKED_UNBOUND_REVIEW_LANE`: no artifact-review lane binding exists.
- `BLOCKED_LANE_COLLISION`: adjacent review precedence is undeclared.
- `BLOCKED_UNBOUND_ARTIFACT_ROLE`: artifact role, source authority, permission,
  freshness, or paired-artifact rule is missing.
- `BLOCKED_ROLE_PERMISSION`: the requested action exceeds role permission.
- `BLOCKED_ROLE_DRIFT`: bound role evidence is stale or conflicting.
- `BLOCKED_SOURCE_REVISION_MISMATCH`: expected revision and available source do
  not match.
- `BLOCKED_DIRTY_SOURCE_UNDECLARED`: dirty source is relied on without allowed
  dirty-state scope.
- `BLOCKED_UNBOUND_VALIDATION_GATE`: mandatory validation expectations are not
  declared.
- `BLOCKED_OUTPUT_MODE_MISSING`: durable or formal review was requested without
  explicit `filesystem-output` or `chat-output`.
- `BLOCKED_OUTPUT_DESTINATION_UNBOUND`: `filesystem-output` lacks a bound
  `required_output_path` or authorized derivation convention.
- `BLOCKED_NOT_RUN`: mandatory validation was skipped without accepted
  deferral.
- `FAILED_VALIDATION`: declared mandatory validation failed.
- `FAILED_REVIEW_WRITE_BOUNDARY`: review attempts source edits.
- `FAILED_REVIEW_OUTPUT_WRITE`: durable review output could not be written.
- `BLOCKED_TEMPLATE_REGISTRY_UNBOUND`: project-specific template routing is
  required but unbound.
- `BLOCKED_AMBIGUOUS_TARGET`: patch-queue target is unstable.
- `BLOCKED_MISSING_VERIFICATION`: patch-queue verification evidence is missing.
- `FAILED_ARTIFACT_REVIEW_LEAKAGE`: reusable review imports project-owned
  paths, categories, result vocabulary, template policy, model routing,
  validation commands, or output destinations.

## Review Workflow

1. **Bind source and authority.** State repository rules, claim-level authority
   handling, overlay bindings when present, target artifact or artifact role,
   review lane, source hierarchy when bound, validation gates when bound, output
   mode and destination when durable or formal review is requested, and
   dirty-source ledger.
2. **Run trigger and collision gates.** Confirm artifact-review scope and block
   ambiguous lane routing.
3. **Run role and validation preflight.** For strict-required claims, confirm
   role permission, freshness, paired-artifact rules, and validation semantics.
   For advisory findings, name missing bindings as strict-only blockers or `not
   proven` boundaries.
4. **Run review-output preflight.** For strict, formal, durable, or
   patch-queue-bearing review, confirm `filesystem-output` plus
   `required_output_path`, or explicit `chat-output`. Block before full review
   when the mode or destination is missing.
5. **Read the artifact against source authority.** Keep reads narrow until a
   missing source could change a finding. For advisory findings, use only
   repo-visible evidence and user-stated context.
6. **Phase 1: correctness.** Check source support, internal consistency,
   downstream executability, boundary control, role-permission consistency, and
   required role outcomes. Do not remove a necessary constraint because it is
   expensive.
7. **Phase 2: friction.** After correctness findings are visible, check
   avoidable process bloat, redundant instructions, unclear routing, unnecessary
   manual work, and validation burden not tied to correctness. Do not keep
   process weight merely because the artifact is technically correct.
8. **Write findings.** Report findings with stable anchors, source evidence,
   impact, blocked state when applicable, `minimum_closure_condition`, and
   `next_authorized_action`.
9. **Prepare optional patch queue.** Only when strict-required patch routing is
   overlay-authorized, translate accepted findings into executor-ready
   `patch_queue_entry` values. Patch queues require strict overlay
   authorization and are never patch execution.
10. **Return review result.** Use overlay-owned result vocabulary only for
   strict-required claims when bound; otherwise return advisory critique
   findings, source gaps, strict-only blockers, `not proven` boundaries, and
   next authorized step without inventing local labels.
11. **Close with review-use boundary.** End the review with a short note that
    findings are decision input for the authorized decision-maker or user, not
    mandatory instructions. Only a separately authorized patch, acceptance,
    validation, lifecycle, or implementation lane can make remediation
    mandatory or executor-ready.
12. **Append delegated return courier when commissioned.** When this review was
    commissioned by `workflow-delegated-review-patch`, append the delegated
    review return courier block so the home model can adjudicate the result in
    a later turn.

## Finding Schema

Each finding must include:

- finding id, such as `AR-01`;
- phase: `correctness` or `friction`;
- commissioned review target and purpose;
- artifact role or reviewed target;
- stable location anchor, structural anchor, or search key;
- source authority used for judgment;
- artifact evidence;
- the strongest reading of the artifact against the finding, and why that
  defense fails; when the defense holds, downgrade or drop the finding rather
  than reporting it;
- requirement or boundary violated, strained, or unsupported;
- impact on correctness, executability, boundary control, validation
  confidence, or operator friction;
- blocked state when the issue is missing authority rather than an artifact
  defect;
- `minimum_closure_condition`: what must become true before this finding can be
  treated as closed;
- `next_authorized_action`: what this review lane is currently authorized to
  do next;
- whether a `patch_queue_entry` is overlay-authorized;
- verification evidence or gate needed for a future executor, including
  red-green proof status when the finding leads to a testable remediation
  claim;
- strict claims that remain `not proven`.

For advisory critique, use repo-visible evidence labels, state source gaps, and
mark unsupported artifact-role, freshness, validation, or verdict claims as
`not proven`. Findings may identify risks, contradictions, and unsupported
claims, but they do not produce artifact-role pass/fail status.

Merge findings that share the same root cause, boundary failure, and
remediation path. Split them only when evidence, impact, or authority differs.
Report friction only when it increases operator error, review cost,
maintenance drift, false authority, or avoidable validation burden.

Do not invent a severity scale, verdict terms, owner names, report envelope, or
destination path. Use them only when the overlay binds them.

## Optional Patch-Queue Adapter

Patch-queue output is optional and read-only. Use it only when strict-required
patch routing is bound and the overlay authorizes executor routing for
artifact-review findings.

Each `patch_queue_entry` must follow the shared fields:

- finding id;
- target file or artifact role;
- stable location key, search string, or structural anchor;
- exact requested change;
- minimum closure condition the patch must satisfy;
- next authorized action for the receiving executor;
- affected expected artifact or role;
- expected behavior impact when applicable;
- verification gate or evidence, with red-green proof status when the
  remediation claim is testable;
- executor result field.

For testable remediation claims, the preferred verification is same-check
red-green proof: the same named test, fixture, check, or validation gate fails
against the pre-fix baseline for the reviewed failure mode and passes after the
fix. When a new test or fixture is part of the remediation, the proof should
show the test-only or check-only state failing against the unfixed behavior
before the implementation change makes it pass.

If same-check red-green proof is unavailable, inapplicable, or too expensive
for the bound lane, the queue entry must say so and label the replacement
evidence as weaker. For source-support, role-authority, boundary, and friction
findings that are not executable checks, mark red-green proof as
`not_applicable` rather than manufacturing a test-shaped gate.

Return `BLOCKED_AMBIGUOUS_TARGET` when the target is not stable. Return
`BLOCKED_MISSING_VERIFICATION` when the executor cannot prove the outcome. A
patch queue is not patch execution and is not permission to edit.

Advisory critique must not emit patch queues. It may mention likely remediation
direction only as advisory prose, not executor-ready handoff.

## Delegated Review Return Courier

When artifact review is commissioned by `workflow-delegated-review-patch`, append
a clearly labeled courier block for the home model. This block is not the
reviewer's verdict becoming accepted truth; it is a transport packet for later
adjudication.

Use this shape:

```text
DELEGATED_ARTIFACT_REVIEW_RETURN_FOR_HOME_MODEL

Here is the delegated artifact review result. Adjudicate it under the
delegated-review-patch return contract.

Include:
- original commission or review target
- reviewed artifact and bounded patch scope
- findings and source evidence
- proposed artifact patch or exact suggested edits, if authorized
- citations
- reviewer verdict
- residual risk
- blockers, off-scope flags, and not-proven boundaries
```

## Output Contract

For strict, formal, durable, or patch-queue-bearing review, follow
`review-lanes/shared-concepts/review-output-binding.md`:

- `filesystem-output` means write the full durable report to the bound
  `required_output_path`; after a successful write, chat contains only a human
  summary and compact courier YAML with path, commission, target, authority,
  decision criteria, evidence summary, reviewer verdict, finding IDs,
  minimum closure conditions, next authorized action, and non-claims.
- `chat-output` means transcript-only review and must be explicitly selected by
  the user, prompt, wrapper, template, or overlay.
- Missing output mode or filesystem path blocks before full review.
- If the durable write fails, return `FAILED_REVIEW_OUTPUT_WRITE` with recovery
  detail and do not claim chat or courier YAML is equivalent to the missing
  artifact.

Return:

- loaded source and overlay bindings;
- claim-level authority handling: advisory findings and any strict-required
  claims or blockers;
- source-read ledger and dirty-source names;
- trigger-gate result;
- lane-collision result;
- artifact-role preflight result;
- validation-gate status;
- review output mode, report path or path blocker, and write status when
  formal or durable review is requested;
- review scope and excluded scope;
- Phase 1 correctness findings;
- Phase 2 friction findings;
- minimum closure conditions and next authorized actions for findings;
- `not proven` boundaries and strict-only blockers;
- optional patch queue only when authorized, including red-green proof status or
  weaker-evidence rationale for testable remediation claims;
- delegated review return courier block when commissioned by
  `workflow-delegated-review-patch`;
- blocked states or overlay-bound result;
- review-use boundary: findings are input, not mandatory remediation, unless
  separately accepted or bound by an authorized lane;
- remaining blockers and next authorized step.

Do not convert advisory critique into strict claims. Do not present advisory
critique as a formal artifact-role verdict.

## Constraints

- Default artifact review is source-read-only.
- Do not edit reviewed artifacts.
- Do not execute patches.
- Do not review implementation/code in this lane.
- Do not review installed-copy or resolver behavior in this lane.
- Do not use installed, user-level, plugin, or project-local skill roots as
  canonical source for this repository.
- Do not import concrete artifact categories, report paths, result vocabulary,
  model routing, template policy, validation commands, output destinations, or
  source hierarchy into reusable kernel source.
- Do not create runtime code, build systems, plugin metadata, commits, remotes,
  pushes, installed skills, deployed copies, or promoted copies.

## Quality Bar

A valid artifact review must:

- use deep-thinking discipline for adversarial findings;
- preserve source-loading, trigger, lane-collision, and artifact-role
  boundaries;
- block undeclared lane collisions and unbound strict claims;
- keep correctness findings before friction findings;
- provide source-backed evidence for every finding;
- keep `minimum_closure_condition`, `next_authorized_action`, and
  `patch_queue_entry` distinct;
- preserve source-read-only review boundaries;
- keep patch queues inside the optional authorized adapter;
- prefer same-check red-green proof for testable remediation claims without
  forcing non-executable artifact findings into test-shaped validation;
- close with a review-use boundary so findings do not anchor later decisions as
  mandatory work without separate acceptance or execution authority;
- include the delegated review return courier when this lane was commissioned
  by `workflow-delegated-review-patch`;
- keep overlay-owned categories, result vocabulary, destinations, templates,
  model routing, source hierarchy, and validation gates out of reusable source;
- leave a clear next authorized step or blocked result.
