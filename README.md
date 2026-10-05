# Agent Workflow

Skills that make coding agents think before they build and prove the work
after. Works with Claude Code and Codex.

The foundation is three skills:

- **Deep thinking** (`workflow-deep-thinking`): get the question right, check
  the facts it rests on, and weigh the real options before answering.
- **Success implement** (`success-implement`): define how you'll know it
  worked, make the smallest complete change, then prove it.
- **Prompt orchestrator** (`workflow-prompt-orchestrator`): write the prompts
  and handoffs that carry work between agents.

On top of that sit the review skills, delegated review-and-patch and
adversarial review, tested and tuned over thousands of PRs.

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

## Skills

| Skill | Use it to |
|---|---|
| **Foundation** | |
| `workflow-deep-thinking` | Reason carefully before answering: right question, checked facts, real options. |
| `success-implement` | Build with success checks defined up front, then prove the result. |
| `workflow-prompt-orchestrator` | Write prompts and handoffs for other agents. |
| **Review** | |
| `workflow-delegated-review-patch` | Have a different model review and patch, then adjudicate what to keep. |
| `workflow-adversarial-artifact-review` | Review plans, specs, and docs adversarially. |
| `workflow-code-review` | Review code against visible evidence. |
| **Planning and handoffs** | |
| `incremental-planning` | Pick the next move that compounds most. |
| `workflow-architecture-planning` | Compare system design options before building. |
| `workflow-implementation-scoping` | Turn an agreed plan into ordered build steps. |
| `micro-decision-locking` | Settle the few small choices that would otherwise drift mid-build. |
| `workflow-assumption-gate` | Catch unverified assumptions a plan quietly relies on. |
| `workflow-handoff` | Package in-progress work for a fresh agent or thread. |

Skills are advisory: they never install, deploy, commit, or push on their own,
and they respect your repository's own rules.

## License

MIT. See [LICENSE](LICENSE).
