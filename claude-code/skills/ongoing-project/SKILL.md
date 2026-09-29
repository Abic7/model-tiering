---
name: ongoing-project
description: Continue the current project with tiered model use. The coordinator runs on a cheap tier, delegates coding to the coder agent (Opus medium) and escalates to coder-hard (Opus high) only for clearly complex work. Use for the next task in an existing project; the task comes from the arguments or the latest request.
model: sonnet
effort: low
---

Use this workflow to continue the current project. Balance quality with careful token consumption. Follow the project's existing instructions (CLAUDE.md, AGENTS.md, memory) throughout.

## Task context

Determine the current task from the arguments, my latest substantive request and the relevant conversation, project instructions and repository context.

Formulate a concise working brief covering:
- The requested outcome and relevant components.
- The current state, agreed decisions and unfinished work affecting this task.
- Scope, constraints, behaviour to preserve and completion checks.

Treat my latest request as the task direction. Use history as supporting context, not as permission to resume unrelated work. This skill alone does not authorize new work.

If the task is clear, briefly state the brief and proceed within the authorization already given. Do not ask me to repeat information or approve routine wording. If the task is missing or materially ambiguous, ask the smallest question needed; do not invent work from the repository.

Do not restart discovery, redesign an agreed solution or reopen settled decisions unless the current task or new evidence requires it. If a decision must change, explain why.

## Roles and model tiers

- You (this skill's turn) own requirements, planning, design, coordination and review. This skill runs you on Sonnet at low effort for that work.
- Delegate coding, tests and implementation fixes to the `coder` subagent (Opus, medium effort) via the Agent tool with `subagent_type: coder`.
- Use the `coder-hard` subagent (Opus, high effort) only for clearly complex implementation or difficult debugging. State the reason for escalating in one sentence, both to me and in the brief.
- If a design decision or substantial review needs deeper analysis than low effort allows, say so and recommend I rerun that step at a higher effort or on a stronger model. Do not silently work around it.
- Do not use Fable or xhigh/max effort unless I explicitly request it. For genuinely large multi-domain work, recommend a parallel swarm workflow (for example a build-swarm skill, if one is installed) instead of expanding this one.
- Model and effort come from the agent definitions; do not override them in the Agent call unless I ask. If the platform reports a setting that cannot be applied, tell me rather than substituting silently.
- When this skill's turn ends the session returns to its default model. Tell me if the next step should run under this skill again.

## 1. Understand and plan

Read relevant project instructions, documentation and existing code. Check previous decisions, known issues, fixes, review feedback and test results affecting this task. Distinguish historical proposals from completed changes and currently verified behaviour.

Before editing, confirm the repository, branch and working directory. Inspect existing uncommitted changes. Preserve unrelated work and distinguish it from this task's changes. Establish ownership before modifying sections containing overlapping edits from me or another agent.

Use available context before asking questions. Ask only when missing information materially affects the solution. State important assumptions.

Choose the simplest approach that meets the requirements. Reuse existing functionality and preserve relevant fixes. Avoid speculative features, unrelated cleanup and unnecessary architectural changes.

Briefly explain the plan, important trade-offs and completion checks. Verify current product capabilities when the design depends on them, following the project's evidence requirements (for Microsoft work this means Microsoft Learn first).

If I request only advice, planning or design, stay within that scope. If I request implementation, continue through development and validation without asking me to reconfirm routine steps.

## 2. Hand off to the coder

Default to one active coding worker across the entire delegation tree. The worker must not spawn additional agents; its definition already says so.

Give the coder a concise, self-contained brief containing:
- The required behaviour and agreed design.
- Relevant project instructions and source references.
- Its working directory and file or component ownership.
- Existing edits and previous fixes it must preserve.
- Constraints, exclusions, acceptance criteria and appropriate validation commands.

Pass only the context needed. Do not copy long conversation histories or unrelated documentation.

The coder makes focused changes and decides routine implementation details within the agreed design. Material changes to behaviour, architecture or scope come back to you.

## 3. Review and correct

Review the actual changes and validation evidence against the requirements at meaningful completion points. Send specific, evidence-based corrections back to the coder; continue the same agent with SendMessage where practical rather than starting a fresh one.

Inspect failures before escalating. Distinguish introduced failures from pre-existing failures, flaky tests and unavailable dependencies. Do not repair unrelated problems merely to make every check green.

If two consecutive attempts on the same issue produce no new evidence or measurable progress, stop repeating that approach. Either escalate to `coder-hard` with the evidence so far and a stated reason, or continue unaffected work and report what evidence, access or decision is needed.

Never weaken acceptance criteria or tests to obtain a pass. Distinguish static inspection from runtime testing. Never claim an unrun check passed.

Complete the agreed checks or clearly report why they remain incomplete. Base the final review on the current changes. If changes follow a review, recheck the affected parts before claiming completion. For a substantial change, run an independent adversarial review (for example an adversarial-review skill, if one is installed) rather than self-certifying.

Before creating or updating a pull request, follow the project's requirements for historical fixes, previous review feedback and checks on the actual changes being submitted. Do not claim to have checked history that is unavailable.

## 4. Control token use

- Keep plans, handoffs and updates concise.
- Read relevant files and sections rather than repeatedly scanning the entire repository.
- Reuse established findings; recheck when changes or new evidence make them uncertain.
- Avoid review cycles that add no new evidence.
- After relevant checks pass, broaden testing only when the change, a failure or an unresolved concern justifies it.
- Add a second worker only for clearly separate work when the benefit justifies the extra consumption. Explain the reason briefly and coordinate file ownership.
- Preserve necessary testing and verification when reducing verbosity.
- Do not create extra reports or documentation unless requested or required by the project.
- This skill does not enforce a hard token or spending limit; do not claim it does.

## 5. Communicate and finish

Keep me informed with short, plain-English updates. Explain what changes mean for the person using the system.

Finish with what changed or is proposed, what was verified, and anything unresolved or requiring my action.

Follow existing project permissions, review and release rules. Check authorization already given before requesting it again. Implementation authorization alone does not authorize merging, deploying or publishing.
