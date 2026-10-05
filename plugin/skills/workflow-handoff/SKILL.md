---
name: workflow-handoff
description: Source-only workflow-kernel candidate for cold cross-lane handoff packets. Use when explicitly invoking `workflow-handoff`, asking to hand off or transfer in-progress work to a fresh lane, different agent, new thread, or separate worktree that has none of the sender's context, or authoring or validating this source candidate. Built on the precompact packet skeleton but specialized for a cold reader — durable destination by default, fresh-reader self-containment, front-loaded goal and open-decision and drift-guard, and a confirm-don't-trust load contract. Do not use for same-thread resume after `/compact`, for writing the courier prompt or wrapper that routes a receiver to a handoff (that is `workflow-prompt-orchestrator`), or for deployment, install, resolver, or readiness claims.
---

# Workflow Handoff

## Purpose

Create a durable, self-contained handoff packet that transfers in-progress work to a fresh lane, agent, thread, or worktree that holds none of the sender's context.

The goal is the minimum complete state a cold reader can independently re-verify and continue from: maximum recoverability, not maximum length. A receiver can rely on the handoff because the contract forces re-verification, not because the sender is trusted; the packet makes the sender's load-bearing claims checkable, not authoritative.

This candidate supplies reusable cold-handoff mechanics only.

## Scope

Use it when the user explicitly asks for `workflow-handoff`; to hand off or transfer in-progress work to a fresh lane, a different agent, a new thread, a separate worktree, or anyone who lacks the sender's context; for a durable handoff packet a cold reader can continue from; or for authoring or validating this candidate.

It reuses the precompact working-packet skeleton: the working-packet contract, the load-bearing-only compression and continuity pass, the source-read ledger with compare targets or `reread-required`, the `Superseded / Dangerous-To-Reuse Context` section, and the recovery outcomes (`REUSE`, `PARTIAL_REUSE`, `STALE_REREAD_REQUIRED`, `BLOCKED_DRIFT`, `BLOCKED_MISSING_PACKET`). The boundary is exact: **precompact = the same agent resuming the same thread after `/compact`** (warm, disposable); **handoff = transfer to a fresh lane with none of the sender's context** (cold, durable). That difference is this skill's reason to exist. It does not impose precompact's `/compact` stop ritual and is not a substitute for same-thread resume.

It does not own:

- the courier prompt, wrapper, or routing instruction that tells the receiver what to do; that is `workflow-prompt-orchestrator` (`handoff` template kind), which points at the packet this skill produces;
- general repository orientation, source loading, routing, or context-packet setup; a generic summary; or project-owned routing and sequencing;
- semantic readiness, review verdicts, deployment readiness, or validation gates; the packet alone is a continuation artifact, never governance, validation, acceptance, or readiness evidence;
- mechanical fact generation beyond invoking or recording bound preflight evidence;
- installed-copy, resolver, plugin, or automatic-hook behavior. Do not install, deploy, promote, package, rename, or shadow skills, edit installed, user-level, plugin, global, or project-local skill roots, or create runtime code, implementation directories, build systems, plugin metadata, commits, remotes, or pushes, unless a later turn explicitly authorizes it. Such a request without authority returns `BLOCKED_UNAUTHORIZED_DEPLOYMENT_OR_INSTALL`.

The packet carries source-loading state and earlier-decided context, but it does not execute source loading or instruct the receiver how to load: that instruction follows overlay source-loading doctrine and is delivered by the courier prompt; the receiver executes it.

Never import another project's paths, lifecycle labels, validation commands, review labels, product facts, or storage conventions.

## Sender Intake

Building a handoff does not require a prior repo-context packet. The sender refreshes current state directly: local instructions, workspace, branch/head, dirty or untracked state, target files, source-read ledger entries with compare targets, blockers, validation status, and latest user constraints. Use targeted refresh, not broad source reading.

A supplied repo-context packet, context pack, repo map, summary, or prior thread note is orientation only. It does not replace live-state refresh, durable-destination authority, the self-containment pass, or the load contract. Reread decisive current source or bound authority before strict recovery, readiness, validation, or saved-artifact claims.

Report exactly one source-loading mode:

- `provided-file-only`: only user-provided files, packet text, or inline context were loaded; live workspace facts are not claimed unless directly supplied.
- `repo-overlay-bound`: a repo overlay or equivalent source contract binds source precedence, durable-output authority, and required reread evidence.
- `live-workspace-preflight`: no overlay binds the source contract, but the workspace can be read for live state, mechanical preflight, and targeted source refresh under local instructions.
- `blocked`: required source, authority, durable destination, or live-state evidence cannot be loaded safely; return the precise blocker before writing or loading a packet.

## Output Modes

`max` is the default. Use another mode only when:

- the user explicitly requests `lean`, `minimal`, or equivalent;
- durable-output authority or destination is missing and the result must block before writing; or
- `max` cannot be produced because required live state is unavailable, and a labeled fallback is explicitly safer than pretending full transferability.

Label `lean` as explicit opt-in or fallback; it does not claim full cold-reader transferability or self-containment.

A fallback `lean` packet must include `mode: lean`, the reason `max` was not produced, `missing_sections`, a warning that full cold-reader transferability and self-containment are not claimed, and the exact next action or blocker. Never silently downgrade `max` to save tokens or latency.

## What Truly Matters

Preserve information by priority.

P0 must stay: the front-loaded `goal_handoff` (or `not supplied`); the front-loaded open decision or fork with options and trade-offs; the front-loaded drift guard; the front-loaded inherited context; active objective; exact next authorized action; blockers; workspace, branch, head, and exact dirty or untracked state; changed files and target artifacts; validation results and known failures; user constraints and explicit preferences.

P1 should stay: frozen decisions; mutable questions (kept separate from frozen decisions); the source-read ledger with compare targets; stale, superseded, or dangerous-to-reuse context; important risks and assumptions.

P2 stays only when it changes the next action: command excerpts, branch or worktree references, prior review findings, artifact paths, rationale excerpts.

Drop: chat history and old reasoning that no longer controls the next action; dead-end attempts unless they explain a blocker; full command logs by default; broad background and polite conversation; sender-only shorthand and implicit referents.

## Cold-Reader Self-Containment

The packet must stand alone. Resolve every referent inline: replace "as discussed", "the earlier decision", "that file", "the prompt", "the usual way", or any pronoun whose antecedent lives only in the sender's memory with the concrete name, path, identifier, ref, or quoted text. Do not rely on prior conversation or unstated conventions; state each load-bearing claim with its evidence or the exact place to re-derive it; define acronyms, lane names, and local labels on first use; name files, artifacts, branches, and commands specifically enough to locate without guessing.

Before output, scan for dangling referents and resolve each inline or record it as a blocker. A handoff that depends on a referent the cold reader cannot resolve is not complete.

## Durable Destination

Handoff packets are durable filesystem output by default, not precompact's disposable output: the receiver needs a stable path, and the sender's thread may end.

Before writing, bind: the target workspace or repository; output mode `filesystem-output` (durable) by default, downgraded to a disposable destination only on explicit user instruction and labeled as such; the durable destination or storage rule; write permission for the handoff path; the dirty-state policy, including the handoff file's own dirty or untracked status; the `goal_handoff`, open decision, and drift guard; and relevant targets or ledger entries with compare targets when known.

Use overlay-bound storage rules when available. Otherwise write only to an explicit user-provided durable path inside the target workspace, described as a persisted handoff artifact. A dated handoff document is the natural shape, but the destination, slug, and path come from overlay authority or the user; never invent a storage convention.

- No safe output mode or destination → `BLOCKED_OUTPUT_MODE_MISSING` or `BLOCKED_OUTPUT_DESTINATION_UNBOUND`, naming the exact missing binding, then ask for the handoff folder or path.
- No write permission → `BLOCKED_ROLE_PERMISSION` or the nearest overlay-owned equivalent.
- A request to treat the packet as durable proof, validation, acceptance, governance, or source-of-truth output → require that durable-artifact or workflow-run authority.

## Goal Handoff, Open Decision, And Drift Guard

These blocks appear immediately after the load-contract identity block and before the detailed state, so the receiver weighs the decision and absorbs the guardrails before skimming the rest.

`goal_handoff` uses the schema `workflow-prompt-orchestrator` couriers: `long_term_goal` (the durable goal beyond this workstream), `anchor_goal` (the receiver's current optimization target), `success_signal` (the receiver's output-fit check). When a supplied `goal_handoff` is visible, copy each field **verbatim**: no paraphrase, normalization, re-derivation, or silent edit. It is orientation, not authority; carrying it forward implies no source authority, validation, readiness, approval, or edit permission. If none is supplied or safely derivable from bound context, record `goal_handoff: not supplied` and warn that `success_signal`-based output-fit checking is unavailable; never invent a goal.

`open_decision` names the unresolved decision or fork: the options, what is already constrained or off the table, the trade-offs, who owns the call, and the sender's recommendation and rationale when it has one. Record `None open` only after live-state refresh confirms it.

`drift_guard` names the guardrails the receiver must not violate: scope boundaries and non-goals, context or artifacts that look reusable but are not, and owner constraints the receiver could skim past.

## Inherited Context (Does Not Flow To A New Lane)

A fresh lane inherits neither the sender's source-loading setup nor its earlier-decided concepts. This block is front-loaded with the goal, decision, and drift guard. Carried items are orientation, not authority.

**Source-loading state to re-establish (pointer-style, following overlay doctrine).** Never carry a loading scheme or hardcode what to read. Carry: a pointer to the overlay's source-loading policy, or `zero-config advisory` when none binds one; claim-relevant targets (named files, symbols, artifacts) for entering the source-loading ladder early; what was already loaded, recorded as a weak source class (context-packet, summary, or prior-thread evidence), freshness-marked; and what must be loaded first before any strict or actionable step. The loaded-set only seeds the ladder; it does not satisfy it.

**Earlier-decided concepts and behaviors (inline gist plus verify pointer).** For each upstream decision that shaped the work but would not travel with a file list (problem or goal framing, operating profile, conventions, prior architecture or method decisions), carry a one-line inline summary as Tier-1 orientation, so the receiver re-anchors without re-litigating settled calls, and a fresh source-visible pointer with a compare target, so the receiver verifies it as Tier-2 authority before strict or actionable use.

## Max Handoff Packet Contract

Produce this structure for `max` packets. Keep it dense, factual, and cold-reader-resolvable.

```md
# Handoff Packet

## Load Contract

- packet_version:
- mode: max
- created_at:
- created_by_lane: sender lane or agent identifier, for provenance only; not an authority claim
- workspace:
- handoff_path:
- expected_branch:
- expected_head:
- expected_dirty_state_including_handoff_file:
- load_rule: confirm-don't-trust; re-verify every load-bearing fact against its compare target before acting; sender claims are hypotheses, not authority

## Goal Handoff

- long_term_goal:
- anchor_goal:
- success_signal:

(or `goal_handoff: not supplied` with the output-fit warning)

## Open Decision / Fork

- decision:
  - options:
  - already constrained / off the table:
  - trade-offs:
  - owner of the call:
  - recommendation and why (if any):

(or `None open`)

## Drift Guard

- invariant, non-goal, or scope boundary:
  - why it matters:
  - what violating it would break:

## Inherited Context (does NOT flow to a new lane)

### Source-loading state to re-establish (follows overlay doctrine)

- overlay source-loading policy: <pointer> or `zero-config advisory`
- targets to enter the ladder: <named files, symbols, artifacts>
- already loaded (weak orientation, freshness-marked; not authority):
- must load first (before strict or actionable steps):
- load rule: receiver re-runs progressive source loading per overlay; the packet's loaded-set only seeds the ladder

### Earlier-decided concepts and behaviors (inline gist plus verify pointer)

- decision, framing, profile, or convention: <one-line summary>
  - decided in: <source path or artifact>
  - compare target: hash, mtime/size, HEAD/ref, or quoted excerpt
  - verify before: strict or actionable use

(or `None known` after live-state refresh confirms it)

## Active Objective

One or two cold-reader-resolvable sentences.

## Exact Next Authorized Action

1. Exact next action, naming its own targets and paths.
2. Follow-up action.
3. Validation or stop condition.

## Authority And Source Ledger

- Repository instructions:
- Overlay or equivalent authority:
- User constraints:
- Source-read ledger:
  - `path-or-source`
    - Role:
    - Load-bearing: yes | no
    - Compare target: hash, mtime/size, HEAD/ref, generated artifact path, quoted excerpt, or `reread-required`
    - Last checked:
    - Reuse rule:
- Source gaps:
- Strict-only blockers:
- Not-proven boundaries:

## Current Task State

- Completed:
- Partially completed:
- Broken or uncertain:

## Workspace State

- Branch:
- Head:
- Dirty or untracked state before handoff:
- Dirty or untracked state after writing the handoff file:
- Target files or artifacts:
- Related worktrees or branches:

## Changed / Inspected / Tested Files

- `path/to/file`
  - Status:
  - Role:
  - Important observations:
  - Symbols or sections:

## Frozen Decisions

- Decision:
  - Evidence:
  - Consequence:

## Mutable Questions

- Question:
  - Why still mutable:
  - What would resolve it:

## Superseded / Dangerous-To-Reuse Context

- Stale instruction, idea, artifact, or finding:
  - Why stale or dangerous:
  - Current replacement:

## Commands And Verification Evidence

- Command:
  ```bash
  command here
  ```
  Result:
  - Passed/failed/not run:
  - Important output:
  - Re-run target so the receiver can confirm rather than trust:

## Blockers And Risks

- Blocker or risk:
  - Evidence:
  - Likely next action:

## Confirm-Don't-Trust Load Checklist

- Load-bearing facts the receiver must re-verify before acting:
- Compare target for each:
- Load outcomes and what each means:
- Sources that must be reread if drift is detected:

## Do Not Forget

- Critical non-duplicated reminder:
```

Mark each ledger entry and load-bearing fact `Load-bearing: yes` or `no`; load-bearing facts gate the next action. Every load-bearing fact must carry a concrete compare target the receiver can check or an explicit `reread-required` marker. Every material ledger entry needs `Role`, `Load-bearing`, `Last checked`, `Reuse rule`, and either a concrete compare target (hash, modified time and size, HEAD/ref, generated/artifact path, or quoted excerpt) or an explicit `reread-required` marker. A load-bearing claim with no compare target and no way to re-derive it is not acceptable.

Required sections may contain `None known` or `None open` only after live-state refresh confirms there is no material entry. Never leave placeholders empty or invent content to satisfy the template.

## Packet Compression And Continuity Pass

Before finalizing, run a load-bearing-only pass: preserve every fact needed to continue safely, but remove or merge anything that does not change recovery, authority, validation, the open decision, the drift guard, or the exact next action.

Give each fact one authoritative home: load identity in `Load Contract`; live repository state in `Workspace State`; source authority and compare targets in `Authority And Source Ledger`; implementation deltas in `Changed / Inspected / Tested Files`; stale or rejected context only in `Superseded / Dangerous-To-Reuse Context`; reminders in `Do Not Forget` only when not captured elsewhere.

Delete duplicated facts outside required contract fields, superseded plans or findings once their replacement is recorded, command output that does not affect the next action, rationale that no longer controls continuation, and reminders that repeat P0/P1 entries. Move a dangerous-but-stale fact to `Superseded / Dangerous-To-Reuse Context` with its current replacement rather than keeping both active. If multiple prior packets or notes exist, record only the latest usable continuation anchor and mark older ones superseded.

## Send Ritual

1. Refresh live state: workspace, branch, head, dirty or untracked status, target files, source-read ledger with compare targets, blockers, validation status, and latest user constraints.
2. Produce a `max` packet unless the lean or blocked/fallback rules apply.
3. Run the self-containment pass and the compression and continuity pass.
4. Write the packet only to a bound durable destination.
5. Record the handoff file itself in dirty or untracked state when applicable.
6. Reply with a short message that includes exactly one fenced handoff courier
   block at the bottom. Select its opening from the packet's explicitly bound
   next act; the emitted courier carries the instruction, so do not ask the
   operator to type a separate trigger:
   - implementation-authorized handoff: `Success implement the commissioned
     work in:`;
   - planning-only handoff: `Plan the commissioned work in the packet with
     success implement. Do not implement.`;
   - review, diagnosis, read-only, or otherwise non-implementation handoff:
     `Get back context from:`.
   This selection does not grant implementation authority or turn planning into
   execution. Emit the selected opening in place of `<selected_opening>`:
   ```
   <selected_opening>
   <handoff_path>

   Follow the packet's confirm-don't-trust load contract. If you have repo/filesystem access, open the packet and re-read its named load-bearing sources before making strict or actionable claims. If you do not have repo/filesystem access, stop and request a pasted source capsule or no-repo handoff.

   Continue only the lane named in the packet's Goal Handoff / Active Objective. Do not perform work excluded by the packet's Drift Guard unless explicitly redirected by the current user.
   ```
7. Do not assume the sender silently continues as the source of truth. If the sender keeps working after writing the packet, it is stale for the receiver; refresh it, or rely on the receiver's confirm-don't-trust pass to catch the drift.

## Confirm-Don't-Trust Load Protocol

A cold reader must not act on the sender's authority. The whole packet is a weak source class (context-packet, summary, or prior-thread artifact): it orients but does not bind strict claims, and the receiver rebinds fresh source for any strict or actionable claim per overlay source-loading doctrine.

On load, treat every load-bearing claim as a hypothesis and compare the packet with current live state before acting:

- workspace, branch, head, exact dirty or untracked state;
- target files or artifacts;
- source-read ledger entries against their recorded compare targets;
- inherited source-loading state, by re-running progressive source loading per the overlay (or zero-config advisory when none is bound) rather than trusting the packet's loaded-set;
- validation evidence the next action depends on, by re-running rather than trusting;
- handoff file path and readability.

Ledger drift includes a missing or unreadable path, a changed content hash, modified time, or size where those were recorded, a changed HEAD/ref for HEAD-bound source, changed dirty or untracked status for working-tree source, missing generated or artifact source, or missing reread evidence for an entry the next action depends on. A material load-bearing entry with no compare target must be re-derived before reuse; if it cannot be, do not proceed on sender say-so.

Return exactly one load outcome:

- `REUSE`: all required load-bearing facts re-verified against their compare targets; continue from the exact next authorized action. For a cold reader, `REUSE` is earned by verification, never granted by trust.
- `PARTIAL_REUSE`: only optional or non-load-bearing facts drifted or are unverifiable; reuse verified sections and re-derive the rest.
- `STALE_REREAD_REQUIRED`: material load-bearing source, head, dirty state, target files, or ledger evidence drifted or lacks a compare target but can be re-derived safely before work.
- `BLOCKED_DRIFT`: drift conflicts with authority, user constraints, target path, dirty-state policy, or unknown edits.
- `BLOCKED_MISSING_PACKET`: handoff path is absent, unreadable, or not provided.
- `BLOCKED_UNVERIFIABLE`: a load-bearing claim has no compare target and cannot be re-derived from available sources; the receiver must not continue on sender say-so. When the cause is missing or stale upstream or source material, including inherited material a strict or actionable claim depends on that is missing, stale, or summarized without fresh source, report the precise source-loading blocker (`BLOCKED_MISSING_SOURCE` or `BLOCKED_STALE_OR_UNSTABLE_EVIDENCE`); `BLOCKED_UNVERIFIABLE` is the cold-lane umbrella for that family.

Never silently continue from an unverified or stale packet.

## Mechanical Evidence

Use deterministic preflight evidence when available, but keep its boundary narrow: it may support branch and head, exact dirty state, required paths, hashes, and scoped diff facts. It does not prove semantic readiness, implementation correctness, review success, deployment readiness, or workflow success. Never claim a validation pass without run evidence.

## Output Contract

Return:

- selected mode: `max` or `lean`;
- source-loading mode and authority status;
- durable destination status;
- the exact fenced handoff courier block when written, which is the only place the chat response gives the packet path;
- the blocked result when required authority or destination is missing;
- the load outcome when loading from a packet.
