---
name: workflow-architecture-planning
description: "Workflow-kernel skill for architecture planning: deliberate target architecture choices, compare cross-cutting options, define core/satellite boundaries, preserve deferred implementation implications, and name the smallest complete next routing object. Use only when explicitly invoking `workflow-architecture-planning`, asking for architecture planning, target architecture planning, architecture decision planning, system architecture, product-method architecture, data spine or cleaning spine style architecture decisions, core/satellite boundaries, architecture option comparison, or architecture planning before feature or implementation planning. Do not use for product planning, feature planning, implementation scoping, source edits, runtime design execution, validation, deployment, installation, package publication, resolver behavior, or readiness claims."
---

# Workflow Architecture Planning

## Purpose

Deliberate on high-level, cross-cutting target architecture before feature
planning or implementation scoping.

Core flow:

```text
architecture question -> option comparison -> target architecture ->
core/satellite boundaries -> deferred implementation implications ->
smallest complete next routing object
```

This skill is planning-only. It compares architectural choices, selects or
defers a target architecture, defines what is core versus satellite, preserves
future implementation implications as non-executable notes, and names the next
routing object when one is warranted.

It does not define product bets, choose feature proof slices, produce
implementation routes, create `STEP-*` plans, edit files, create runtime code,
write durable artifacts, validate, install, deploy, promote, package, stage,
commit, push, claim readiness, or claim resolver/plugin behavior.

## Skill Boundary

`workflow-architecture-planning` supplies reusable workflow-kernel mechanics
only. It is not a project router, product strategy method, feature-planning substitute,
implementation-scoping substitute, architecture artifact store, review lane,
runtime design executor, lifecycle lane, or installed-copy authority.

Local project facts, source hierarchy, architecture artifact roles, artifact
destinations, validation gates, review lanes, output modes, protected paths,
edit permissions, lifecycle authority, and downstream execution contracts remain
overlay-owned.

Do not use installed, user-level, plugin, project-local, or global skill copies
as source authority for this skill.

## Trigger Gate

Use this skill only when the user explicitly asks for:

- `workflow-architecture-planning`;
- architecture planning;
- target architecture planning;
- architecture decision planning;
- system architecture or product-method architecture deliberation;
- architecture option comparison;
- data spine, cleaning spine, evidence spine, workflow spine, or similar
  cross-cutting spine decisions;
- core/satellite boundary planning;
- ADR-style option comparison before feature or implementation planning.

Architecture vocabulary is not sufficient by itself. The request must ask to
decide, compare, frame, or preserve a target architecture; a passing mention of
system architecture, product-method architecture, workflow spine, or similar
terms does not trigger this skill.

Do not trigger for:

- generic planning, next-step selection, or route selection;
- low-level-start detection or missing-upstream-artifact detection;
- product direction, customer proof, buyer clarity, product promise, or
  commercial product bet work;
- feature shape, feature workflow, proof slice, validation-as-learning, or
  feature-planning transfer;
- implementation route construction, source edits, patch queues, migrations,
  generated outputs, tests, or validation execution;
- review findings, formal verdicts, prompt artifacts, postmortems, installs,
  deployments, packaging, publication, resolver behavior, or readiness claims.

When a request is actually product, feature, implementation, prompt, review, or
postmortem work, preserve that adjacent lane's ownership.
If the main task is to decide which planning lane applies, do not run a full
architecture-planning pass. Return only whether architecture planning appears to
be the nearest missing lane, then stop.

## Direct-Entry Source And Context Intake

When invoked directly, do not require a prior repo-context packet. Read only the
smallest complete context needed to understand the architecture question:

- the current user request and visible conversation;
- user-provided architecture question, prior option ledger, decision record,
  product or feature plan, implementation route, review, or artifact;
- local repository instructions when authority or artifact boundaries matter;
- narrow repo-visible source only when it can change the architecture options,
  constraints, source precedence, or strict blockers.

Use zero-config advisory mode when no overlay or equivalent authority is bound.
Label evidence and assumptions. Do not convert source reading, summaries,
context packets, installed copies, prior-thread memory, or generated reports
into strict authority.

If the architecture question is missing or too vague to compare real options,
return `NEEDS_ARCHITECTURE_QUESTION` and ask for the smallest complete missing decision
context. If the request requires product direction, feature proof, or accepted
scope before architecture can be compared, return the nearest architecture
result that names that missing upstream decision instead of inventing it.

## Goal Handoff Intake

When a `goal_handoff` is supplied, use `anchor_goal` to bound the architecture
question and `success_signal` to check whether the target architecture serves
the current workstream outcome. Treat `long_term_goal` as horizon context, not
permission to broaden into platform, runtime, source-map, or implementation
work.

Do not mutate `anchor_goal` or `success_signal` silently. If the architecture
question conflicts with the supplied handoff, surface the conflict before
selecting or rejecting a target architecture. If no `goal_handoff` is supplied,
do not invent one.

## Operating Profiles

Select exactly one profile. Use `standard` by default because architecture
decisions usually shape later work. Use `lean` only when the architecture
question is narrow, the user wants a compact decision, or one local evidence
pass is enough to avoid obvious drift without hiding cross-cutting risk.

Profiles tune depth only. They do not grant edit permission, artifact roles,
validation gates, review authority, lifecycle authority, deployment, install,
promotion, packaging, resolver behavior, or readiness.

### Lean

Lean runs one compact architecture grounding pass. It does not require
subagents. Use a subagent only when the user explicitly authorizes subagents or
delegation.

- **Architecture grounding lane:** check the architecture question, repo-visible
  evidence, option realism, core/satellite clarity, boundary leakage, bloat
  risk, missing evidence, and the smallest complete next routing object.

Lean output should stay compact:

- architecture frame;
- two or three materially different options unless fewer are real;
- option comparison;
- target architecture or reason no target is selected;
- core/satellite boundary;
- deferred implementation implications;
- smallest complete next routing object;
- blockers or `not proven` boundaries.

In lean mode, compare at most two or three options and include only fields that
change the decision. Do not fill the full option ledger unless the user asks for
ADR-level detail or omitting a field would hide a material tradeoff.

Minimum valid lean output:

```text
Evidence:
Decision:
Result:
Why:
Core:
Satellite:
Deferred implications:
Not proven / blockers:
Next:
```

### Standard

Standard requires three advisory architecture perspectives, not implicit
subagents:

- **Directional lane:** make the strongest source-backed case for the most
  promising target architecture.
- **Adversarial lane:** make the strongest case against that target, including
  coupling, future rigidity, boundary leakage, hidden assumptions,
  fake-success paths, and premature implementation gravity.
- **Grounding lane:** keep the target architecture repo-native, source-bounded,
  anti-bloat, reversible where possible, and planning-only; identify what to
  cut or defer.

Standard output should make the decision traceable:

- architecture frame and source-read ledger when source was read;
- questions this architecture decision must answer;
- three to five materially different options unless fewer are real;
- directional, adversarial, and grounding perspectives run locally unless
  subagents are separately authorized;
- target architecture recommendation or explicit non-selection;
- core/satellite, contract, invariant, and interface boundaries;
- deferred implementation implications;
- next routing object;
- bloat-cut queue, risks, and answer-changing evidence.

Advisory perspectives are inputs, not verdicts. The main planner owns
synthesis and must not treat agreement between perspectives as proof.

### Evidence Lane Contract

Evidence lanes are advisory inputs, not verdicts. Subagents are execution
permission, not a profile. Invoking `lean`, `standard`, or this skill does not
authorize subagents by itself.

Delegated perspectives use this runtime-safe contract:

- Perspective count is a semantic requirement; agent type, model, role, and
  reasoning-effort overrides are not.
- For full-history or full-context forks, use the inherited/default agent type,
  model, role, and reasoning effort unless the host explicitly supports that
  override shape.
- Put lane differences in the task prompt: directional, adversarial, or
  grounding perspective; source boundaries; evidence scope; output shape; and
  advisory-only non-verdict boundary.
- Do not combine `fork_context: true` with `agent_type`, `model`,
  `reasoning_effort`, or equivalent override fields unless the host explicitly
  supports that combination.
- If delegation is rejected or unavailable, run the required perspectives
  locally as separately labeled passes and disclose the fallback.

Every run must disclose evidence coverage. Lean disclosure may be one compact
line:

```text
Evidence: local compact pass; no subagents.
```

Use the full evidence disclosure when subagents are requested or launched,
source authority or strict claims matter, or the result needs substantial
standard-mode traceability:

```text
Subagents launched: none | 1 | 3
Evidence mode: local compact pass | local directional/adversarial/grounding passes | delegated lane subagents
Delegation runtime: inherited/default agent type and model | explicit override supported | unavailable/blocked
Reason:
```

Default behavior:

- `lean` without explicit subagent authorization: the main agent runs one
  compact architecture grounding pass locally.
- `standard` without explicit subagent authorization: the main agent runs the
  directional, adversarial, and grounding perspectives locally.
- `standard` with explicit authorization to launch three subagents: launch
  separate directional, adversarial, and grounding subagents when the host
  permits it, using inherited/default agent type and model unless explicit
  override support is bound.

If the user explicitly requests subagents and delegation is unavailable or
blocked by higher-priority host policy, disclose the block and run the required
perspectives locally unless the user explicitly forbids local fallback. Do not
pretend a local evidence pass was an independent delegated return.

Each evidence output, local or delegated, must state:

```text
This is advisory input only. It is not a verdict, not implementation authority,
and not proof of readiness.
```

The main planner owns synthesis. It must not anchor on the first evidence
output, delegate the final architecture result, or treat evidence-lane
agreement as proof.

## Architecture Decision Reasoning

Use this reasoning discipline during framing, option comparison, and synthesis:

- Treat the user's proposed architecture, prior-thread direction, obvious
  implementation path, and nearest existing pattern as candidates, not defaults.
- Reconstruct the actor or operator, triggering context, decision pressure,
  target system or workflow, core invariant, integration boundary, trust
  burden, and failure visibility before selecting an architecture.
- Generate materially different architecture options when real, including
  minimal/manual, contract-first, data-first, workflow-first, trust-first,
  platform-first, no-build, defer, and hybrid shapes.
- Compare options by decision fit, boundary clarity, invariants, future
  feature leverage, implementation reversibility, failure visibility,
  validation burden, maintenance cost, bloat risk, and fake-success risk.
- Select a target architecture only when the evidence and constraints justify
  it. If the architecture question is premature or source context is missing,
  return a non-selection result and name the exact missing input.
- Preserve implementation implications without converting them into
  implementation units, route steps, patch queues, or source-changing verbs.
- State what would change the recommendation.

## Frame

The frame stage must separate:

- stated architecture question;
- inferred architecture question;
- missing or conflicting architecture context;
- target actor, operator, downstream consumer, or system surface;
- current workaround, current architecture, or absent architecture;
- decision pressure and why architecture planning is needed now;
- constraints, non-goals, rejected scope, and forbidden paths;
- source gaps that could change the option set or target architecture.

Create an initial source map from user-stated context and repo-visible evidence.
Mark each material claim as sourced, user-stated, inferred, assumed, missing,
conflicting, stale, or `not proven`.

## Questions This Must Answer

Use project-owned axes when available. Otherwise keep only material questions
from this list:

- What is the core architectural decision?
- What must remain stable across future features?
- What belongs in the core, and what should stay satellite?
- What data, artifact, control-flow, prompt, review, or workflow boundary is
  being created or clarified?
- What invariant would make later feature planning safer?
- What implementation implications matter later but must not be executed now?
- What trust, validation, failure-visibility, or fake-success risk drives the
  architecture?
- What source or owner decision could change the target architecture?
- What bloat should be cut before feature planning or implementation scoping?

## Architecture Option Ledger

Use compact `AO-*` labels when useful. Do not treat `AO-*` labels as execution
identifiers or permission to execute.

In lean mode, do not expand this full ledger by default. Compare at most two or
three options and include only decision-changing fields. The full ledger is for
standard, ADR-level, or user-requested detail.

Each option should state:

- option name;
- target architecture shape;
- core responsibilities;
- satellite responsibilities;
- key contracts, invariants, interfaces, or boundaries;
- data, artifact, control-flow, or workflow implications;
- deferred implementation implications;
- assumptions and source gaps;
- failure modes and fake-success paths;
- bloat introduced or avoided;
- why it may win;
- why it may lose.

## Target Architecture

A target architecture is a planning recommendation, not implementation
authority. It should state:

- architecture result;
- selected or deferred target;
- why this target wins or why no target is selected;
- core/satellite boundaries;
- invariants and contracts that later work must preserve;
- deferred implementation implications;
- what remains intentionally undecided;
- what would change the recommendation;
- smallest complete next routing object.

Select a target architecture only when the minimum selection threshold is met:

- a stable invariant is clear;
- the core/satellite split is clear enough to prevent boundary leakage;
- non-goals are known;
- no unresolved upstream product, feature, or authority blocker could
  materially change the target.

If the threshold is not met, return a no-selection or nearest-missing-decision
result instead of forcing a target.

Do not include code snippets, patch hunks, migration content, test content,
executor-ready tasks, implementation units, `STEP-*` routes, or source-changing
instructions.

## Architecture Results

Return exactly one architecture result:

- `TARGET_RECOMMENDED`: a target architecture is selected with boundaries,
  assumptions, and implications clear enough for owner review or a later
  planning lane.
- `OPTIONS_COMPARED_NO_SELECTION`: viable options are compared, but evidence,
  owner preference, or source context is insufficient to select one.
- `NEEDS_ARCHITECTURE_QUESTION`: the request does not yet state a concrete
  architecture question.
- `NEEDS_PRODUCT_DECISION`: product direction, customer promise, or product
  proof could materially change the architecture.
- `NEEDS_FEATURE_PLANNING`: feature workflow or proof-slice decisions are the
  nearest missing object, and architecture planning would overgeneralize.
- `NEEDS_SOURCE_CONTEXT`: decisive current source, overlay authority, prior
  decision, or artifact role is missing for the requested architecture claim.
- `DEFER_OR_REJECT`: the architecture change is unnecessary, oversized,
  premature, or would add more bloat than value.
- `AUTHORITY_BLOCKED`: the user requests a strict or source-changing outcome
  without the required authority.

These are planning results only. They are not validation, approval, readiness,
implementation-start permission, deployment safety, resolver proof, or plugin
readiness.

## Smallest Complete Next Routing Object

Name the smallest complete next routing object only when it materially helps
continuation. The next object may be an artifact, question, owner decision,
downstream lane, or no-op. Do not default to artifact creation:

- architecture brief;
- ADR slice;
- contract boundary note;
- option ledger;
- feature-planning input;
- implementation-scoping input only after owner acceptance and only when the
  architecture is concrete enough to imply behavior, touch areas, validation
  expectations, and cut scope;
- prompt handoff, when the user asks for a prompt or a downstream actor needs
  a stage-preserving prompt.

Do not create or save an artifact-shaped routing object unless output mode, artifact role,
destination, freshness marker, write authority, and validation/readback
expectations are separately bound.

## Evidence And Assumption Grading

Use overlay-defined evidence vocabulary when available. Otherwise use these
portable labels:

- `sourced fact`: directly backed by loaded local authority.
- `user-stated`: stated by the user in the current task.
- `repo-visible inference`: plausible from loaded repo-visible source but not
  directly stated.
- `reasoned inference`: plausible from sources but not directly stated.
- `assumption`: not yet proven, but usable if clearly marked.
- `source gap`: missing evidence that could affect the architecture.
- `authority blocker`: missing project authority required for the requested
  strict claim.
- `strict-only blocker`: missing authority that blocks strict outcomes but not
  advisory architecture planning.
- `not proven`: an architecture, validation, readiness, implementation,
  deployment, resolver, or plugin claim the loaded evidence does not establish.

## Boundary With Adjacent Methods

Product planning owns customer, buyer, product promise, product proof,
commercial plausibility, and readiness for feature planning.

Architecture planning owns target architecture, option comparison,
core/satellite boundaries, contracts, invariants, and deferred implementation
implications before feature planning or implementation scoping.

Feature planning owns feature shape, workflow mechanics, proof slice,
validation-as-learning, repo fit, and non-executable planning-transfer notes.

Implementation scoping owns non-executing implementation routes after an
accepted and sufficiently concrete plan exists. Architecture planning may name
implementation implications, but it must not create the route.

Prompt orchestration owns saved prompts, wrappers, model lanes, template
routing, and stage-preserving handoff formatting.

Review lanes own formal review authority, verdict vocabulary, review-output
binding, and review findings.

Execution and lifecycle lanes own source-changing implementation, validation
execution, install, deploy, package, promotion, resolver behavior, publication,
and rollback work after separate authorization.

## Output Contract

Return the shortest useful result that preserves the decision. For lean runs,
the minimum valid lean output is enough when it preserves the decision. For
substantial architecture planning, use:

```text
Human Summary:
Decision:
Target Architecture:
Why This Wins:
Core / Satellite Boundary:
Deferred Implementation Implications:
What We Are Not Doing:
Boundary:
Next:

Agent Detail:
Profile / Evidence Mode / Source Mode:
Subagents Launched:
Source-Read Ledger:
Questions This Must Answer:
Architecture Option Comparison:
Architecture Result:
Target Architecture Detail:
Validation / Failure Implications:
Bloat-Cut Queue:
Blockers / Not-Proven Boundaries:
What Would Change The Recommendation:
```

Omit empty sections. Do not expose internal scaffolding when a compact answer is
enough. Include source-read ledger details when source was read and the evidence
coverage affects the recommendation, blockers, or next routing object.

## Constraints

- Do not edit project files while using this skill.
- Do not install, deploy, shadow, promote, package, rename, stage, commit, or
  push skills.
- Do not create runtime code, build systems, plugin metadata, saved artifacts,
  validation records, review verdicts, implementation routes, patch queues, or
  resolver claims.
- Do not import project-specific paths, lifecycle labels, validation commands,
  review labels, domain facts, artifact destinations, protected paths, or local
  downstream policy.
- Do not make this skill a general router, mandatory front door, product
  method, feature-planning mode, implementation-scoping mode, or architecture
  artifact store.
- Do not run this skill as a lane-selection router. If the request is only
  asking what planning lane applies, identify whether architecture planning is
  the nearest missing lane and stop.

## Quality Bar

A valid architecture-planning result must:

- trigger only on explicit architecture-planning intent to decide, compare,
  frame, or preserve a target architecture;
- preserve planning-only boundaries;
- consume a supplied `goal_handoff` as architecture context without replacing
  it or treating it as implementation authority;
- compare materially different architecture options before selecting a target;
- keep `standard` as the default profile while allowing compact `lean` output
  for narrow architecture questions;
- keep lean comparison capped to decision-changing fields unless ADR-level
  detail is requested or materially necessary;
- treat the proposed architecture and obvious implementation path as
  candidates, not defaults;
- identify core/satellite boundaries and key invariants;
- select a target only when a stable invariant, clear core/satellite split,
  known non-goals, and no unresolved upstream blocker justify it;
- preserve future implementation implications without turning them into route
  steps, implementation units, patch queues, or source-changing instructions;
- avoid feature proof-slice and scoping verdict vocabulary;
- avoid product-bet and customer-proof ownership;
- separate advisory architecture reasoning from strict authority, validation,
  readiness, deployment, resolver, and plugin claims;
- keep delegated advisory perspectives separate from agent type/model override
  and use inherited/default delegation unless explicit override support is
  bound;
- name `not proven` boundaries and source gaps when evidence is insufficient;
- choose the smallest complete next routing object instead of creating an
  artifact by default;
- leave the next actor with one architecture result, a clear boundary, and the
  smallest complete next authorized step.
