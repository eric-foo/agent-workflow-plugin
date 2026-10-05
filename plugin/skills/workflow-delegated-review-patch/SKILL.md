---
name: workflow-delegated-review-patch
description: Source-only workflow-kernel candidate for commissioning a de-correlated, different-family review-and-patch hardening pass on authored artifacts, with home-model adjudication. Use when explicitly invoking `workflow-delegated-review-patch`, asking to commission delegated/de-correlated review-and-patch, using `delegate patch`, `delegate prompt review`, or `delegate prompt patch` (including `delegate the prompt review/patch` / `delegate review/patch prompt`) when intent is this lane rather than ordinary subagent delegation, asking a different-family reviewer to critique and propose a bounded patch, bringing back delegated reviewer output for adjudication, or authoring/validating this candidate. Reads active overlay bindings; dormant/advisory when unbound. Do not use to run artifact review or code review, bypass the overlay-selected prompt renderer, or claim deployment/install/resolver/readiness.
---

# Workflow Delegated Review-And-Patch

## Purpose

Produce one guardrail-complete commission for a **de-correlated, different-vendor review-and-patch hardening pass** on one or more high-stakes authored artifacts, plus the **home-model adjudication contract** that decides what is kept. A de-correlated controller in an independent receiving lane reviews and patches the submitted scope; the home model then adjudicates the result.

The value is that the commission is repeatable: a hand-rebuilt commission drifts and drops the invariants that make the pass trustworthy. This skill carries them in one place. It has no one-tier mode; a same-family home-model self-review is outside this lane.

The active project overlay owns every project fact: opt-in and status, the model ladder, the protected-path list, the operating-contract section, preflight evidence, source-context fields, prompt routing, and output destinations. The kernel reads those bindings and hardcodes none of them; it never imports another project's paths, lifecycle labels, model rungs, validation commands, review labels, or product facts.

It claims no validation, readiness, resolver behavior, or auto-keep authority.

## Boundary

This skill owns the roles, invariants, mode selection, commission-input and output contracts, the home-model adjudication contract, the review-return adjudication shape, and how the overlay interface is read.

It does not own:

- running the review: it routes the controller to a kernel review lane (see Review Method);
- prompt mechanics, worktree preflight, hash pins, or output-mode bindings: the overlay selects the renderer through `prompt_routing`, defaulting to `workflow-prompt-orchestrator` and its `review` / `patch` template; an explicitly authorized compact renderer may produce the strict prompt inline under that overlay contract;
- model lanes, protected paths, operating-contract locations, preflight schemas, source fields, destinations, or project routing and sequencing;
- semantic readiness, validation gates, formal verdicts, auto-keep, or off-scope protected-path edit permission;
- installed-copy, resolver, plugin, packaging, promotion, or automatic-hook behavior, and edits to installed, user-level, plugin, global, or project-local skill roots, unless a later deployment turn explicitly authorizes them.

Do not install, deploy, promote, package, rename, or shadow skills without later explicit authorization.

Commissioning prepares the prompt through the selected renderer; it does not execute the review or apply its patch. A strict prompt request must not end with the commission contract alone.

## Roles

Keep the three role names distinct:

- **Author / CA / home model**: authored the artifact; adjudicates the final result; owns what is kept.
- **Controller**: the de-correlated reviewer. Owns judgment, findings, fixes, and citation bindings, and the verdict relative to the executor; it is **not** final over the CA.
- **Patch executor**: applies bounded edits only. Default is a deterministic apply (`git apply` or an exact-replacement edit); a cheap model tier is used only for semi-structured packets or graceful block reporting. It creates no findings and never creates or validates citations.

## Invariants

Every commission carries these, in both modes:

- **De-correlation is a who-constraint with a receipt.** The controller must be a different vendor or family from the author. State it as who may serve as controller, never as a claim that the controller model performs better; the commission carries **no `Recommended model` block**. Strict commissions record the actor / model-family receipt (see Commission Inputs) before review or patch work begins.
- **Edit only the submitted scope; flag everything else.** When the correct fix lies outside the bounded patch scope (an off-scope protected, canonical, or test path, an architectural or design change, or the source upstream of a submitted generated artifact), the delegate **flags it and does not edit it.** The overlay owns the protected-path list.
- **No tester/testee shortcut.** The authoring, commissioning, or adjudicating model must not review its own work or directly spawn a reviewer to satisfy this lane, and grants no recursive or unrelated subagent authority. The review occurs in the independent receiving lane. Self-review is a blocker, never a fallback: if no independent lane or de-correlated controller can be produced, return the nearest de-correlation or renderer blocker, not a review result. Courier prompt preparation follows its separate identity rule (see Overlay Interface).
- **The CA adjudicates before anything is kept.** Carry this review standard verbatim into both modes:

  The delegate's (controller's) citations and changes are **decision input only.** The home model / CA reserves **final authority** over what is kept and may **veto any change at its discretion** when it judges the change adds no benefit or is net-negative — even an individually defensible change may be rejected. This is the standard "claims to adjudicate, not premises to inherit." Citations must be **neutral in tone but decision-sufficient in substance**: the delegate's argument lives in the verdict and residual-risk note, not in the citations. Thin citations push the CA back onto its own priors and defeat de-correlation.
- **Citation authority stays with the controller.** When author and adjudicator share a family, the citation map is what makes adjudication evidence-based rather than blind-spot-correlated, so the executor may carry citations but never creates or validates them.
- **Use the overlay-selected renderer.** Read `prompt_routing` for renderer, delivery mode, and eligibility; use an authorized compact renderer when its conditions hold, otherwise `workflow-prompt-orchestrator`. Preserve the selected route's source-loading, worktree, revision, dirty-state, target, validation-evidence, authority, freshness, output-mode, and destination gates. The receiver inspects the pinned source directly; no summary, context pack, alternate checkout, or recreated source may substitute. A real gate failure is never evaded by switching renderers.
- **`NEEDS_ARCHITECTURE_PASS` is the escalation valve.** A design-level problem stops patching, reverts the partial patch, and returns findings only.
- **No new authority.** No validation, readiness, auto-keep, or edits outside the operator-submitted, CA-adjudicated scope; respect the overlay's provisional status. The token-saving figures (controller roughly 30–45 percent, total multi-agent roughly 10–25 percent, higher with exact diffs) are unmeasured hypotheses; the cheap first measurement is logging controller tokens with and without the split.

## Commission Inputs

1. One or more **named targets**. With multiple artifacts, give each a short label tag (e.g. `[auth-handler]`) that every finding, diff hunk header, and citation carries. Whatever is submitted, within the bounded patch scope, is the editable scope: the operator chooses what to harden and the CA adjudicates every change, so there is no protected-path *target* block. An overlay may mark a specific path *never-target*; that is its explicit call, not a kernel default.
2. **Why** source-read-only review is insufficient: one shared reason, or per-artifact reasons when targets differ.
3. A **bounded patch scope**: shared or per-artifact.
4. An **actor / model-family receipt** for strict commissions: author/home model family, controller model family, current receiving actor role (`home-dispatcher`, `controller`, or `patch-executor`), dispatch mode, and de-correlation status.
5. **Roles and models** from the overlay's model ladder. The controller must be de-correlated from the author; the executor is a cheap mechanical tier and need not be.

## Review Method

The controller runs the kernel review lane for the target type, in its own runtime:

- a non-code artifact: `workflow-adversarial-artifact-review`;
- code, an implementation, a diff, or a change packet: `workflow-code-review`;
- a mixed target: code claims in the code lane, non-code claims in the artifact lane.

If the lane is unavailable, return `BLOCKED_REVIEW_LANE_UNAVAILABLE` with the reason and do not patch; never emulate the lane inline. Advisory commissions may name the intended lane but must not claim it ran. Run the review at the lane's own claim level (advisory findings unless the overlay binds a formal lane). The bounded patch is separately authorized remediation on top; de-correlation is what this skill adds to the lane.

## Mode Selection

Default to **base-subagent**. Engage **split-executor** only when *all* hold: the source pack is large, the findings are already decided, the patch is mechanical enough to specify exactly, and a final controller readback is mandatory. Never for small judgment-coupled artifacts, where the extra hop and exact-packet authoring are not amortized.

### base-subagent

The rendered prompt names the receiving actor's role. A receiver that is already the controller verifies from the receipt that it is de-correlated from the recorded author/home family and proceeds; it does not launch a replacement controller. The controller treats the overlay's operating-contract section as its contract, invokes the review lane, then **reviews and patches each target directly in the working tree (no commit)** and returns:

- a unified diff, each hunk prefixed with its label tag when there are multiple artifacts;
- **per-change source citations**, each referencing its label tag;
- a **verdict**: one overall, plus per-artifact sub-verdicts when targets differ materially;
- a **residual-risk note**.

The home / CA model then adjudicates per change (accept, modify, or reject against the citations and the artifact's intent), reverting rejected hunks.

### split-executor

1. **Controller** reviews the target and decision-bearing source and produces an **exact patch packet**: target file, exact edits, citation map, constraints, and no-discretion rules.
2. **Patch executor** reads only the target and the packet and applies the edits, **preferring a deterministic apply for an exact unified diff or exact replacement blocks**. A model executor is only for semi-structured packets or graceful `PATCH_BLOCKED` reporting. It does not review, infer, improve, or add citations; it reports `PATCH_APPLIED` or `PATCH_BLOCKED`, the changed file, a diff or hash, and any failed patch ids.
3. **Controller final check** (readback layer 1): fresh-reads the changed file and diff and confirms intent and citations still hold. Catches executor mis-apply.
4. **CA adjudication** (readback layer 2, never merged with layer 1): decides what is kept.

## Overlay Interface

Resolve the active project overlay and read this interface before drafting anything strict:

```text
{ opt_in/status, operating_contract_pointer, protected_path_list, model_ladder,
  prompt_routing, prompt_orchestrator_available, preflight_schema, source_context_fields,
  output_destinations }
```

If a field required by the selected phase and route is absent, go dormant, advisory, or blocked for what depends on it. Use only the explicit defaults below; never invent project bindings:

- no `opt_in/status` → dormant disclosure; at most an advisory description, not a bound commission;
- no `operating_contract_pointer` → `BLOCKED_OPERATING_CONTRACT_UNBOUND`;
- operator-courier prompt preparation: an unknown controller identity or unavailable local controller does not block rendering. Carry the author/home family, the explicit different-vendor who-constraint, the receiving role, and a pending controller identity/status; require the receiver to complete and verify the receipt before any review or patch. Do not probe availability or dispatch when the overlay makes this courier-only. A known same-family target must not be described as eligible;
- actual dispatch or a receiving controller's execution: missing or inconsistent identity evidence → `BLOCKED_DECORRELATION_RECEIPT_MISSING`; an ineligible controller → `BLOCKED_CONTROLLER_NOT_DECORRELATED`; an unbound required execution model lane → `BLOCKED_MODEL_LANE_UNBOUND`. No review or patch begins under pending courier identity;
- use `prompt_routing` when bound. If it selects the full renderer, or no override exists, a missing `workflow-prompt-orchestrator` → `BLOCKED_PROMPT_ORCHESTRATOR_UNAVAILABLE`. An eligible compact route does not require the full renderer; its other gates still apply;
- no `output_destinations` for the controller's diff, citations, verdict, or the adjudication record → `BLOCKED_OUTPUT_DESTINATION_UNBOUND`;
- no `protected_path_list` → the off-scope boundary is unproven; the delegate stays strictly within the submitted scope and flags everything off-scope.

The model ladder pattern is **author → de-correlated controller → cheap executor**; read concrete names, vendors, and rungs from the overlay's `model_lanes`. De-correlation applies to the controller only; the executor is a cheap tier of the controller's family.

The completed receipt is an execution preflight. A receiving `controller` must show the different author/home family and its own family, then proceeds without re-dispatching. A `home-dispatcher` may dispatch only a controller whose recorded family differs from the author/home family. A `patch-executor` performs only mechanical apply work and cannot satisfy controller de-correlation.

## Prompt Rendering and Delivery

The overlay's `prompt_routing` selects the renderer and delivery mode; `workflow-prompt-orchestrator` is the default. An eligible compact route renders the same load-bearing commission under the overlay's compact contract; full routing applies only when the selected route requires it. Review finding schemas and output bindings stay with the invoked review lane.

An explicit delegated review-and-patch invocation, including `delegate patch`, `delegate prompt review`, and `delegate prompt patch`, requests the prompt that lets the independent review occur, unless the user asks for commission-only, handoff-only, advisory design, or review-return adjudication. The shorthand never authorizes patch execution or dispatch.

For prompt intent, apply the selected renderer in the same turn and return exactly one terminal result:

- `orchestrated_prompt`: the complete prompt under the selected contract, compact or full (the name describes a completed prompt, not a requirement to invoke the full orchestrator);
- `prompt_orchestrator_handoff`: only when handoff-only or commission-only output was requested, or the required renderer cannot proceed and the handoff carries the precise blocker; or
- the nearest preserved binding or renderer blocker.

Lack of launch authority, billing access, or a local controller (for example an owner-only or billed review command) never suppresses or downgrades an authorized operator-courier prompt: render it with the who-constraint as an operator paste-instruction and the receiver's identity check. Naming an owner-run path may supplement the prompt but never replaces it. Missing pinned source, disallowed dirty state, unbound scope, or another real prompt gate still blocks.

A carried implementation review checkpoint follows the same rules: render the prompt or precise blocker without asking again whether to route or stopping at a commission summary. Rendering does not satisfy a required review; the review, bounded patch, adjudication, and any guarded resume remain pending under their own authority.

Use `advisory-commission` only for explicitly requested advisory design: emit a paste-ready advisory prompt, label it non-bound, mark unbound fields as placeholders or operator-owned, route to the review lanes without claiming they ran, preserve CA adjudication before keep, and make no path, hash, preflight, resolver, or validation claim. Never downgrade an actionable strict prompt to advisory to evade a real gate.

Write files or claim paths and hashes only under the selected renderer's output authority and verified evidence; authorized compact chat rendering needs neither a saved path nor a hash.

## Review Return Adjudication

When the user brings back delegated controller output, adjudicate it directly. This is a home-model adjudication turn, not a new delegated review and not a claim that the review lane ran in this turn; do not render a new commission by default.

Inputs, when available: the original target or commission; the reviewer's findings, diff or patch packet, citations, verdict, residual risk, blockers, and off-scope flags; validation evidence or explicit not-run status.

Treat the returned output as claims:

- accept, reject, or modify each material finding and proposed change;
- decide what, if anything, is kept;
- state what was implemented before review and the final state after adjudication;
- state validation evidence, gaps, remaining risk, and any blocked next step;
- emit `operator_closeout_source`: a compact factual packet a human reader can use.

Emit `humanised_closeout` only when the user explicitly asks for plain-language output; it is a surface transform of `operator_closeout_source` and must not add, remove, or soften facts, caveats, gaps, or risk.

## Commission Contract

Supply these load-bearing fields to the selected renderer. A compact route may express them as dense prose under the overlay contract; a full route hands them to `workflow-prompt-orchestrator`. Pending courier identity is allowed only until execution preflight.

```md
# Delegated Review-And-Patch Commission

## Lane Binding
- overlay_status: provisional | bound | dormant
- operating_contract_pointer: <overlay section the controller treats as its contract>
- review_lane: artifact (workflow-adversarial-artifact-review) | code (workflow-code-review) | advisory-when-unbound
- mode: base-subagent | split-executor
- actor_model_family_receipt:
  - author_home_model_family: <recorded family of author / CA / home model>
  - controller_model_family: <recorded family of controller; must differ from author/home family>
  - current_receiving_actor_role: home-dispatcher | controller | patch-executor
  - dispatch_mode: external-controller-courier | runtime-subagent | split-executor
  - de_correlation_status: satisfied | pending-receiver-verification (courier preparation only) | blocked
- de_correlation: controller is a different vendor or family than the author (who-constraint, not a model recommendation); if the receipt is missing, inconsistent, or blocked, return BLOCKED_DECORRELATION_RECEIPT_MISSING or BLOCKED_CONTROLLER_NOT_DECORRELATED before review or patch work
- subagent_authority: no tester/testee shortcut; the authoring, commissioning, or adjudicating model must not launch a reviewer to review its own work; controller execution belongs to the independent receiving lane named by the prompt; if current_receiving_actor_role is controller, do not launch a replacement controller; no recursive or unrelated subagents
- prompt_rendering: use the overlay-selected compact or full renderer; absent an override, use workflow-prompt-orchestrator. The receiver inspects the pinned repo directly. Preserve the selected route's source-loading, worktree, freshness, dirty-state, target, validation-evidence, output-mode, and destination blockers; no substitute-source review.

## Target
- targets:
    - label: <short searchable tag, e.g. [auth-handler]>
      path: <artifact path>
      bounded_patch_scope: <what the patch may touch within this artifact>
    - label: <short searchable tag, e.g. [schema-v2]>   # repeat for each artifact; omit block if single target
      path: <artifact path>
      bounded_patch_scope: <what the patch may touch within this artifact>
- why_read_only_insufficient: <why source-read-only review cannot do the job; shared reason or per-label when targets differ>
- off_scope: read-only — flag, don't edit (off-scope protected/canonical/test paths, architectural changes, generated-artifact sources); overlay owns the protected-path list

When multiple artifacts are submitted, all diffs and citations must carry the artifact's label tag so any finding or hunk can be located by searching that tag.

## Roles (who-constraints, not recommendations)
- author / CA / home model: adjudicates; owns what is kept
- controller (de-correlated): judgment, findings, fixes, citations; verdict relative to the executor, not final over the CA
- patch executor: mechanical apply only; default deterministic apply; creates no findings; never creates or validates citations

## Controller Output Contract
- invoke the selected review lane (`workflow-adversarial-artifact-review` for artifacts, `workflow-code-review` for code); if unavailable, return `BLOCKED_REVIEW_LANE_UNAVAILABLE` with the reason and do not patch
- review findings in the invoked lane's finding schema, with per-change source citations neutral in tone and decision-sufficient in substance; each finding and citation must carry the artifact's label tag when multiple targets are submitted
- unified diff (working tree, not committed) as the bounded patch for accepted findings; each hunk header must include the artifact's label tag (e.g., `# [auth-handler]`) so findings can be searched by label
- verdict and residual-risk note; one overall verdict plus per-artifact sub-verdicts when targets differ materially
- escalation: NEEDS_ARCHITECTURE_PASS on a design-level problem (stop patching; revert partial; findings only)

## Adjudication Contract (home / CA model)
- the diff, citations, and verdict are claims to adjudicate, not premises to inherit
- accept / modify / reject per change against the citations and the artifact's intent; revert rejected hunks
- final authority to veto any change at discretion, even an individually defensible one
- split-executor only: keep the two readback layers non-merged — controller final check (catches executor mis-apply), then CA adjudication (decides what is kept)
```

## Output

Report exactly one lane mode:

- `bound-commission`: the overlay binds the requested delivery, operating contract, scope, and destinations; produce the commission through its selected renderer (courier receiver fields may stay pending until execution preflight);
- `advisory-commission`: some fields present but status provisional or advisory, and advisory design was explicitly requested; unbound fields marked as placeholders, no strict claim;
- `dormant`: opt-in or status absent; dormant disclosure only;
- `blocked`: a required field for the requested strict commission is missing; the precise blocker.

Then return: which overlay was loaded and which interface fields were present versus unbound; the receipt and de-correlation status for strict commissions; the terminal output (`orchestrated_prompt`, `prompt_orchestrator_handoff`, `advisory-commission`, `dormant`, or `blocked`); the complete prompt or precise blocker for strict prompt requests; overlay-owned placeholder fields; the selected renderer and delivery mode with its worktree, identity, and output bindings; and the source-only, provisional, and unmeasured-estimate non-claims. For review-return adjudication, return the items listed under Review Return Adjudication.
