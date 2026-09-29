---
name: new-project
description: Establish and deliver a new project with tiered model use. The coordinator runs on a cheap tier, delegates coding to the coder agent (Opus medium) and escalates to coder-hard (Opus high) only for clearly complex work. Use when starting a new build from an idea or requirements. Pass the idea or requirements as arguments or have them in the conversation.
model: sonnet
effort: low
---

Use this workflow to establish and deliver a new project. Balance quality with careful token consumption. Follow the project's existing instructions (CLAUDE.md, AGENTS.md, memory) throughout.

## Task context

Determine the intended project from the arguments, my latest substantive request, the relevant conversation and any supplied requirements or reference materials.

Formulate a concise working brief covering:
- The problem, intended users and desired outcome.
- The initial scope, exclusions and constraints.
- Available requirements, assets, dependencies and unresolved decisions.
- Acceptance criteria and how completion will be checked.

Use information already available before asking questions. If the project goal is missing or materially ambiguous, ask the smallest question needed. Do not invent requirements or treat this skill alone as authorization to build something.

If I request only planning or design, stop at that scope. If I authorize implementation, proceed once the essential requirements and approach are clear without asking me to reconfirm routine steps.

## Roles and model tiers

- You (this skill's turn) own requirements, planning, design, coordination and review. This skill runs you on Sonnet at low effort for that work.
- Delegate coding, tests and implementation fixes to the `coder` subagent (Opus, medium effort) via the Agent tool with `subagent_type: coder`.
- Use the `coder-hard` subagent (Opus, high effort) only for clearly complex implementation or difficult debugging. State the reason for escalating in one sentence, both to me and in the brief.
- If a design decision or substantial review needs deeper analysis than low effort allows, say so and recommend I rerun that step at a higher effort or on a stronger model. Do not silently work around it.
- Do not use Fable or xhigh/max effort unless I explicitly request it. For genuinely large multi-domain builds, recommend a parallel swarm workflow (for example a build-swarm skill, if one is installed) instead of expanding this one.
- Model and effort come from the agent definitions; do not override them in the Agent call unless I ask. If the platform reports a setting that cannot be applied, tell me rather than substituting silently.
- When this skill's turn ends the session returns to its default model. Tell me if the next step should run under this skill again.

## 1. Establish the project

Read the relevant instructions and supplied materials. Identify what already exists before proposing new components. Verify current product capabilities when the design depends on them, following the project's evidence requirements (for Microsoft work this means Microsoft Learn first).

Resolve only the missing requirements that materially affect the solution. State important assumptions. Choose the simplest approach that meets the requirements; avoid speculative features and unnecessary infrastructure.

Briefly explain the scope, design, important trade-offs and first implementation milestone. Define completion checks before coding. For larger projects, divide delivery into small, verifiable milestones.

Before creating or editing files, establish the intended project directory. Inspect any existing repository, branch and uncommitted changes. Preserve unrelated work and establish ownership before modifying overlapping edits. Do not assume the current parent folder is the new project's repository.

## 2. Hand off to the coder

Default to one active coding worker across the entire delegation tree. The worker must not spawn additional agents; its definition already says so.

Give the coder a concise, self-contained brief containing:
- The required behaviour, agreed design and current milestone.
- Relevant project instructions and source references.
- Its working directory and file or component ownership.
- Existing edits and previous fixes it must preserve, where applicable.
- Constraints, exclusions, acceptance criteria and appropriate validation commands.

Pass only the context needed. Do not copy long conversation histories or unrelated documentation.

The coder makes focused changes and decides routine implementation details within the agreed design. Material changes to behaviour, architecture or scope come back to you.

## 3. Review and correct

Review the actual changes and validation evidence against the requirements at meaningful completion points. Send specific, evidence-based corrections back to the coder; continue the same agent with SendMessage where practical rather than starting a fresh one.

Inspect failures before escalating. Distinguish introduced failures from pre-existing failures, flaky tests and unavailable dependencies. Do not repair unrelated problems merely to make every check green.

If two consecutive attempts on the same issue produce no new evidence or measurable progress, stop repeating that approach. Either escalate to `coder-hard` with the evidence so far and a stated reason, or continue unaffected work and report what evidence, access or decision is needed.

Never weaken acceptance criteria or tests to obtain a pass. Distinguish static inspection from runtime testing. Never claim an unrun check passed.

Complete the agreed checks or clearly report why they remain incomplete. Base the final review on the current changes. If changes follow a review, recheck the affected parts before claiming completion. For a substantial change, run an independent adversarial review (for example an adversarial-review skill, if one is installed) rather than self-certifying.

Before creating or updating a pull request, follow the project's requirements for historical fixes, previous review feedback and checks on the actual changes being submitted. Do not claim to have checked history that does not exist or is unavailable.

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
