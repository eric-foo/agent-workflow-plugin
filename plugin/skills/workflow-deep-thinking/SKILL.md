---
name: workflow-deep-thinking
description: Workflow-kernel skill for deeper reasoning. Use when the user explicitly invokes `workflow-deep-thinking`, asks to `deep think`, `deep-think`, asks for careful reasoning about workflow-kernel source behavior, or when a migration/deployment validation prompt names this skill.
---

# Workflow Deep Thinking

Give a better answer than a quick reply would: the right question, checked
facts, the options that matter, and a clear answer or recommendation. This
skill shapes reasoning and the answer only. Higher-priority instructions, the
user's formatting requests and local project rules take precedence.

On the turn that invokes the skill, start the response with this line unless a
higher-priority instruction requires a different format. Do not repeat it on
later turns unless deep thinking is asked for again.

```md
Using `workflow-deep-thinking`.
```

## Before answering

Use the checks that can change the answer; they are not a required sequence.

1. **Pin down the question.** What does the person need decided or fixed, and
   under what conditions? Separate what they observed from their explanation
   and their proposed fix. Do not quietly answer a broader or different
   question.
2. **Check the facts the answer rests on.** Identify the facts that would
   change the answer if they were wrong. Check each against its owning file,
   live system or primary source, not a summary, overview, earlier thread note
   or memory. Supplied primary evidence can suffice when its currentness is
   established. If decisive evidence is unavailable, qualify the answer.
3. **Check what already exists.** Use available context and narrow source
   checks to see what is already built, decided or ruled out. Build on it;
   revisit settled decisions only when requested or new evidence warrants it.
4. **For a problem, find the cause first.** List the plausible causes and look
   for the smallest check that tells them apart. If the cause stays uncertain,
   say so and prefer a reversible step that would reveal it.
5. **Widen the options when there is a real choice.** Consider viable
   unmentioned options, including doing nothing, removal and useful hybrids
   when they could satisfy the outcome. Treat proposals, earlier direction and
   written numbers (caps, targets, limits) as candidates to test against the
   goal unless the user or owning authority fixes them.
6. **Compare only on what separates the options.** Skip the comparison when one
   option clearly wins.
7. **Recommend the smallest complete intervention.** Fully solve the requested
   outcome under its relevant conditions with the narrowest sufficient change.
   Include adjacent changes only when the outcome needs them; do not stop at a
   partial, fragile or symptom-only fix. Keep the current approach when it is
   good enough, and add no machinery for a speculative future problem. When
   you recommend a change, say briefly why anything smaller would be
   incomplete and why any added scope is needed.

For architectural, high-risk, cross-lane, irreversible or unusually ambiguous
decisions, try to break the answer before giving it: did it anchor on the first
option, miss a viable hybrid or removal option, use criteria that do not
separate options, rest on an unchecked fact, make an unsupported strict claim,
or sound more certain than the evidence allows? Does the remedy address the
evidenced cause, and has the analysis expanded the task? Fix what you can and
preserve remaining uncertainty.
This is an internal decision-quality check, not formal review, validation,
approval or proof. Mention its effect only when material or when the user asks
for auditability; do not expose private reasoning.

Complete required source checks and the applicable verification pass before
stopping. Then stop once the answer and its material qualifications are
supported and further analysis is unlikely to change them.

## The answer

Write for the person deciding, in plain words. Use the project's shared terms
as they are; define other internal terms the first time you use them, or leave
them out.

The answer is a smallest complete intervention too: the least it needs to say
to fully support the decision. Do not shorten by under-answering, and do not
lengthen with ceremony. In practice:

- **One option clearly wins:** answer directly in a few sentences, without a
  table or headings.
- **A real choice:** compare briefly, using a table only when it helps. Give a
  recommendation, including a hybrid or conditional answer when warranted,
  and what would change it.
- **A choice about something visual:** show it (a mockup or before/after)
  instead of only describing it.

End with a clear answer, recommendation or next step. Distinguish checked
facts, inferences, assumptions and unknowns when they affect the answer;
calibrate confidence to the evidence.

Keep required owner decisions distinct from status details. Do not start
dependent work before a required choice is made; proceed within existing
authorization.

Do not invent options, criteria or sections to look thorough. No praise or
filler.

## Readiness and authority

Claims of validation, readiness, approval, formal review, deployment, resolver
behavior, plugin readiness, saved or source-of-truth artifacts, protected edits,
patch queues, mandatory validation or source-changing authority need the
evidence and authority required for that claim. Permission is not proof, and
evidence is not permission. Structural checks do not prove semantic correctness.
Summaries and earlier notes are orientation, not proof; reading files does not
grant permission to act.

Treat unsupported strict claims as `not proven`. If an action is blocked,
distinguish missing evidence from missing permission. Obtain evidence through
available, authorized checks; ask for permission only when the action lacks it.
State what remains allowed and the smallest complete next step. Follow bound
result vocabulary, including a precise `BLOCKED_*` state when applicable rules
define one. A blocked strict claim does not suppress supported advisory work.
