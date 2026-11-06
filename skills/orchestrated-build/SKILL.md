---
name: orchestrated-build
description: Run a build as a small software factory. One orchestrator briefs subagents through fixed stages (research, tickets, design gate, grill, contract, implement in parallel, review, QA, report) and stops at the gates where a human decides. Use when the user asks to build, extend or improve a project "the factory way", with /orchestrated-build, or asks for an orchestrated build with subagents.
disable-model-invocation: true
---

# Orchestrated build

You are the director and integrator, not the only builder. Your job is to brief every stage, read every result, decide what moves on, and stop at the gates. Subagents do the typing.

This skill is the generalised version of the loop shown in the talk "Under the hood: Claude at Buildwise". It assumes Matt Pocock's skills are installed (`/research`, `/to-spec`, `/to-tickets`, `/grill-me`, `/implement`, `/code-review`). Where one is missing, run the fallback written next to it. Where the project has an AGENTS.md or CLAUDE.md, read it first and follow it: house rules win over this file.

## Modes

Ask once, at the start, which mode applies. Default to **manual**.

- **manual**: stop at every gate marked below and wait for the user. Nothing moves until they say so. Use for anything a client will see or that touches production.
- **automatic**: you decide at the gates, log every decision in the report, and the user reads the report at the end. Use for a redesign, a prototype, a throwaway experiment.

Whatever the mode, three decisions always belong to the human: what it should look like, what each ticket means, and whether the result works.

## Stage 0: Frame and audit

1. Restate the goal in two sentences and confirm it with the user (AskUserQuestion). Capture: what exists already, what "done" looks like, what must not change.
2. If there is existing code, audit it yourself before anyone touches it: read the entry points, the build and test commands, the folder layout. Write `docs/build/00-audit.md` with a short report card: what is good, what is weak, what is risky. Back up or tag the current state so there is something to compare against.
3. Create `docs/build/` if it does not exist. Every stage writes its output there. That folder is the memory of the run; a new session can pick it up from the files alone.

## Stage 1: Research (subagents)

Spawn one subagent per research question, in parallel, each running `/research` (fallback: "investigate against primary sources, cite every claim, write a Markdown file"). Typical split: fundamentals of the domain, what succeeded and failed before, the current state of the codebase or product.

Each subagent writes `docs/build/01-research-<topic>.md`. You read all of them and write a one-page `docs/build/01-research-summary.md`: the five findings that change what we build. Do not paste the research into the main context; link to the files.

## Stage 2: Tickets (subagents)

Run `/to-spec` on the framed goal plus the research summary to get `docs/build/02-spec.md` (fallback: write the spec yourself: scope, non-goals, constraints, acceptance criteria).

Then `/to-tickets` to cut the spec into tracer-bullet tickets (fallback: see `references/ticket-template.md`). A good ticket is functional, small enough to verify in one session, and names the files it is likely to touch. Write them to `docs/build/02-tickets/NN-<slug>.md`.

## Gate A: Design (manual mode: stop here)

If the work has a visible surface, produce an artifact the user can judge without reading code: a mockup, a page, a clickable prototype, a diagram. Ask for feedback, revise, repeat until approved. Record the approved version in `docs/build/03-design.md`. If nothing is visible, say so and skip the gate.

## Gate B: Grill, ticket by ticket (manual mode: stop here)

Run `/grill-me` on each ticket in turn (fallback: "ask the user every question whose answer changes the implementation, in rounds, with a recommendation per question, until nothing is left to ask"). Write the answers into the ticket under a `## Decisions` heading. A ticket with open questions does not go to a builder.

In automatic mode, answer the questions yourself from the spec and research, mark each answer `(assumed)`, and list all assumptions in the report.

## Stage 3: Contract

Before anyone builds in parallel, lock the seams:

- Write `docs/build/04-contract.md`: the interfaces between the pieces (function signatures, data shapes, routes, events), the shared conventions (naming, error handling, where tests live), and the exact **file ownership**: which ticket owns which files. No two builders may own the same file.
- Build the core yourself if there is one: the skeleton everything hangs off, with stubs for every seam. Pre-wire each builder's file so it exists and compiles before the builder starts.
- Make sure the test command runs and is green on the skeleton. If a Stop hook that runs the tests is not in place, propose adding one (see `references/hooks.md`) and let the user decide.

## Stage 4: Implement in parallel (subagents)

Spawn one builder subagent per ticket, in one message, all in the background. Each brief contains: the ticket with its decisions, the contract, the files it owns and only those, the test command, and the rule "verify your own work before handing back" (run the tests, open the page, call the endpoint, whatever applies). Each builder runs `/implement` (fallback: "implement the ticket, write or update tests at the seams, run them, report what you verified and what you did not").

A builder reports back with: what changed, what was verified and how, what was not run. Read every handback. A handback without evidence of verification goes back to the builder.

## Stage 5: Review (subagents)

Spawn review subagents running `/code-review` against the diff since the contract tag (fallback: review by hand on two axes: does it do what the ticket says, and does it follow the standards). Give them `references/review-standards.md` plus the project's own conventions. Reviews come back as findings with a severity; you decide which block the release and send those back to the owning builder.

This stage is what keeps the code from turning to spaghetti when nobody reads it: the standards are written once, and applied every run.

## Stage 6: QA

Run the result the way a user would, not the way a developer would. If the project has a QA or playtest skill, use it with several personas. If not, spawn two or three subagents with different user profiles and one question each ("would you come back tomorrow", "what confused you in the first minute", "what broke"). Treat their reports as claims and verify the serious ones yourself before they go in the report.

## Stage 7: Report

Write `docs/build/07-report.md` from `references/report-template.md`: what was asked, what was built, what was verified and how, what was not run, every assumption made in automatic mode, every review finding and what happened to it, and what to do next. Written so someone who did not read the transcript can follow it.

If the user is not a developer, also offer an artifact that explains the run visually, with as little text as possible.

## Rules that do not bend

- Never let two builders edit the same file in the same run.
- Never report "done" without saying what was verified and what was not.
- Keep big reads out of your own context: subagents read, you read their summaries.
- A gate skipped in automatic mode is still a decision; log it.
- If the spec is thin, stop and grill. A factory amplifies the spec it gets.
