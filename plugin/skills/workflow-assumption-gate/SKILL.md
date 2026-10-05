---
name: workflow-assumption-gate
description: Thin pre-build assumption-verification gate for accepted directions. Use when explicitly invoking workflow-assumption-gate, checking pre-build assumptions or build-readiness, or asking whether an accepted plan secretly commits to an unverified input, capability, coupling, scaffold, or cross-lane contract. Produces a readiness ledger only; never implements, routes, scopes, locks micro-decisions, writes specs, installs, deploys, stages, commits, pushes, or claims readiness.
---

# Workflow Assumption Gate

## Purpose

Run a thin, hard-fenced check **after a direction is accepted and before any
build begins**. Given an accepted direction, this gate does exactly three things
and then stops:

1. **Surface** the 1-3 load-bearing assumptions the build is about to bake in.
2. **Verify** each one against source, owner, or another lane — *before* the
   build, not during it.
3. **Triage** every prerequisite into `blocker`, `deferrable`, or
   `already-decided`, ordered with gating reads first and split into
   agent-owned versus owner-owned.

The output is a compact **readiness ledger**, not a route, a spec, or a lock.

Its one reason to exist: a build otherwise commits *implicitly* — to inputs,
scaffolding, couplings, cross-lane assumptions, or an approach that cannot
actually deliver its value — and discovers the bad commitment mid-build or after
ship. This gate makes those commitments explicit and checks the load-bearing
ones against source first.

Core question:

```text
Given this accepted direction, which 1-3 load-bearing assumptions would the
build bake in, are they true against source / owner / another lane, and which
prerequisites are real blockers versus already decided or safely deferrable?
```

This gate is advisory and read-first. It accepts the direction as given; it does
not re-decide it. It either clears the build to proceed or returns a precise
blocker.

## Named Failures It Prevents

All observed repeatedly; generalized here. This gate exists to stop them.

1. **Plan assumes a capability that does not exist.** An accepted plan binds to
   a producer or data input that is not actually available, and only a pre-build
   source read catches it. *(Anonymized real case: an accepted deriver
   classified a value by comparing it against a reference the producer never
   persists — the reference was computed in memory and discarded. The build
   would have run on a false premise; a single pre-build read of the producer
   would have caught it.)*
2. **Already-decided things mis-flagged as blockers,** stalling the build —
   items settled in a prior acceptance but still tracked as open residuals.
3. **High-lock-in commitment made implicitly** by coding convenience instead of
   being surfaced and chosen — an input signature, a cross-module coupling, or a
   no-I/O versus I/O input shape.
4. **Scaffolding that structurally cannot exercise the new code path,**
   discovered mid-build — e.g. a test builder that must produce a real on-disk
   artifact for an I/O path but does not.
5. **Cross-lane dependency surfaced too late** — building on a *guessed*
   contract instead of asking the owning lane.
6. **Prerequisite ordering errors** — passing structure or building before the
   gating read, baking a dead design.
7. **Smallest-complete erosion** — over- or under-provisioning prerequisites.

**Unifying root:** commitments — inputs, scaffolding, coupling, cross-lane
assumptions, and *whether the approach can even deliver its value* — get baked
implicitly during a build unless surfaced and verified against source first.

## Activation Notice

When activated, start the response with:

```md
Using `workflow-assumption-gate`.
```

## Source Candidate Boundary

`workflow-assumption-gate` is reusable workflow-kernel source only. It is not a
project router, a planning or scoping substitute, a validation lane, a review
lane, an artifact store, or installed-copy authority.

The active project overlay owns every project fact: source hierarchy, artifact
roles, validation gates, review lanes, output destinations, protected paths,
edit permission, lifecycle authority, and which prior acceptances are binding.
This candidate reads those bindings; it hardcodes none of them.

Do not use installed, user-level, plugin-cache, project-local, or global skill
copies as source authority for this candidate. It is source-only in this
repository until a later explicit deployment turn; it claims no validation,
readiness, resolver behavior, deployment, or promotion.

## Trigger Gate

Use this candidate only when:

- the user explicitly invokes `workflow-assumption-gate`; or
- the user asks to check pre-build assumptions, pre-build readiness, or whether
  an accepted direction secretly commits to an unverified input, capability,
  coupling, scaffolding shape, or cross-lane contract; or
- a build lane routes a pre-build assumption check here before scoping
  or implementation.

Operational test:

```text
Would the build bake in a commitment that, if wrong, forces rework or reveals
the approach cannot deliver — and that no one has checked against source, the
owner, or the owning lane?
```

If yes, this gate earns its keep. If no, fast-exit (see [States](#states)) or do
not run it.

Do not trigger for: choosing or comparing directions; building a route;
stabilizing a behavior contract; locking execution mechanics; general "think
harder" reasoning; or any source-changing, install, deploy, or lifecycle work.
Route those to the owning lane (see [Hard Fences](#hard-fences--non-goals)).

## Hard Fences / Non-Goals

This gate is deliberately thin. Each fence names the adjacent lane and the
failure that lane does *not* prevent.

- **NOT a route, `STEP-*`, touch-point map, source map, or validation matrix.**
  That is `workflow-implementation-scoping`. Scoping *lists* prerequisites and
  blockers while routing an accepted plan, and freezes the accepted direction
  rather than challenging its premises; it does not *verify* the load-bearing
  premises against source as its deliverable or *gate* on the pre-build read.
  This gate produces the verified readiness ledger that scoping then routes.
- **NOT execution decision-locking.** That is `micro-decision-locking`, which
  runs *after* a concrete route exists and locks edit order, wording,
  validation choice, and claim boundaries. This gate runs *before* a route and
  *surfaces* a high-lock-in commitment so it is chosen or routed, not defaulted;
  it does not lock the execution mechanics.
- **NOT next-move selection.** That is `incremental-planning`, which compares
  plausible moves *before* a direction is accepted. This gate runs *after*
  acceptance and does not re-pick the direction.
- **NOT general judgment or re-derivation.** That is `workflow-deep-thinking`.
  This gate does not relitigate the accepted direction; it stress-tests the
  specific premises the build would bake in.
- **NOT a behavior contract.** This gate verifies whether the premises the
  accepted direction *relies on* are real; it writes no required-behavior / non-goals / acceptance-criteria contract. If it
  finds intent missing or contradicted, it routes to spec writing rather than
  inventing it.

## Inputs

- the **accepted direction** to be gated, and its acceptance basis as context
  (the gate does not re-open acceptance; if no direction is accepted, it
  returns `BLOCKED_NO_ACCEPTED_DIRECTION`);
- visible source and context needed to verify the load-bearing assumptions;
- a supplied `goal_handoff` as context only: use `anchor_goal` and
  `success_signal` to judge whether the assumptions and prerequisites still
  serve the workstream outcome; do not mutate them silently.

## Gate Workflow

1. **Surface load-bearing assumptions.** Apply the
   [Load-Bearing Assumption Test](#load-bearing-assumption-test). Name at most
   1-3. If none qualify, fast-exit.
2. **Verify each.** Apply the [Verification Method](#verification-method). Each
   assumption gets a `verify_by` and a verdict. An assumption left merely
   `assumed` is never waved through and never promoted to a verified blocker.
3. **Triage prerequisites.** Apply [Prerequisite Triage](#prerequisite-triage):
   tag each `blocker` / `deferrable` / `already-decided`, order gating reads and
   decisions first, and split agent-owned versus owner-owned.
4. **Return the readiness ledger and one state.** Hand the build lane a verified
   ledger, a fast-exit premise, or a precise blocker.

## Load-Bearing Assumption Test

An assumption is load-bearing only when **both** hold:

- the build would *silently rely* on it (it is not stated and chosen), and
- if it were false, it would *force rework* or *reveal the approach cannot
  deliver its value*.

Cap the set at 1-3. Most low-risk builds have zero — fast-exit those. If more
than ~3 distinct premises look load-bearing, the direction is probably not
stable enough to gate; say so and route back rather than expanding the gate.

Common load-bearing shapes (examples, not a checklist):

- a producer or data input exists and is actually persisted, not computed and
  discarded;
- a capability or approach can deliver the value at all;
- a cross-module coupling or invariant holds;
- an input signature or no-I/O versus I/O input shape;
- scaffolding can structurally exercise the new code path;
- a cross-lane contract is as the build is about to guess it.

## Verification Method

Each surfaced assumption carries a `verify_by`:

- `source_read` — read the producing or consuming source before the build;
- `owner` — ask the owner or user when the premise is a decision, not a fact;
- `cross_lane` — ask the owning lane for the contract instead of guessing it.

Each assumption then carries one verdict:

- `verified_real` — evidence (a source reference, an owner statement, a lane
  answer) confirms it; a blocker derived from it is evidence-backed;
- `verified_false` — evidence refutes it; the build cannot proceed on this
  premise — block or route back;
- `unverifiable_now` — it cannot be checked this turn; do **not** wave it
  through and do **not** silently promote it to a blocker without evidence —
  surface it and block.

Reading source improves evidence; it does not grant authority. Owner-owned
decisions and missing authority remain blockers, not agent assumptions.

## Prerequisite Triage

Tag every prerequisite as exactly one:

- `blocker` — must be true or done before the build, and is `verified_real` (or
  `verified_false` and therefore must be fixed first). A blocker is never a bare
  assumption.
- `deferrable` — genuinely safe to handle during or after the build without
  baking a dead design. State why deferral is safe.
- `already-decided` — settled by a prior acceptance or ratification; cite where.
  It must **not** be re-flagged as an open blocker (Named Failure 2).

Then:

- **Order** the list so gating reads and gating decisions come first; never
  place a structure-pass or build step ahead of the read that gates it (Named
  Failure 6).
- **Split** each item `agent` (the build lane can resolve it from the route) or
  `owner` (needs an owner decision or another lane). Never convert an
  owner-owned prerequisite into an agent assumption to keep moving.
- **Keep it smallest-complete** — list only prerequisites that change the build.
  Do not over-provision scaffolding or under-provision a required exercise path
  (Named Failures 4, 7). When you bound or drop prerequisites, say so.

## States

Return exactly one state.

- `PROCEED_NO_LOAD_BEARING_ASSUMPTIONS` (fast-exit) — no assumption passes the
  load-bearing test. State the one-line implicit premise the build may rely on,
  and point to the build lane. A fast-exit with no stated premise is invalid.
- `READY_WITH_VERIFIED_LEDGER` — assumptions are surfaced and every `blocker` is
  `verified_real`; hand the ledger to the build lane.
- `BLOCKED_ASSUMPTION_UNVERIFIABLE` — a load-bearing assumption is
  `unverifiable_now`; name it, the verification it needs, and the owner.
- `BLOCKED_DIRECTION_UNSTABLE` — verification shows the direction's intent is
  missing or contradicted; route to spec writing or the owning planning lane
  with the specific gap, rather than inventing intent.
- `BLOCKED_NO_ACCEPTED_DIRECTION` — nothing accepted to gate yet; route to the
  owning planning or acceptance lane.

The gate must be able to *both* fast-exit and block. Forcing a block when a
clean fast-exit is correct, or waving through an `unverifiable_now` assumption,
both fail this skill.

## Readiness Ledger Output Contract

Default output is `chat-output`: a readiness ledger in the current response, not
a saved artifact, source-of-truth record, or validation record. If the user asks
to save it, bind output mode, artifact role, destination or authorized
derivation, and write authority first; missing bindings block with
`BLOCKED_OUTPUT_MODE_MISSING`, `BLOCKED_UNBOUND_ARTIFACT_ROLE`,
`BLOCKED_OUTPUT_DESTINATION_UNBOUND`, or `BLOCKED_BY_AUTHORIZATION`.

Lead with a one-line state and the ledger. Use compact prose plus this block:

```yaml
assumption_gate:
  status: PROCEED_NO_LOAD_BEARING_ASSUMPTIONS | READY_WITH_VERIFIED_LEDGER | BLOCKED_ASSUMPTION_UNVERIFIABLE | BLOCKED_DIRECTION_UNSTABLE | BLOCKED_NO_ACCEPTED_DIRECTION
  applies_to: ""            # the accepted direction being gated
  load_bearing_assumptions:
    - assumption: ""
      why_load_bearing: ""  # forces rework, or reveals the approach cannot deliver
      verify_by: source_read | owner | cross_lane
      verdict: verified_real | verified_false | unverifiable_now
      evidence: ""          # source ref / owner statement / lane answer; empty if unverifiable
  prerequisites:
    - item: ""
      triage: blocker | deferrable | already-decided
      owner: agent | owner
      order: 0              # gating reads / decisions first
      basis: ""             # already-decided: where it was settled; deferrable: why safe
  proceed_premise: ""       # fast-exit only: the one-line premise the build may rely on
  blocked_reason: ""        # blocked states only
  next_authorized_step: ""  # the owning build/spec/planning lane
```

Omit empty lists. For fast-exit, fill `proceed_premise` and leave the assumption
and prerequisite lists empty or minimal. For blocked states, fill
`blocked_reason` and `next_authorized_step`.

## Boundary With Adjacent Methods

- **Incremental planning** selects the direction before acceptance; this gate
  runs after.
- **Spec writing** owns the behavior contract (required behavior, non-goals,
  acceptance criteria); this gate routes to it on `BLOCKED_DIRECTION_UNSTABLE`
  but writes no spec.
- **Implementation scoping** owns the route, source map, `STEP-*` steps, and
  validation matrix; it consumes this gate's verified ledger.
- **Micro-decision locking** locks execution mechanics after a route exists;
  this gate only surfaces high-lock-in commitments for an explicit choice.
- **Deep thinking** owns open-ended judgment; this gate is a fixed pre-build
  checkpoint, not a reasoning session.
- **Prompt orchestration** owns any prompt, wrapper, or handoff; this gate emits
  none.
- **Review lanes** own formal verdicts; this gate makes none.

## Validation Expectations

Validation should be able to fail when the gate:

- waves through an assumption that is only `assumed` (not `verified_real`) as if
  the build may proceed (Named Failure 1);
- blocks or flags when no load-bearing assumption exists and a clean fast-exit
  was correct;
- mis-flags an `already-decided` item as an open `blocker` (Named Failure 2);
- promotes an `unverifiable_now` assumption to a `blocker` without evidence, or
  buries it instead of blocking;
- lets a high-lock-in commitment (input signature, coupling, I/O shape) pass
  implicitly instead of surfacing it for an explicit choice (Named Failure 3);
- omits a scaffolding prerequisite that structurally cannot exercise the new
  path (Named Failure 4);
- guesses a cross-lane contract instead of assigning a `cross_lane`
  verification (Named Failure 5);
- orders a structure-pass or build prerequisite ahead of its gating read
  (Named Failure 6);
- over- or under-provisions prerequisites beyond smallest-complete (Named
  Failure 7);
- produces a `STEP-*` route, locks execution micro-decisions, selects or
  compares next moves, re-derives the accepted direction, or writes a behavior
  contract (fence violations);
- claims readiness, validation, deployment, resolver behavior, or promotion;
  writes files without bound output authority; or imports project-specific
  paths, lifecycle labels, validation commands, or product facts.

Structural validation does not prove runtime trigger or resolver behavior.

## Constraints

- Do not implement, edit source, build routes, lock execution decisions, write
  specs, run review, or run proof while using this skill.
- Do not install, deploy, promote, rename, shadow, stage, commit, or push.
- Do not treat installed skill copies as source authority.
- Do not import another project's paths, lifecycle labels, validation commands,
  review labels, product facts, or downstream skill names.
- Do not convert a missing authority or owner-owned decision into an agent
  assumption; mark it unverifiable or owner-owned and block.
- Do not expand past 1-3 load-bearing assumptions or list prerequisites that do
  not change the build.

## Quality Bar

A valid `workflow-assumption-gate` result must:

- accept the direction as given and not re-decide it;
- surface at most 1-3 load-bearing assumptions, each with why it is load-bearing
  and a `verify_by`;
- carry a verdict per assumption and never wave through an `assumed` one;
- triage every prerequisite `blocker` / `deferrable` / `already-decided`,
  order gating reads first, and split agent-owned versus owner-owned;
- keep every `blocker` evidence-backed and never re-flag an `already-decided`
  item;
- be able to **fast-exit** with a stated premise when nothing is load-bearing,
  and able to **block** when a load-bearing assumption cannot be verified;
- stay smallest-complete — only what changes the build;
- route to the owning lane (spec, planning, scoping) instead of inventing
  intent, a route, a lock, or a contract;
- make no readiness, validation, deployment, resolver, or promotion claim, and
  leave the build lane one precise next authorized step.
