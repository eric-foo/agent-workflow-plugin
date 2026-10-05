---
name: workflow-prompt-orchestrator
description: Prompt orchestration skill for prompts, wrappers, handoffs, review prompts, reruns, and patch prompts. Prepares prompts only; never executes.
---

# Workflow Prompt Orchestrator

## Purpose

Create or adapt prompt artifacts and thin wrappers while preserving local source authority, output mode, worktree preflight, validation expectations, and retry boundaries.

This skill supplies generic prompt mechanics only. The active project overlay owns source hierarchy, artifact roles, template registries, output destinations, model lanes, review lanes, workflow sequencing, routing rules, validation commands, domain facts, and any project-specific process policy.

The orchestrator prepares the prompt or wrapper. It does not perform the downstream planning, review, patch, implementation, validation, or rerun task described by that prompt.

## Authority And Claim Contract

Apply this contract at the claim level:

```text
Advisory prompt drafting may proceed from visible evidence.
Strict claims require explicit bound authority.
Reading more source can improve evidence, but cannot create authority.
```

Use only the active project overlay, authorized project templates, user-provided facts, and visible source evidence for prompt content. Do not import another project's paths, process stages, model lanes, review labels, validation commands, product facts, lifecycle policy, or prompt destinations.

Strict-shaped prompt claims include saved artifacts, prompt paths, SHA256 values, source-of-truth prompts, executable patch queues, executor-ready handoffs, validation success, readiness, formal verdicts, resolver behavior, plugin readiness, packaging, install, deployment, and source-changing authority. If the user asks for a strict-shaped claim and authority is missing, return the most precise blocker: `BLOCKED_OUTPUT_MODE_MISSING`, `BLOCKED_AUTHORITY_ORDER_MISSING`, `BLOCKED_UNBOUND_ARTIFACT_ROLE`, `BLOCKED_UNBOUND_VALIDATION_GATE`, `BLOCKED_UNBOUND_PATCH_AUTHORITY`, `BLOCKED_TEMPLATE_REGISTRY_UNBOUND`, `BLOCKED_TEMPLATE_KIND_UNSUPPORTED`, `BLOCKED_MODEL_LANE_UNBOUND`, `BLOCKED_OUTPUT_DESTINATION_UNBOUND`, `BLOCKED_BY_AUTHORIZATION`, or `BLOCKED_UNBOUND_LIFECYCLE_AUTHORITY`.

Unsupported strict claims are `NOT_CLAIMED`; ordinary advisory gaps are `not proven`. Neither label grants permission or readiness.

## Overlay Source-Loading Resolver

When generating any prompt artifact, resolve the active project overlay first. If the overlay declares a source-loading or retrieval policy, load and apply it as retrieval-only authority before generating the prompt. Do not hardcode project-specific source-loading files, repo maps, read packs, prompt paths, review labels, validation commands, model lanes, or source hierarchy in this skill.

If no overlay source-loading policy is declared, continue with the generic claim contract using visible evidence only. Mark source-pack completeness, source hierarchy, and strict source authority as `not proven` or placeholder-bound.

For reruns, preserve frozen context and unresolved delta. Re-run source loading only when the unresolved issue is a source gap, source conflict, stale source, missing authority, changed target source scope, or overlay-required retrieval update.

## Direct-Entry Source And Context Intake

When invoked directly, do not require a prior repo-context packet. Reorient from the current repository before drafting: read local instructions, user-named targets, requested receiving stage, output mode needs, material workspace facts, and local overlay or equivalent authority needed for the prompt claim.

Any supplied repo-context packet, task-local context pack, repo map, summary, or prior thread note is orientation only. It does not replace template-kind selection, output-mode binding, destination authority, validation expectations, patch/executor authority, or final prompt boundary. Reread decisive current source or bound authority before strict or executor-ready handoff claims.

## Goal Handoff Intake

When a `goal_handoff` is supplied, preserve it as downstream goal context. Treat
`anchor_goal` as the current workstream optimization target and
`success_signal` as the receiver's output-fit check. Do not mutate either field
silently, and do not treat the handoff as template authority, output-mode
authority, edit permission, validation evidence, acceptance, or readiness.

For handoffs, wrappers, and next-thread prompts, include the supplied
`goal_handoff` block verbatim unless the user explicitly asks to omit it.

`goal_handoff` / `thread_operating_target` YAML shape and continuity rules are owned by the active overlay's prompt-orchestration source. When a visible `thread_operating_target` is present, carry it forward verbatim only when continuity is warranted and its `lifecycle_status` is `active_thread_local`; surface conflicts before generating the prompt. A continuity disclosure block must accompany carried-forward or explicitly-omitted targets. Do not infer workflow sequencing, source authority, validation, readiness, approval, lifecycle completion, edit permission, or template authority from `goal_handoff` or `thread_operating_target`. If the active overlay is absent and a `thread_operating_target` decision is required for strict routing, return `BLOCKED_UNBOUND_LIFECYCLE_AUTHORITY`.

## Overlay-Owned Routing And Sequencing

Workflow sequencing means the project-owned order among stages, lanes, methods,
prompts, reviews, handoffs, wrappers, and next authorized actions. It is not the
source-loading order.

When a prompt, wrapper, or handoff chooses or describes a receiving stage, lane,
method sequence, next action, or route, resolve that sequence from the active
project overlay, an accepted project workflow or prompt artifact, or explicit
user instruction. Do not infer it from generic workflow skill names, source-pack
tiers, `goal_handoff`, `thread_operating_target`, or another project's overlay.

If project-specific sequencing is required but unbound, return
`BLOCKED_UNBOUND_LIFECYCLE_AUTHORITY`, `BLOCKED_MODEL_LANE_UNBOUND`,
`BLOCKED_TEMPLATE_REGISTRY_UNBOUND`, or the nearest overlay-defined blocker
instead of generating an executor-ready, stage-routed, or model-routed prompt.
For advisory chat-only prompts where formal routing is not claimed, mark sequence
status as warning-only unbound.

Structured handoffs may carry compact routing state:

```yaml
workflow_sequence_policy: overlay_owned
workflow_sequence_source: active_overlay | accepted_project_artifact | explicit_user_instruction | unbound
workflow_sequence_status: bound | warning_only_unbound | blocked
```

## Output Mode Resolution

Output-mode vocabulary is owned by the active project overlay. The skill resolves the overlay's declared mode list and binds exactly one mode per prompt.

When no overlay binds modes, the skill may use only `chat-only` and `paste-ready-chat` as universal fallbacks:

- `chat-only`: return the prompt in chat; do not write files; do not claim a path or hash. Use for ordinary inline prompt requests when no stricter mode is required.
- `paste-ready-chat`: return one fenced paste-ready prompt in chat; do not write files; do not claim a path or hash. Default for reusable or cross-recipient prompt requests (prompts destined for another model, agent, thread, worktree, stage, or review lane) unless the user or overlay specifies otherwise.

For any durable-artifact output (written files, saved artifacts, disposable drafts) when no overlay is active, return `BLOCKED_OUTPUT_MODE_UNBOUND`. Do not invent durable output modes; the overlay must declare them.

**Named limitation:** projects without an active overlay lose access to disposable-draft and saved-artifact output modes. Chat-only and paste-ready-chat remain fully functional without an overlay.

## Template Kind Resolver

Normalize natural language requests into one template kind before generating content:

| Request shape | Template kind |
| --- | --- |
| product direction, product bet, product planning prompt | `product` |
| feature plan, feature planning prompt, accepted product direction handoff | `feature` |
| deep think, option comparison, decision stress test | `deep-thinking` |
| handoff, next thread, next agent, implementation handoff, takeover prompt | `handoff` |
| review prompt, audit prompt, artifact review, source review | `review` |
| rerun, retry, recheck, patch recheck, unresolved finding follow-up | `rerun` |
| patch prompt, patch queue, bounded fix prompt | `patch` |
| full prompt, standalone prompt, prompt artifact | `full-prompt` |
| wrapper, thin wrapper, launch wrapper, paste wrapper | `thin-wrapper` |

If the request names multiple kinds, select the smallest complete kind that satisfies the user request and report what was deferred. If the kind remains ambiguous after reading available overlay authority and user context, ask one concise clarification or return `BLOCKED_TEMPLATE_KIND_UNSUPPORTED`.

`patch` means a bounded patch-prompt template. It does not by itself authorize patch execution, source edits, or executable patch queues.

## Prompt Naming And Titles

When the requested prompt is adversarial, make that adversarial role explicit in the visible prompt title and in any filename derived or proposed by the orchestrator. Use a snake_case token that includes `adversarial` and the review kind, such as `adversarial_review`, `adversarial_implementation_review`, `adversarial_code_review`, or `adversarial_artifact_review`. A generic title or filename that omits the adversarial token is insufficient.

Apply the adversarial token to the prompt title for chat-only prompts; to both the title and the filename or referenced prompt path for saved or disposable artifacts, wrappers, and handoffs. If an exact user- or overlay-supplied path lacks the adversarial token, do not silently rename it — return `BLOCKED_OUTPUT_DESTINATION_UNBOUND` or ask for a corrected path.

## Binding Matrix

| Binding | Chat-only prompt | Saved or disposable artifact | Thin wrapper / handoff | Patch / rerun |
| --- | --- | --- | --- | --- |
| output mode | required; default to `paste-ready-chat` for reusable or cross-recipient prompt requests; may default to `chat-only` for ordinary inline prompt requests | required | required | required |
| template kind | required | required | required | required |
| overlay authority | use if supplied; use placeholders when absent | required for project-owned fields | required for project-owned fields | required for project-owned fields |
| artifact destination | not required; do not claim path/hash | required | required if path/hash/destination is claimed | required if writing |
| downstream review-output binding | not required unless the prompt asks for formal or durable review | required for formal or durable review prompts | required for review handoffs that expect durable reports | required for review-rerun or patch prompts that expect durable review reports |
| edit permission | not required | required for writes | required if write/edit expected | required for writes or patch execution |
| workspace preflight | not required unless repo-bound | required when destination or source scope is repo-bound | required when repo-bound | required when repo-bound |
| model lane | warning-only unless model routing is requested | warning-only unless model routing is requested | bind if routed | bind if routed |
| validation gate | placeholder unless supplied; no pass/fail claims | bind if claimed | bind if claimed | bind if claimed |
| patch/executor authority | not required; no executable queue | not required unless executor-ready | required only for executor-ready handoff | required for executable patch queue or patch execution |

## Template Discovery

Use this order:

1. Active project overlay template registry, if present and bound.
2. Project-local prompt templates, if present and authorized by the active overlay or explicit user instruction.
3. Canonical generic fallback templates in `templates/` next to this `SKILL.md`.
4. Block with `BLOCKED_TEMPLATE_KIND_UNSUPPORTED` or `BLOCKED_TEMPLATE_REGISTRY_UNBOUND` if no suitable template exists.

User-supplied template paths or inline templates count as project-local inputs only when they are within the authorized read scope. When using a project template, preserve the project-owned fields and do not rewrite local policy unless the user asks for a patch. When using a fallback template, include placeholders for all project-owned fields instead of inventing defaults.

Do not silently fall back when a bound registry says the requested kind is project-specific, requires a named template, or forbids generic fallback. In that case, report the missing binding or template as blocked.

When using a bundled fallback template, disclose that the prompt did not use a project template and mark project-owned placeholders explicitly.

## Model Lane And Output Frame Resolver

Resolve the model lane and output frame in this order:

1. Active project overlay or template registry.
2. Authorized project-local template metadata.
3. User instruction for this prompt.
4. Bundled generic template return shape.

If a model lane is bound, include it in the generated prompt and wrapper. If no lane is bound and the user did not require model-specific routing, set `model_lane: unbound`, continue with a warning-only boundary, and use the output frame supplied by the selected template. If the user asks for a specific model lane, stage lane, or model-routed handoff and the active overlay does not bind it, return `BLOCKED_MODEL_LANE_UNBOUND`.

For implementation handoffs or model-routed executor prompts, resolve model lane from active overlay authority, authorized project template metadata, or explicit user instruction. Default to the project-bound normal implementation lane when bound; escalate only when the active prompt or overlay explicitly triggers executor judgment or false-pass risk. Do not hardcode concrete model names. If model routing is required but the relevant project lanes are unbound, return `BLOCKED_MODEL_LANE_UNBOUND`.

For model-routed prompts or wrappers, put the model-lane status in the final footer at the bottom of the entire message — not before the generated prompt, blockers, or next-step content.

Output frame means the required response shape for the receiving model: summary vocabulary, verdict vocabulary, report destination, and artifact vs. summary vs. findings vs. patch-queue return shape. Do not infer a project-owned output frame from another project or from adjacent template names.

## Prompt-Type Contracts

### Chat-Only And Paste-Ready

When output mode is `chat-only` or `paste-ready-chat`, do not write prompt files. Return the generated prompt or wrapper in chat, state that no prompt path or SHA256 exists, and preserve any saved-artifact destination as an overlay-owned placeholder. Use clearly marked placeholders for missing project-owned fields. Unbound model lane, report destination, artifact role, validation gate, or workspace preflight is warning-only unless the generated prompt makes a strict claim that depends on it.

For paste-ready prompts, wrap the full prompt in one outer four-backtick `markdown` fence when the prompt is meant to be pasted into another model, agent, thread, worktree, or review lane.

### Saved Or Disposable Artifact

When output mode requires a written prompt, write only if the active overlay or disposable workflow-state rules bind the destination and write permission. `disposable-draft` is temporary output and must keep the non-durable boundary visible. `saved-artifact` is durable filesystem output and requires artifact role, destination, exact path or authorized derivation, freshness expectations, and write authority. Otherwise return `BLOCKED_OUTPUT_MODE_MISSING`, `BLOCKED_OUTPUT_DESTINATION_UNBOUND`, or `BLOCKED_UNBOUND_ARTIFACT_ROLE`. Do not claim a prompt path, SHA256, durable destination, or source-of-truth artifact exists unless it was actually written under bound authority.

### Thin Wrapper And Handoff

A thin wrapper references the prompt path (or states `chat-only inline prompt` when no file exists) and prompt revision or SHA256 when a file exists; it must not restate or fork project policy. Full required-field lists and handoff contract details are owned by the active overlay's prompt-orchestration source.

For a handoff prompt, carry the downstream workflow instruction in the prompt
itself instead of asking the operator to add a second message. When the bound
next act explicitly authorizes implementation, open with `Success implement the
commissioned work in:` followed by the handoff path. When the bound next act is
planning-only, open with `Plan the commissioned work in the packet with success
implement. Do not implement.` followed by the handoff path. For review,
diagnosis, read-only, or otherwise non-implementation handoffs, keep the neutral
continuation opening. This wording selects an already-bound workflow; it does
not supply missing authority or convert planning into execution.

Universal rule: a wrapper must always reference its source prompt path and revision, and must never fork or restate project-owned policy that the overlay solely controls.

For chat-only inline wrappers, repo fields may be `not bound` or `not applicable`; do not invent branch, HEAD, path, hash, dirty-state allowance, or validation gates.

### Rerun

Rerun prompts should include only the frozen context plus the unresolved delta. Do not regenerate the whole workflow unless explicitly authorized.

Patch recheck prompts for a prior blocker or major review finding must use a
smallest-complete bounded blast-radius check: first verify whether the patch
closes the original finding, then scan only the touched patch scope for
patch-caused or newly visible blocker/major issues. Newly visible means the
patch exposed or changed evidence inside the touched scope enough that the issue
could invalidate closure or create serious downstream risk. Exclude unrelated
structural review, full-artifact review, minor/nit findings, and pre-existing
issues outside the touched scope unless the user and overlay separately request
a second-pass review lane.

Rerun prompts must name:

- Prior artifact path and hash, revision, or explicit reason the prior artifact is unavailable.
- Frozen decisions that must not change.
- Mutable fields that the rerun may update.
- The unresolved finding, failed gate, or missing evidence being retried.
- For patch rechecks, the touched patch scope and blocker/major-only threshold
  for the smallest-complete bounded blast-radius check.
- The prior output frame unless the user and overlay explicitly authorize a different one.

No rerun prompt may reset scope, reopen settled decisions, or replace the prior contract without explicit authority from the user and active overlay.

### Review

Review prompts are read-only unless the user and overlay explicitly bind edit or patch authority. Findings-first is the universal default: surface findings before formal conclusions. Formal verdicts, readiness claims, review-lane labels, patch queues, report-destination binding, and verdict-vocabulary rules are owned by the active overlay's review doctrine; defer to it. If a durable review prompt lacks output-mode or path binding, return `BLOCKED_OUTPUT_MODE_MISSING` or `BLOCKED_OUTPUT_DESTINATION_UNBOUND` instead of emitting a prompt that falls back to chat-only review.

Repo-bound review prompts are written for a receiving reviewer that is expected to have direct access to the source-of-truth repo or worktree. They must identify that source directly: absolute worktree path or repo identifier, expected branch and HEAD or commit, dirty-state allowance, target files or artifact roles, and the validation evidence to inspect. The prompt must tell the reviewer to inspect the pinned source in place and must not offer a substitute-source fallback. If the runtime unexpectedly cannot open the named repo or worktree, sees the wrong revision, or sees disallowed dirty state, it returns the nearest existing blocker rather than reviewing a context pack, summary, alternate checkout, or recreated source.

### Patch

Patch prompts describe a bounded fix request. They do not authorize source edits, patch execution, or executable patch queues unless the active overlay and user bind patch or executor authority, target files, write boundary, dirty-state allowance, protected-path rules, and validation expectations. Without that authority, return an advisory patch prompt or block the executable claim with `BLOCKED_UNBOUND_PATCH_AUTHORITY` or `BLOCKED_BY_AUTHORIZATION`.

## Minimal Workflow

1. **Resolve output mode and template kind.** Use `paste-ready-chat` for reusable or cross-recipient prompt requests, without requiring the exact phrase "paste-ready prompt". Use `chat-only` for ordinary inline prompt requests when no stricter mode is requested.
2. **Apply the binding matrix.** Separate warning-only placeholders from strict blockers.
3. **Resolve overlay-owned routing and sequencing.** Bind stage, lane, method order, and next authorized action only from active overlay authority, accepted project artifacts, or explicit user instruction.
4. **Choose template source.** Follow the template discovery order: bound registry, authorized project-local templates, bundled generic fallback with disclosure, then blocked.
5. **Resolve model lane and output frame.** Use only active overlay authority, authorized project template metadata, user instruction, or the selected generic template shape.
6. **Apply adversarial naming.** Ensure adversarial prompt titles and derived filenames or prompt paths carry an explicit adversarial review token.
7. **Fill only sourced fields.** Use user-provided facts, loaded overlay facts, and current workspace evidence. Mark unknown required fields as placeholders or blockers.
8. **Add preflight only when relevant.** Include workspace, revision or hash pins, dirty-state allowance, target files, edit permission, and validation expectations only when the prompt is repo-bound, written, routed, rerun, patch-shaped, or strict.
9. **Run final leakage check.** Remove unsupplied project paths, stage names, review labels, model lanes, validation commands, domain facts, and prompt destinations.
10. **Return the prompt or blocked result.** Write only when destination and permission are bound.

## Return Contract

For small chat-only prompts, return the prompt plus a compact note containing output mode, template kind, template source, and any placeholders or warning-only unbound fields.

Use the full return contract for saved artifacts, disposable drafts, wrappers, handoffs, reruns, patch prompts, formal review prompts, model-routed prompts, and blocked results:

- Loaded overlay and template sources.
- Requested template kind and output mode.
- Template source used: project registry, authorized project-local template, user-supplied inline/path input, or bundled generic fallback.
- Workflow sequence and routing status: bound, warning-only unbound, or blocked.
- Generated prompt or blocked result.
- Placeholder fields that remain project-owned.
- Path and SHA256 only when a file was actually written.
- Validation and leakage-check notes without pass/fail claims unless gates are bound and evidence exists.
- Next authorized step.
- Final model choice footer: model lane and output frame status, bound,
  warning-only unbound, or blocked, placed at the bottom of the entire message.

## Final Leakage And Non-Execution Constraints

- Do not install, deploy, rename, promote, or shadow skills.
- Do not create same-name non-shadow skills.
- Do not edit project files unless the prompt and overlay both grant write permission.
- Do not import another project's paths, process stages, workflow sequence, downstream skill names, model lanes, review labels, validation commands, product facts, or prompt destinations.
- Do not call a prompt execution-ready unless the receiving actor's authority, output mode, artifact destination if any, template kind, preflight, patch/executor authority if needed, and mandatory validation gates are all bound.
- Do not execute downstream planning, review, patching, implementation, validation, reruns, installs, deployments, staging, commits, or pushes.
