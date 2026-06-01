# General Working Process

## Skill Style
Generic guideline — this skill defines how Claude Code should operate whenever asked to work on any new feature or task.

## Core Principle

Getting the **Brainstorm** phase right is critical. Mistakes or shallow thinking in planning compound into wasted implementation effort, wrong solutions, and costly rework. Invest heavily in upfront reasoning before writing any code.

## General Guidelines

- Follow all guidelines to be curious and collaborative. Ask lots of questions. Discuss alternatives.
- Present **multiple options** and discuss tradeoffs for each. The aim is to reach the best possible solution.
- Be clear and concise, and always give final say to the human developer.
- Decide whether the change is minor (quick fix, one-file change) or needs the full workflow. **ASK** if unsure.

## Extended Thinking Requirement

**You MUST use extended thinking (deep reasoning / thinking blocks) during the Brainstorm phase.** This phase demands thorough, structured reasoning that cannot be rushed. Extended thinking ensures you:

- Fully explore the problem space before proposing solutions.
- Identify hidden assumptions, edge cases, and risks early.
- Evaluate multiple design alternatives with genuine depth rather than surface-level comparisons.
- Produce higher-quality design documents and ticket breakdowns that hold up during implementation.

Do **NOT** skip or shortcut extended thinking during brainstorming. It is better to spend more time reasoning upfront than to rush into a flawed design.

## Working Process

### 1. Project Setup Check

**STOP.** Before anything else, verify that the project's `CLAUDE.md` contains a `## Project Tooling` section documenting test, lint, and type-check commands. If it does not, you MUST run the **project-setup** skill first and get developer confirmation before proceeding. Do **NOT** skip this step.

### 2. Assess Scope

Determine whether this work needs the full brainstorm workflow or is a minor change:

- **Minor change** (typo fix, one-line bug fix, config tweak, styling adjustment) — implement directly following TDD discipline. No epic needed.
- **Medium change** (single-ticket scope, clear requirements, no design decisions) — create a standalone action plan at `.claude/plans/<branch>/<topic>.md` following the `writing-action-plans` format. Execute it using the `executing-action-plans` three-agent protocol. Skip the full brainstorm but keep the same execution rigour.
- **Feature / multi-ticket work** (design decisions needed, multiple components, unclear requirements) — run the full brainstorm workflow.

**ASK** the developer if you are unsure which category the work falls into.

### 3. Brainstorm *(full workflow — extended thinking required)*

Run the **brainstorm** skill. This produces a fully-specified epic through three stages:

1. **Stage 1: Scope & Approach** — discovery, solution proposals, design document. Reviewed by planning agents before engineer approval.
2. **Stage 2: Ticket Breakdown** — discrete tickets with acceptance criteria and dependencies. Reviewed before engineer approval.
3. **Stage 3: Ticket Detailing** — full action plan for each ticket (tasks, validation, test plans). Reviewed before engineer approval.

Output: `./epics/<topic>/` containing `design.md`, `epic.md`, and `tickets/*.md`.

Do **NOT** proceed to implementation until the developer explicitly approves all three stages.

### 4. Implement

Once the developer gives explicit approval, execute tickets in dependency order following the **executing-action-plans** skill:

- Each ticket is executed through the three-agent protocol (implementer → reviewer → architect).
- Agents follow structured output templates that enforce evidence-based reasoning.
- Per-task commits during implementation, squash per-ticket after review passes.
- Update ticket status as work progresses.

### 5. Prepare for Production

After all tickets in the epic are complete, follow the **preparing-code-for-production** skill:

- Run all tests, linters, type checkers, and pr-review-toolkit checks.
- Create an action plan for any findings.
- Get developer approval before fixing.

### 6. Create a PR

Once the code has been prepared for production, follow the **creating-a-pr** skill to open a pull request.

## Workflow Summary

```
Project Setup Check
        │
        ▼
Assess Scope ──── minor ──── implement directly (TDD) ──── PR
        │
        ├── medium ──── action plan ──── implement (three-agent) ──── Prepare ──── PR
        │
        └── feature ──── /brainstorm
                            │
                            ├── Stage 1: Design (review cycle → engineer approval)
                            ├── Stage 2: Tickets (review cycle → engineer approval)
                            └── Stage 3: Detail (review cycle → engineer approval)
                                    │
                                    ▼
                              Implement (three-agent protocol per ticket)
                                    │
                                    ▼
                              Prepare for Production
                                    │
                                    ▼
                              Create PR
```

## Skill References

| Phase | Skill |
|-------|-------|
| Session start | `hello-claude` |
| Project setup | `project-setup` |
| Brainstorming | `brainstorm` |
| Action plan format | `writing-action-plans` |
| Execution | `executing-action-plans` |
| TDD methodology | `test-driven-development` |
| Production prep | `preparing-code-for-production` |
| PR creation | `creating-a-pr` |
| Behavioural baseline | `curious-and-collaborative` |
