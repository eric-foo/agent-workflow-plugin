---
name: workflow-code-review
description: Workflow-kernel skill for implementation/code review. Use when explicitly invoking `workflow-code-review`, asking for `coding review`, `code review`, `review this code`, or `review this implementation`. Without a bound review lane, run zero-config findings-only advisory review against repo-visible evidence. Formal implementation review, patch queues, verdicts, and validation claims require strict overlay bindings. Do not use for generic review, artifact review, installed-copy review, postmortem review, brainstorming, style-only review, security-only review, or project-local review lanes without declared overlay precedence.
---

# Workflow Code Review

## Purpose

Run implementation/code review. By default, when no implementation-review lane is bound, run zero-config findings-only advisory review against repo-visible evidence. Formal implementation review remains strict and requires overlay-bound source authority.
Review target and purpose are commission-bound; do not silently retarget the
review, widen the implementation surface, or convert advisory critique into a
formal result because adjacent evidence is visible.

This skill supplies reusable implementation-review mechanics only. The active
project overlay owns review lane binding, required input packet shape, verdict
vocabulary, output destination, review output mode, required output paths,
validation gates, patch-queue routing, protected paths, artifact roles, and
collision policy with adjacent review tools.

This packaged skill supplies reusable code-review mechanics. The package copy
is runtime behavior; source candidates and installed cache copies are not
canonical source authority.

## Trigger Gate

Use this skill only when the user explicitly asks for implementation/code review, such as `workflow-code-review`, `coding review`, `code review`, `review this code`, `review this implementation`, or equivalent wording.

If the request provides or identifies an implementation-review packet and a project overlay binding for the implementation-review lane, use strict formal review.

If the request identifies repo-visible implementation source, a diff, PR diff, or change target but does not bind a review lane or packet, use zero-config findings-only advisory review. This includes requests phrased as `review this diff` or `review this PR` when a repo-visible implementation diff or change target is available. Findings-only advisory review must not include a formal PR verdict, formal review-lane verdict, overlay-bound result, severity taxonomy, overlay-bound blocked/ready state, patch queue, executor-ready handoff, mandatory remediation, mandatory validation claim, pass/fail completion claim, or protected edit approval.

If the user says only `review`, `review this`, or asks for general feedback
without identifying implementation source, a diff, PR diff, or change target,
do not infer this lane. Ask for the intended review lane or return
`BLOCKED_UNBOUND_REVIEW_LANE` when formal review was requested but the lane is
not bound.

## Source Loading Modes

Use zero-config advisory mode by default when no project overlay or equivalent authority is declared. Apply this claim-level contract: findings-only advisory review may proceed from repo-visible implementation evidence, while strict-shaped claims such as formal verdicts, severity taxonomies, overlay-bound blocked/ready status, approval, validation pass/fail, mandatory remediation, patch queue authority, executor-ready handoff, readiness, deployment, resolver behavior, or plugin readiness require explicit bound lane, decision criteria, gate, patch, readiness, or lifecycle authority. Reading more source can improve evidence, but it cannot create review-lane, validation, patch, readiness, lifecycle, or source-changing authority. Keep a source-read ledger, label evidence, and mark unsupported correctness, validation, runtime, readiness, or deployment claims as `not proven`.

Use strict mode for formal review verdicts, overlay-bound result vocabulary, severity taxonomies, overlay-bound blocked/ready status, implementation-review packets, patch queues, protected edits, mandatory remediation, mandatory validation, pass/fail claims, deployment or readiness claims, and executor-ready remediation handoff. Missing lane, packet, source authority, decision criteria, validation gate, collision precedence, output mode, or output destination blocks strict formal review.

## Direct-Entry Source And Context Intake

When this skill is invoked directly, do not require a prior repo-context
packet. Reorient from the current repository before review: read local
instructions, the implementation source or diff target, material workspace
facts, and local overlay or equivalent authority needed for any strict review
claim.

If a repo-context packet, task-local context pack, repo map, summary, or prior
thread note is provided, use it only as orientation. It does not replace the
trigger gate, input-packet gate, collision checks, review scope, findings
standard, formal-review blockers, or final result. Reread decisive current
source, diff, validation evidence, or bound authority before strict or
actionable claims.

Do not treat a context packet as evidence of automatic invocation, installed behavior, plugin readiness, resolver behavior, deployment readiness, packaging behavior, formal review authority, patch-queue authority, or source-changing authority.

## Required Bindings

Before strict formal review, bind:

- Implementation-review lane id, purpose, scope, excluded scope, and review-output
  write permission or no-write policy.
- Input packet requirements and concrete packet source.
- Source authority for the reviewed implementation, such as spec, contract,
  architecture decision, runtime context, tests, or validation evidence as
  defined by the overlay.
- Review output mode and destination: `filesystem-output` with bound
  `required_output_path` for durable review artifacts, or explicit
  `chat-output` for transcript-only review.
- Verdict vocabulary.
- Escalation and patch-queue routing.
- Collision notes for generic review, artifact review, installed-copy review,
  postmortem review, user-level skills, plugin tools, and project-local review
  lanes.
- Validation gates with pass, fail, blocked, and not-run semantics.
- Red-green proof expectations for testable remediation claims, including when
  same-check proof is mandatory, accepted as weaker evidence, or not
  applicable.

Return `BLOCKED_UNBOUND_REVIEW_LANE` when lane binding is missing. Return
`BLOCKED_LANE_COLLISION` when adjacent-review precedence is undeclared. Return
`BLOCKED_UNBOUND_VALIDATION_GATE` when mandatory validation expectations are
missing.

For strict, formal, durable, verdict-bearing, or patch-queue-bearing review,
preflight review output binding before full review. Missing mode returns
`BLOCKED_OUTPUT_MODE_MISSING`; `filesystem-output` without a valid
`required_output_path` or explicitly authorized deterministic derivation returns
`BLOCKED_OUTPUT_DESTINATION_UNBOUND`. Do not produce a full chat transcript as
the fallback for an unbound durable review.

Zero-config findings-only advisory review may proceed without these bindings when repo-visible source is available, but the missing bindings must be named as strict-only blockers or `not proven` boundaries.

## Input Packet Gate

For strict formal review, the input packet must identify:

- implementation or change under review;
- source authority used to judge the implementation;
- validation evidence or an overlay-accepted reason validation is not
  applicable;
- runtime or environmental context when correctness depends on it;
- patch instructions only for a follow-up review pass that is explicitly
  reviewing applied patches.

If required packet fields are absent, unreadable, malformed, stale, or
conflicting in a way that prevents review, return
`BLOCKED_MISSING_INPUT_PACKET`. Do not fill gaps from conversation history.

For zero-config findings-only advisory review, do not require an overlay packet. Use only the repo-visible source or diff identified by the user. If no target source, diff, or change can be identified, ask for the target or return `BLOCKED_MISSING_SOURCE`.

## Review Workflow

1. Bind repository rules and determine source-loading mode: zero-config findings-only advisory review or strict formal review.
2. Run the trigger gate.
3. Build a source-read ledger for repo-visible evidence and name dirty or untracked sources when material.
4. In strict mode, run the input packet gate, validation-gate preflight, and adjacent-review collision preflight.
5. For strict, formal, durable, or patch-queue-bearing review, run
   review-output preflight and block if `filesystem-output` lacks a bound
   `required_output_path`, or if `chat-output` is not explicit.
6. In zero-config mode, identify strict-only blockers and `not proven` boundaries before reviewing.
7. Inspect the implementation against bound source authority in strict mode or repo-visible evidence in zero-config mode.
8. Report hard correctness findings before risk commentary.
9. Separate input blockers, validation blockers, implementation findings,
   risks, and `patch_queue_entry` handoff entries.
10. Use the overlay-bound verdict vocabulary and output destination only in strict mode.
11. If patch handoff is authorized in strict mode, use the shared patch-queue
    fields and include verification evidence for every entry. For testable
    remediation claims, prefer same-check red-green proof: the same named test,
    fixture, check, or validation gate fails before the fix and passes after
    the fix.
12. When this review was commissioned by `workflow-delegated-review-patch`,
    append the delegated review return courier block so the home model can
    adjudicate the result in a later turn.

## Reusable Findings Standard

Each finding must include:

- finding id;
- commissioned review target and purpose;
- reviewed target or artifact role;
- location key, line, structural anchor, or search key;
- evidence from the implementation;
- authority or evidence basis;
- impact on correctness, validation, runtime behavior, or review confidence;
- `minimum_closure_condition`: what must become true before this finding can be
  treated as closed;
- `next_authorized_action`: what this review lane is currently authorized to
  do next;
- verification expectation, including red-green proof status when the finding
  leads to a testable remediation claim;
- whether a `patch_queue_entry` is authorized for this finding.

In strict formal review, the authority basis must come from bound source
authority. In zero-config findings-only advisory review, use repo-visible
evidence labels, state source gaps, and mark unsupported claims as `not
proven`.

A finding is reportable only when implementation evidence plus the authority or
evidence basis supports a concrete correctness, runtime, validation, or
review-confidence impact. Otherwise report it as a risk, source gap, or `not
proven`.

Do not suggest broad rewrites, style preferences, or remediation ownership
unless the overlay-bound lane asks for that output.

## Patch Queue Handoff

Use `patch_queue_entry` values only in strict mode when the overlay authorizes
executor handoff. Each entry must include the shared fields: finding id, target
file or artifact role, stable location key, exact requested change, minimum
closure condition, next authorized action for the receiving executor, affected
expected output or artifact role, expected behavior impact, verification
command or evidence, and executor verdict field.

For testable remediation claims, the preferred verification is same-check
red-green proof: the same named test, fixture, check, or validation gate fails
against the pre-fix baseline for the reviewed failure mode and passes after the
fix. When a new test or fixture is part of the remediation, the proof should
show the test-only or check-only state failing against the unfixed behavior
before the implementation change makes it pass.

If same-check red-green proof is unavailable, inapplicable, or too expensive
for the bound lane, the entry must say so and label the replacement evidence as
weaker. Weaker evidence must not be treated as an equivalent validation pass
unless the active overlay explicitly accepts that gate.

Return `BLOCKED_AMBIGUOUS_TARGET` when no stable target exists. Return
`BLOCKED_MISSING_VERIFICATION` when the executor cannot prove the patch
outcome.

Zero-config findings-only advisory review must not emit patch queues. It may mention likely remediation direction only as advisory prose, not executor-ready handoff.

## Delegated Review Return Courier

When code review is commissioned by `workflow-delegated-review-patch`, append a
clearly labeled courier block for the home model. This block is not the
reviewer's verdict becoming accepted truth; it is a transport packet for later
adjudication.

Use this shape:

```text
DELEGATED_CODE_REVIEW_RETURN_FOR_HOME_MODEL

Here is the delegated code review result. Adjudicate it under the
delegated-review-patch return contract.

Include:
- original commission or review target
- implementation context, diff, and reviewed files
- findings and implementation evidence
- proposed patch, diff, or exact requested edits, if authorized
- citations
- reviewer verdict
- validation evidence and not-run checks
- residual risk
- blockers, off-scope flags, and not-proven boundaries
```

## Non-Goals

This skill is not:

- generic review;
- artifact review;
- installed-copy review;
- postmortem review;
- product, architecture, or feature planning;
- implementation scoping;
- patch execution by default;
- style-only review;
- security-only review without overlay-bound security scope;
- a replacement for project-local review lanes without explicit overlay
  precedence.

## Overlay-Owned Behavior

Do not define globally:

- review lane names beyond this candidate name;
- packet schemas or exact required document types;
- verdict envelope or verdict names;
- output paths;
- validation commands;
- severity taxonomy;
- finding routing or remediation ownership;
- protected paths;
- paired or generated artifact rules;
- project lifecycle stage routing;
- precedence over installed, user-level, plugin, or project-local review tools.

## Failure States

- `BLOCKED_UNBOUND_REVIEW_LANE`: no overlay-bound implementation-review lane
  exists.
- `BLOCKED_LANE_COLLISION`: another review mechanism could answer the same
  request and precedence is undeclared.
- `BLOCKED_MISSING_INPUT_PACKET`: required packet fields are absent, unreadable,
  malformed, stale, or blocking-conflicting.
- `BLOCKED_UNBOUND_VALIDATION_GATE`: mandatory validation expectations are not
  declared.
- `BLOCKED_OUTPUT_MODE_MISSING`: durable or formal review was requested without
  explicit `filesystem-output` or `chat-output`.
- `BLOCKED_OUTPUT_DESTINATION_UNBOUND`: `filesystem-output` lacks a bound
  `required_output_path` or authorized derivation convention.
- `BLOCKED_NOT_RUN`: a mandatory validation gate was skipped without accepted
  deferral.
- `FAILED_VALIDATION`: a declared mandatory validation gate failed.
- `FAILED_REVIEW_WRITE_BOUNDARY`: review attempts source edits without overlay
  write authority.
- `FAILED_REVIEW_OUTPUT_WRITE`: durable review output could not be written.
- `BLOCKED_AMBIGUOUS_TARGET`: patch-queue target is not stable.
- `BLOCKED_MISSING_VERIFICATION`: patch-queue outcome cannot be verified.
- `FAILED_REVIEW_LEAKAGE`: source imports project-local routing, packet schema,
  verdict envelope, output discipline, or remediation routing.
- `BLOCKED_MISSING_SOURCE`: no repo-visible target source, diff, or change can
  be identified for zero-config findings-only advisory review.

## Leakage Bans

Reusable source must not import project-specific lifecycle routing, local packet
schemas, fixed verdict envelopes, local output discipline, local remediation
routing, or project-specific insufficient-context rules.

Known banned leakage includes S-stage routing, fixed spec/ADR/contract packet
schema as universal policy, `artifact_root_cause` routing, local verdict
envelopes, and local review-output paths.

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

- loaded source, overlay bindings if present, and strict-only blockers if
  absent;
- source-loading mode: zero-config findings-only advisory review or strict formal review;
- source-read ledger and evidence labels;
- trigger-gate result;
- input-packet status;
- validation-gate status;
- review output mode, report path or path blocker, and write status when
  formal or durable review is requested;
- collision-gate status;
- review scope and excluded scope;
- findings with evidence, minimum closure conditions, and next authorized
  actions;
- `not proven` boundaries and strict-only blockers;
- risks with evidence when the overlay requests risk reporting;
- `patch_queue_entry` values only when authorized, including red-green proof status or
  weaker-evidence rationale for testable remediation claims;
- delegated review return courier block when commissioned by
  `workflow-delegated-review-patch`;
- blocked states or overlay-bound verdict;
- remaining blockers and next authorized step.

Do not claim pass, success, or completion when lane binding, input packet,
validation gates, collision precedence, or output destination are unbound.
Do not present zero-config findings-only advisory review as a formal review verdict.
