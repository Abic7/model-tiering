# model-tiering

Route each part of an AI-assisted build to the cheapest model tier that can do it well, and reserve the premium tier for the reasoning nobody else can do.

This repo holds the same workflow expressed for two tools:

- `codex/` - paste-in prompts for OpenAI Codex (the original "Astra / Sol" pattern).
- `claude-code/` - the same pattern as Claude Code skills and subagents, with model and effort fixed in frontmatter so the routing lives in configuration rather than in the prompt.

## The idea

| Tier | Role | Give it |
|---|---|---|
| Coordinator | cheap model, low effort | Requirements, plan, handoffs, review of evidence |
| Worker | mid tier, medium effort | Coding, tests, implementation fixes |
| Escalation | mid tier, high effort | Clearly complex implementation or difficult debugging, with the reason stated |
| Premium | strongest model | Only on explicit request |

Rules that hold in both implementations:

- One active coding worker across the whole delegation tree. Workers never spawn their own agents.
- Inspect a failure before increasing effort. Two attempts on the same issue with no new evidence means stop, escalate or report, never loop.
- Never weaken a test or acceptance criterion to get a pass. Never claim an unrun check passed. Separate static inspection from runtime testing.
- Pass the worker a self-contained brief, not the conversation history.
- Implementation authorisation does not authorise merging, deploying or publishing.

## Codex

Select the coordinator model at Low reasoning and Standard speed, then paste the prompt from `codex/01-new-project.md` (new build) or `codex/02-ongoing-project.md` (next task in an existing project) into the project chat. The prompt asks the coordinator to spawn workers with a stated model and effort; it cannot change the main chat's own settings.

## Claude Code

Copy the folders into your user-level config so they follow you into every project:

```
~/.claude/agents/coder.md
~/.claude/agents/coder-hard.md
~/.claude/skills/new-project/SKILL.md
~/.claude/skills/ongoing-project/SKILL.md
```

Then run `/new-project <idea>` or `/ongoing-project <task>`.

- The skills carry `model: sonnet` and `effort: low`; the override lasts for the turn that invokes the skill, then the session returns to its default model.
- `coder` is `model: opus`, `effort: medium`. `coder-hard` is `model: opus`, `effort: high`.
- Model aliases resolve through Claude Code's normal settings, so the tiers move with the models without editing these files.

The skills mention optional swarm and adversarial-review companions. Any equivalent skill will do; nothing here depends on them.

## Licence

MIT. See `LICENSE`.
