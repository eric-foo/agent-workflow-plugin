---
name: workflow-reorient
description: Workflow-kernel skill for operator-facing state reorientation after prompt couriering, review loops, compaction, parallel handoffs, or long workflow summaries. Use when explicitly invoking `workflow-reorient` or asking for the current state, where we are, what is accepted versus unproven, what is next in a state/status sense, or the final goal, steps, progress percentage, current phase, and next safe move. Chat-only snapshot; does not plan from scratch, write prompts, precompact, review, patch, validate, accept, deploy, install, or claim readiness.
---

# Workflow Reorient

## Purpose

Reconstruct the operator's mental map from visible workflow state.

Core question:

```text
Given the visible summaries, artifacts, and latest workflow state, where are we,
what is accepted, what remains unproven, and what is the next safe move?
```

This skill exists for long courier loops where formal workflow artifacts
may be correct, but the operator no longer has a compact view of the goal,
current phase, accepted work, unproven claims, and next action.

## Boundary

`workflow-reorient` is a chat-only state snapshot lane. It rebuilds a current
operator view; it does not create the next artifact or execute the next lane.

It does not:

- choose strategy from scratch or compare compounding product moves;
- decide the missing upstream artifact unless visible state itself proves the
  operator is below the right decision layer;
- preserve context before compaction or write checkpoint files;
- write prompts, wrappers, rerun prompts, or handoffs;
- perform implementation, patching, review, validation, acceptance, deployment,
  installation, packaging, promotion, or resolver checks;
- create source files, prompt drafts, workflow-run artifacts, governance
  records, plugin metadata, commits, pushes, or installed skill copies; or
- claim readiness, launch, certification, acceptance, validation success,
  review success, or product proof.

Recommend an adjacent workflow only after naming the next safe move in plain
language.

## Trigger Gate

Use this skill when the user asks to regain situational awareness, such as:

- `workflow-reorient`;
- "what's the current state?";
- "where are we?";
- "reorient me";
- "what is accepted and what is still unproven?";
- "what's next?" when the context is state/status after workflow loops;
- "what is the final goal, steps, progress %, current phase, and next move?";
- "after these executor/reviewer summaries, what should happen next?"

Do not use this skill for:

- ordinary repository orientation or context packets: use `workflow-repo-context`;
- incremental product or planning sequencing from scratch: use
  `incremental-planning`;
- manual compaction survival packets: use the installed precompact skill when
  applicable;
- prompt, wrapper, handoff, patch prompt, rerun, or review prompt creation: use
  `workflow-prompt-orchestrator`; or
- code review, artifact review, implementation scoping, postmortem review,
  source edits, or lifecycle actions.

If the phrase "what's next?" is ambiguous, inspect the visible state and the
user's recent workflow context. After summaries, reviews, handoffs,
compaction, or prior workflow artifacts, default to this skill and give a state
snapshot first. If the operator is asking which strategic product, planning, or
build move compounds most, route to incremental planning. If the intent remains
unclear, give the safest visible state snapshot, then recommend the adjacent
lane without executing it.

## Source And Evidence Intake

Use the smallest complete evidence packet that can answer the state question:

- current user request and visible conversation summaries;
- latest executor, reviewer, patch, recheck, or handoff summaries;
- named artifacts, accepted reports, branch reports, review reports, or prompt
  drafts supplied by the user;
- branch, head, dirty state, or changed files only when they materially affect
  the state snapshot; treat them as internal guardrails and omit them from
  output unless they change the next safe move; and
- durable source paths only when needed to resolve ambiguity about acceptance,
  freshness, proof boundaries, or next safe action.

Do not run broad source scans by default. A couriered summary is a claim unless
it is backed by visible source, raw output, or an accepted report path. A report
path is not proof of its contents until read when its contents matter.

Evidence strength follows this ladder:

```text
accepted source or report path read now > raw validation output > source diff or
artifact text > generated review or executor report > couriered summary >
conversation memory
```

Use labels such as `accepted`, `reported complete`, `reviewed`, `patched`,
`rechecked`, `not proven`, `source gap`, `NOT_CLAIMED`, and `strict-only
blocker`. Do not collapse these states into a single success claim.

## Authority And Minimum Evidence

Acceptance authority must be visible. Treat work as `accepted` only when the
operator, a governing acceptance report, or an explicit acceptance gate says it
is accepted.

Review, recheck, test success, executor completion, or report existence may
support progress, but does not imply acceptance, readiness, validation success,
deployment readiness, product proof, or permission to continue.

Before claiming accepted, blocked, rechecked, recommending a next lane as safe,
or giving a completion percentage, read the smallest complete available artifact that
controls that claim: latest accepted report, reviewer report, raw validation
output, source diff, named handoff, or visible acceptance gate.

If that controlling artifact is unavailable or unread, label the claim
`reported`, `not proven`, `source gap`, or `BLOCKED_MISSING_SOURCE`.

## Admin Signal Minimization

Administrative state is background signal, not the point of reorientation.

- Do not include branch names, commit SHAs, dirty-state inventories, staging,
  commit, push, remote, package, install, deploy, or publication details in
  ordinary snapshots.
- Do not recommend stage, commit, push, package, install, deploy, promote, or
  publish as `Next best move` unless the user explicitly asked about that
  lifecycle action or visible state proves it is the only material blocker.
- When an admin detail is material, compress it to one short line under
  `Open risks or decisions`, `Evidence used`, or `Source gaps`.
- Omit admin state by default, but include one compressed admin-risk line when
  branch, dirty state, untracked files, generated artifacts, or install state
  could invalidate the visible workflow claim.
- Prefer workflow-content moves such as accept, patch, review, recheck,
  handoff, compact, or stop over lifecycle/admin moves.
- If lifecycle authority is absent, state `NOT_CLAIMED` only when it prevents
  confusion; do not expand it into a commit/push/deploy checklist.

## Procedure

1. Bind the final goal in human terms. If the visible state does not establish
   it, mark the goal as `not established from visible state`.
2. Identify the known path or steps from accepted plans, routes, user requests,
   or visible handoffs. If no accepted sequence exists, say so instead of
   inventing a plan.
3. Gather only decisive state evidence. Read durable artifacts only when a
   missing detail could change accepted status, unproven status, phase, or next
   move.
4. Classify each material item as accepted, reported complete, reviewed,
   patched, rechecked, blocked, deferred, or not proven.
5. Name the current phase using plain workflow language, such as planning,
   scoping, implementation, review, patch, recheck, handoff, compact/recovery,
   blocked, or stop/acceptance decision.
6. Default completion percentage to `unknown / not safely estimable`. Give a
   number or range only when both the goal and accepted step sequence are
   visible, and label the basis.
7. Preserve excluded-action boundaries when material. Tests, harness success,
   review success, or recheck success do not imply readiness, acceptance,
   launch, deployment, certification, or product proof.
8. Name the next safe move as one action. Examples: accept, patch, review,
   recheck, prepare next prompt, climb upstream, stop, compact, run validation,
   or avoid action because a claim is not proven.
9. Optionally name the downstream skill that fits that move. Do not execute it
   unless separately requested and authorized.

## Progress Percentage

`Completion %` is an operator-facing orientation estimate, not validation,
readiness, acceptance, or product proof.

Default to:

```text
Completion %: unknown / not safely estimable
```

Give a number or range only when both the goal and accepted step sequence are
visible. Never estimate from effort spent, summary confidence, or number of
messages.

Use one of these forms:

```text
Completion %: 60% operator estimate, based on 3 of 5 visible steps being accepted or rechecked.
Completion %: 40-60% range, because the goal is visible but remaining validation scope is not bound.
Completion %: unknown / not safely estimable, because no accepted step sequence is visible.
```

Do not produce false precision. Prefer a range when evidence is uneven. Always
state whether the estimate is based on accepted steps, reported steps, or
operator inference.

## Output Contract

For ordinary use, return a compact human-readable snapshot:

```text
Final goal:
Steps to take:
Completion %:
Current phase:
Latest completed step:
Current artifact / handoff:
Open risks or decisions:
Next best move:
```

The ordinary template intentionally has no branch/status/commit/push fields.
Do not add an admin or lifecycle section unless the user asks for it or the
admin state changes the next safe move.

If 3 or more fields are unknown for the same reason, collapse the missing
evidence under `Source gaps` and keep the snapshot short. Do not restate the
same missing-evidence reason in multiple fields.

Add these fields when they materially prevent confusion:

```text
Reported complete but not accepted:
Reviewed / patched / rechecked:
Recommended downstream skill:
Evidence used:
Source gaps:
```

Keep the output short unless material source conflicts, strict claims, or
handoff risk require expansion. Do not turn an ordinary reorientation response
into a new plan, patch queue, prompt, context packet, or precompact checkpoint.

## Adjacent Routing

- Recommend `workflow-repo-context` when source orientation or lane ownership
  is missing and that prevents even an advisory state snapshot.
- Recommend `incremental-planning` when the next question is which
  product or planning move compounds most, not what the current state is.
- Recommend `workflow-prompt-orchestrator` only when the next safe move is to
  write a prompt, wrapper, rerun prompt, review prompt, patch prompt, or
  handoff.
- Recommend review, implementation scoping, postmortem review, branch
  completion report, validation, or lifecycle lanes only when the state itself
  indicates that lane is the next safe move and authority remains separately
  bound.

## Failure Behavior

- `STATE_TOO_THIN_FOR_REORIENTATION`: visible state does not establish the
  final goal, current artifact, or latest workflow state well enough for an
  honest snapshot. Ask for the smallest complete missing summary, artifact path, or
  latest report.
- `BLOCKED_MISSING_SOURCE`: a dependent strict or actionable claim relies on a
  missing artifact, report, diff, validation output, or prior-thread material.
- `BLOCKED_STALE_OR_UNSTABLE_EVIDENCE`: a strict or actionable claim relies on
  stale, dirty, untracked, generated, installed, historical, summary, or
  prior-thread evidence without allowance.
- `NOT_CLAIMED`: the snapshot deliberately does not claim validation,
  acceptance, readiness, launch, deployment, resolver behavior, or product
  proof.

Advisory reconstruction may still proceed when explicitly labeled. Missing
authority blocks strict claims; it does not require a new strategy plan.

When blocked, still provide any safe advisory snapshot:

```text
Known:
Not established:
Unsafe to claim:
Smallest complete missing input:
Next safe move:
```

## Quality Bar

A valid reorientation snapshot:

- restates the final goal in human terms;
- names the current phase;
- includes steps to take or says no accepted step sequence is visible;
- gives a bounded completion estimate or says it is not safely estimable;
- separates accepted, reported complete, reviewed, patched, rechecked, and not
  proven work;
- treats couriered summaries as claims unless backed by visible source or read
  accepted report paths;
- preserves excluded-action boundaries when material;
- avoids turning tests, harness checks, reviews, or summaries into readiness,
  launch, acceptance, validation success, certification, or product proof;
- names one next safe move; and
- recommends, but does not execute, any downstream workflow unless separately
  requested and authorized.
