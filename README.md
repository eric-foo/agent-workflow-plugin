# Agent Workflow

Skills that make coding agents think before they build: plan the next step,
scope the work, lock the small decisions, review the result, and hand off
cleanly to the next agent. Works with Claude Code and Codex.

## Install

**Claude Code**: run inside Claude Code:

```
/plugin marketplace add eric-foo/agent-workflow-plugin
/plugin install agent-workflow@agent-workflow
```

**Codex**:

```
codex plugin marketplace add eric-foo/agent-workflow-plugin
```

Then install **Agent Workflow** from the Codex plugin browser and start a new
thread.

## Try it

- `deep think: should we split this service?`
- `what's the highest-compounding next step?`
- `scope implementation for this plan`
- `check this plan's assumptions before we build`
- `review this code`
- `hand this off to a fresh thread`
- `where are we?`

## Skills

| Skill | Use it to |
|---|---|
| `workflow-deep-thinking` | Reason carefully before answering: right question, checked facts, real options. |
| `incremental-planning` | Pick the next move that compounds most. |
| `workflow-architecture-planning` | Compare system design options before building. |
| `workflow-implementation-scoping` | Turn an agreed plan into ordered build steps. |
| `micro-decision-locking` | Settle the few small choices that would otherwise drift mid-build. |
| `workflow-assumption-gate` | Catch unverified assumptions a plan quietly relies on. |
| `success-implement` | Build with success checks defined up front. |
| `workflow-code-review` | Review code against visible evidence. |
| `workflow-adversarial-artifact-review` | Review plans, specs, and docs adversarially. |
| `workflow-delegated-review-patch` | Have a different model review and patch, then adjudicate. |
| `workflow-before-after-lens` | Compare a result against the original intent. |
| `workflow-postmortem-review` | Find what went wrong and why. |
| `workflow-prompt-orchestrator` | Write prompts and handoffs for other agents. |
| `workflow-handoff` | Package in-progress work for a fresh agent or thread. |
| `workflow-reorient` | Get a status snapshot: where are we, what's next. |
| `workflow-repo-context` | Orient in an unfamiliar repository. |

Skills are advisory: they never install, deploy, commit, or push on their own,
and they respect your repository's own rules.

## License

MIT. See [LICENSE](LICENSE).
