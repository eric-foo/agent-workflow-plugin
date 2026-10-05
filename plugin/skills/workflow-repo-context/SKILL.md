---
name: workflow-repo-context
description: Build compact repository context packets, task-local context pack views, and advisory routing recommendations for workflow-kernel use. Optional context lane only; not a dispatcher, resolver, mandatory front door, or authority layer.
---

# Workflow Repo Context

## Purpose

Build a compact repository context packet, task-local context pack view when
triggered, and routing recommendation from the smallest complete repo-visible
source set.

This skill orients downstream work. It labels evidence, records source gaps, names strict-only blockers, and recommends a possible next lane. It does not perform product planning, feature planning, deep thinking, code review, artifact review, implementation scoping, source edits, validation execution, install, deployment, packaging, or resolver behavior.

## Skill Boundary

`workflow-repo-context` is a packaged skill for context packets, task-local
context pack views, and routing recommendations. It is not a central dispatcher,
resolver, mandatory front door, route owner, packaging contract,
plugin-readiness proof, or authority transfer layer.

Direct downstream invocation remains valid without a prior context packet or
task-local context pack view. A downstream skill may use a packet as
orientation, but it still owns its trigger gate, domain judgment, strict
blockers, and final recommendation.

Do not present this skill, its output, or any loaded source as proof of automatic invocation, current plugin front-door behavior, resolver behavior, install status, deployment readiness, packaging behavior, promotion readiness, source-changing approval, validation success, review acceptance, or workflow-run authority.

## Trigger Gate

Use this skill when the user explicitly asks for `workflow-repo-context`,
repository orientation, a context packet, task-local context pack,
context-management setup, source-read ledger setup, or routing advice.

If the user directly invokes another workflow skill, use that skill directly. A
repo-context packet or task-local context pack view is optional support, not a
prerequisite.

## Direct-Entry Reorientation Hook

When this skill is the entry point, reorient from the current repository before
recommending a lane: read local instructions, workspace overview, material
branch/revision/dirty state, user-named targets, upstream material the request
depends on, and any local overlay or equivalent authority needed for the
requested consequence. Repo maps, context packets, task-local context packs,
summaries, and Agent Workflow maintainer docs orient only unless the current
repo adopts them as local authority.

This hook is smaller than a context packet. It does not create a dispatcher,
packet requirement, repo-map requirement, blocker table, plugin-readiness claim,
resolver claim, or replacement for the directly invoked downstream skill.

## Minimal Preflight

Read only the smallest complete repo-visible pack needed to orient:

- active repository instructions, such as `AGENTS.md`;
- workspace overview, such as `README.md`;
- branch, revision, and dirty-state status when available and relevant;
- upstream workflow material relied on by the request, including inline text, source path, freshness marker, or missing/stale status when relevant;
- user-named files, directories, artifacts, or diffs;
- a repo-owned repo map only when it is already present and can reduce broad
  navigation without becoming proof;
- nearby source, tests, docs, config, examples, package-local instructions, or manifests only when they can change the context packet or routing recommendation.

Do not run whole-repo scans by default. Escalate reads only when missing source could materially change routing, blockers, or packet accuracy. Otherwise stop and mark a `source gap`.

Use the shared source-loading ladder for escalation decisions.
Before upgrading the budget, name the missing question, likely source layer, and
how that layer could change routing, blockers, packet accuracy, or proof
boundaries. If the gap is missing strict authority, return the canonical
source-loading blocker instead of reading more source.

## Decision Procedure

1. Read the smallest complete source set.
2. Record a source-read ledger.
3. Choose the context budget.
4. Apply the source-loading ladder before escalating source reads.
5. Label evidence and source gaps.
6. Mark unsupported strict claims as `not proven`, `NOT_CLAIMED`, or a precise `BLOCKED_*` state.
7. Recommend a possible downstream lane and excluded lanes.
8. Stop at the next authorized step.

## Source-Read Ledger

Keep the ledger compact. Each material source should record:

| field | meaning |
| --- | --- |
| `source` | path, role, or user-provided context |
| `why read` | why this source was needed |
| `claim supported` | packet claim, route choice, blocker, or boundary supported |
| `freshness / dirty note` | clean, dirty, untracked, stale, user-stated, generated, installed, summary, or not checked when material |
| `limits` | what the source does not prove |

Dirty, untracked, stale, generated, installed, summary, prior-thread, or context-packet evidence may orient advisory work when named. It does not support strict claims unless the needed authority explicitly permits that evidence class.

## Context Budgets

- `small`: repository instructions, overview, branch or dirty status when material, and named targets are enough.
- `medium`: add target subtree, nearby tests, configs, docs, examples, or manifests when they could change the packet.
- `large`: use capped top-level inventory plus targeted reads around the named target; avoid broad maps. A tiny repo map may be read only as a scale-triggered navigation pointer, not as authority.
- `monorepo`: read root instructions plus package-local instructions, README, manifest, tests, and changed files when the package target is known.

Upgrade the budget only through the source-loading ladder when missing source could change routing, blockers, packet accuracy, or proof boundaries. If the package, target, or upstream material is unclear and more reading would be speculative, ask one concise clarification or mark `source gap`.

Return `FAILED_UNBOUNDED_READ` if broad source loading replaces a targeted source question or is treated as authority.

Repo maps are optional. They are useful only when repo scale or package
ambiguity would otherwise force repeated broad reads, missed source areas, or
wrong-route recommendations. They must not be required before direct downstream
skill invocation, and stale or dirty map evidence must be labeled before
advisory use.

## Strict Claims

This section summarizes the zero-config source-loading contract as it applies to repo-context behavior.

| claim family | evidence needed | advisory fallback | strict blocker |
| --- | --- | --- | --- |
| formal review, approval, acceptance | bound review lane, source authority, verdict vocabulary, collision precedence | findings or routing context only | `BLOCKED_UNBOUND_REVIEW_LANE` or `BLOCKED_AUTHORITY_ORDER_MISSING` |
| validated, passed, required gate result | validation gate semantics and evidence location | likely checks, labeled `not proven` | `BLOCKED_UNBOUND_VALIDATION_GATE` |
| ready, safe to proceed, deployment or implementation readiness | readiness criteria and evidence for that lane | next-step recommendation only | `BLOCKED_UNBOUND_READINESS_GATE` or claim-specific blocker |
| install, deploy, package, promote, resolver, plugin readiness | lifecycle authority, collision checks, rollback boundary, source checks, validation evidence | route elsewhere; no lifecycle claim | `BLOCKED_UNBOUND_LIFECYCLE_AUTHORITY` |
| source-changing work, patch queues, executor-ready handoff | bounded edit or patch authority, stable targets, protected-path rules, verification expectations | non-executing route or advisory scope only | `BLOCKED_BY_AUTHORIZATION` or `BLOCKED_UNBOUND_PATCH_AUTHORITY` |
| saved artifact or source-of-truth output | output mode, artifact role, destination, freshness marker, write authority, paired-artifact rules | chat-only packet | `BLOCKED_OUTPUT_MODE_MISSING`, `BLOCKED_UNBOUND_ARTIFACT_ROLE`, or `BLOCKED_OUTPUT_DESTINATION_UNBOUND` |
| authority order or canonical policy | source-precedence authority | unresolved conflict note | `BLOCKED_AUTHORITY_ORDER_MISSING` |
| strict claim from dirty, stale, installed, generated, summary, prior-thread, or packet evidence | explicit allowance for that evidence class | `not proven` boundary | `BLOCKED_STALE_OR_UNSTABLE_EVIDENCE` or the more precise blocker |

Reading more source can improve advisory accuracy. It cannot create strict authority.

## Packet Contract

Return the shortest useful packet that includes:

- objective or user request;
- active workspace or repository identifier;
- branch, revision, and dirty or untracked status when available and material;
- source-loading mode: `zero-config advisory` or `strict-required`;
- context budget tier: `small`, `medium`, `large`, or `monorepo`;
- source-read ledger or compact evidence line;
- upstream workflow material relied on by the request, with inline material, source-visible path, freshness status, or missing/stale note when relevant;
- target files, directories, artifacts, diffs, or package scope;
- evidence labels, source gaps, and `not proven` boundaries;
- strict-only blockers and `NOT_CLAIMED` strict results when material;
- missing, stale, dirty, large, or contradictory context notes;
- optional prose event observations, only when useful and clearly non-authoritative;
- routing recommendation, excluded routes, and repo-dependent lane availability marked unknown unless source-proven;
- next authorized step.

A valid packet keeps loading narrow, preserves direct downstream invocation, avoids lifecycle/readiness/resolver/plugin claims, and leaves a clear next step.

## Task-Local Context Pack View

For meaningful tasks, the packet may include a task-local context pack view: a
current-task working-memory section that compresses navigation and rationale
without compressing authority. Use it when a source-read ledger alone would be
too thin for implementation, review, planning, or handoff because the task spans
files, contracts, schemas, tests, callers, callees, lanes, compaction risk,
dirty/stale/untracked/generated/source-visible-but-unauthorized evidence, or
open questions that must survive later steps.

Do not require this view for trivial tasks, single-slice reads, or compact
answers where the ledger already names the source and limits. Keep the view
sparse and omit empty fields:

- Relevant symbols:
- Relevant files:
- Contracts/types/schemas:
- Tests/examples/fixtures:
- Callers:
- Callees:
- Implementation slices:
- Loaded source:
- Not loaded / why:
- Open questions:
- Freshness and weak-evidence limits:
- Next authorized step:

The task-local context pack view is chat-oriented by default. Do not save it,
turn it into a repo map or thin wiki, place it under `.workflow-runs/`, or treat
it as a precompact checkpoint, input packet, proof/readback, review result,
validation evidence, approval, readiness state, deployment state, or
source-of-truth artifact unless separate authority explicitly binds that
destination and role.

Context packets are `chat-output` by default. A bulky orientation packet may use
`temporary-output` only when a local disposable-state convention permits it and
the response keeps the non-durable boundary visible. A durable context artifact
requires explicit output mode, artifact role, destination, write authority, and
the durable binding fields from the Agent Workflow durable artifact output
binding contract, including protected-path rules, freshness or dirty-state
expectations when source state matters, mutability, validation/readback/hash
expectations, retention/cleanup/promotion rules, compact courier fields, and
write-failure behavior; otherwise it remains chat-only or blocks with the
precise missing binding field.

Downstream lanes may use the view for orientation only. They still own direct
invocation, trigger gates, source-loading escalation, decisive rereads,
proof/readback, blockers, and final lane judgment. In arbitrary consumer
repositories, reflect that repository's local instructions, overlays, package
boundaries, and source layout; do not import Agent Workflow's internal paths or
fixture names as local authority.

## Routing Discriminators

Recommend routes only as advisory possibilities. Repo-dependent lane names, availability, contracts, and authority are unknown unless current repo source proves them.

| lane | choose when | do not choose when |
| --- | --- | --- |
| product planning | the missing decision is customer, buyer, promise, product shape, or product proof | the user asks for source orientation, implementation steps, review, or artifact critique |
| feature planning | the missing decision is feature shape, proof slice, option comparison, or advisory implementation-unit planning before source changes | the feature plan is already accepted and concrete enough for implementation scoping |
| deep thinking | the task needs option comparison, decision criteria, uncertainty handling, or failure-mode analysis | the user asks for repo orientation, formal review, implementation route, or artifact acceptance |
| implementation scoping | an accepted plan needs a read-only implementation route before edits | the plan is unaccepted, too abstract, or the user only wants context |
| code review | the user asks for implementation/code review against source or diff evidence | the target is a non-code artifact, product decision, or implementation plan |
| adversarial artifact review | the target is a non-code artifact needing critique against cited source | the user asks for code review, prompt drafting, or implementation |
| prompt orchestration | the user asks for a prompt, wrapper, handoff, rerun prompt, or prompt artifact | the user asks for repo orientation only; do not emit prompts unless requested or authorized |
| postmortem review | completed work or claimed results need integrity review | the work has not happened or the user only needs pre-work context |

For patch execution, deployment, packaging, installed-copy review, source-changing work, or lifecycle behavior, route elsewhere and do not invent local policy.

## Persistence And Event Boundary

Repo context packets and task-local context pack views are chat-oriented by
default. Do not create, update, or require `.workflow-runs/<run-id>/` artifacts
for ordinary orientation, routing recommendations, source-read ledgers, or
short-lived status.

If the user asks to persist context under `.workflow-runs/<run-id>/`, require explicit artifact root, run id, allowed files, mutable and immutable artifact roles, validation gates, retention and cleanup expectations, and write boundaries. Missing fields block with existing source-loading blockers.

Repo context may include short prose observations of material workflow state. Do not create event schemas, telemetry, log stores, dashboards, event sinks, adapters, runtime helpers, automation, event-specific blockers, or platform behavior.

## Failure Behavior

- Missing target source: ask one concise clarification or return `source gap`.
- Missing upstream workflow material: block dependent strict or actionable claims with `BLOCKED_MISSING_SOURCE`, `BLOCKED_STALE_OR_UNSTABLE_EVIDENCE`, or a more precise claim-specific blocker; advisory reconstruction must be explicitly requested and labeled non-authoritative.
- Dirty or untracked evidence: name it in the ledger; use it for advisory orientation only unless dirty-state allowance is bound.
- Stale revision: warn in advisory mode; block strict claims when freshness matters.
- Contradictory sources: use bound authority order if present; otherwise report the conflict and avoid strict claims.
- Non-code artifact closeout: route elsewhere when appropriate; do not claim acceptance, approval, validation success, readiness, or source approval.

## Examples

Advisory packet:

```text
mode: zero-config advisory
budget: small
ledger:
- source: AGENTS.md
  why read: active repo rules
  claim supported: no install/deploy/source-root mutation
  freshness / dirty note: repo-visible
  limits: does not prove downstream lane availability
recommendation: implementation scoping may be useful if the plan is accepted
not proven: validation, readiness, resolver behavior
next authorized step: ask for implementation-scoping route or provide target files
```

Strict request blocked:

```text
request: confirm plugin-ready deployment
loaded evidence: repo source and package notes
strict claim: plugin readiness / resolver behavior
blocker: BLOCKED_UNBOUND_LIFECYCLE_AUTHORITY
advisory fallback: source can be summarized, but plugin readiness is NOT_CLAIMED
next authorized step: bind lifecycle authority, resolver checks, and validation evidence
```

## Constraints

- Do not edit files, execute validation gates, install, deploy, promote, package, rename, shadow skills, stage, commit, push, or mutate external systems.
- Do not create plugin metadata, runtime code, build systems, deployment records, installed copies, resolver claims, event infrastructure, or workflow-run artifacts.
- Do not import another project's source hierarchy, packet roots, review lanes, validation commands, lifecycle stage names, protected paths, domain policy, output destinations, or artifact categories.
- Do not treat installed, user-level, plugin, project-local, or global skill roots as canonical source for this repository.
- Do not make direct invocation of downstream skills depend on this skill.
