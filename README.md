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

### Share it with your team (Claude Code)

To have Claude Code offer the plugin to everyone who opens a repo, commit this
to the repo's `.claude/settings.json`:

```json
{
  "extraKnownMarketplaces": {
    "agent-workflow": {
      "source": { "source": "github", "repo": "eric-foo/agent-workflow-plugin" }
    }
  },
  "enabledPlugins": { "agent-workflow@agent-workflow": true }
}
```

## How to use it

Ask in plain language ("deep think about…", "success implement this",
"scope this plan") or call a skill by name, for example
`/agent-workflow:workflow-deep-thinking`. Skills read your repo's own
instructions (`CLAUDE.md`, `AGENTS.md`) and follow them; they never install,
deploy, commit, or push on their own.

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

## What works out of the box

Every skill works in any repo with no setup, except that two need a little
repo configuration for their full mode:

- **Delegated review-and-patch** sends your work to a reviewer from a
  *different* AI vendor (for example, Codex reviewing Claude's work), then has
  your main model decide which of the reviewer's changes to keep. It needs two
  things:
  1. **A second AI tool** from another vendor. The skill writes a
     ready-to-paste prompt; you run it in the other tool and bring the result
     back for adjudication.
  2. **A repo overlay**: a short file in your repo, referenced from
     `AGENTS.md` or `CLAUDE.md`, that opts the repo in and names the operating
     contract, model choices, protected paths, and where outputs go.

  Without an overlay, the skill explains what it would do but does not run a
  full review commission.
- **Prompt orchestrator** writes prompts in chat anywhere. Saving prompts to
  files requires the overlay to say where they go.

Code review and adversarial review work without an overlay and return advisory
findings; an overlay adds formal verdicts and patch queues.

## License

MIT. See [LICENSE](LICENSE).
