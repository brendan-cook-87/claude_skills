# Claude Skills

A collection of skills for [Claude Code](https://docs.anthropic.com/en/docs/claude-code) that define a structured, collaborative workflow for software development.

## What Are Skills?

Skills are markdown files that instruct Claude Code how to behave during different phases of work. They are installed to `~/.claude/skills/` and apply automatically across all projects.

## Installation

```bash
./install.sh
```

This copies all skills from `skills/` into `~/.claude/skills/`.

## Skills

| Skill | Purpose |
|---|---|
| **Curious and Collaborative** | Behavioral baseline applied to all tasks. Prioritizes collaboration, asks early questions, and defers architectural and business decisions to the developer. |
| **Brainstorming** | Guides how Claude approaches new feature requests. Presents multiple options with tradeoffs before committing to an approach. |
| **Writing Design Documents** | Defines when and how to produce design documents for large work. Focuses on business context and high-level technical approach, with Mermaid diagrams. |
| **Writing Action Plans** | Defines how to break work into small, atomic tasks with detailed acceptance criteria and test plans. |
| **Executing Action Plans** | Defines a strict test-driven development process for implementing tasks from an action plan. |
| **Test-Driven Development** | Defines how agents should work through action plan tasks — including status tracking, retries, and per-task commits. |
| **Preparing Code for Production** | Defines the quality gate before any PR: run all tests, linters, and checks, then create a plan for any findings. |
| **Creating a PR** | Defines PR description standards: high-level overview, diagrams, clarifying Q&A, test plans, and code quality confirmation. |

## Workflow

The skills define a sequential workflow for implementing new features. Each phase requires explicit developer approval before moving to the next.

```
Brainstorm --> Design --> Plan --> Implement --> Production Review --> PR
```

### 1. Brainstorm

When asked to work on a new feature, Claude starts by asking questions — not writing code. It presents multiple approaches with tradeoffs and lets the developer choose a direction. No implementation begins until the developer explicitly says to proceed.

### 2. Design Document

For work that involves architectural decisions, Claude writes a design document before any planning or implementation. Design documents are stored in `.designs/<branch>/` and focus on business context and high-level technical approach. Diagrams are authored in Mermaid (`.diagrams/<topic>_<type>.mmd`) and rendered to PNG.

Small changes may skip this step — Claude will ask the developer whether a design is warranted.

### 3. Action Plan

Before implementation, Claude creates an action plan in `.plans/<branch>/`. Work is broken into the smallest possible atomic tasks. Each task includes:

- A clear description
- Detailed acceptance criteria
- A test plan with input/output tables covering edge cases

Claude stops after writing the plan and waits for the developer to review and approve it.

### 4. Implementation (Test-Driven Development)

Tasks are executed using strict TDD:

1. Write tests first.
2. Confirm the tests fail.
3. Implement the change.
4. Confirm the tests pass.

Each task is worked on by a spawned agent (up to 3 attempts). On completion, the agent updates the task status and makes a single commit. Failing tests are never disabled or modified to pass — the code is fixed instead.

### 5. Production Readiness

After implementation, Claude runs all tests, linters, type checkers, and available review checks. Any findings are captured in a new action plan. Claude stops and presents the plan to the developer for approval before making fixes.

### 6. Pull Request

Only after the code passes all quality gates does Claude create a PR. The description includes:

- A high-level overview of the change
- Diagrams where useful
- Relevant questions and answers from the development process
- Test plans and which unit tests cover them
- Confirmation that all code quality checks pass

### Throughout: Curious and Collaborative

Across every phase, Claude operates with a collaborative mindset:

- Asks insightful questions early to surface hidden assumptions
- Uses multiple-choice questions wherever possible
- Defers to the developer on architecture and business context
- Never proceeds through ambiguity

## Project Structure

```
skills/                  # Skill definitions (installed to ~/.claude/skills/)
  <skill-name>/
    SKILL.md             # The skill file Claude Code reads
prompts/                 # Source/draft prompt files
install.sh               # Copies skills to ~/.claude/skills/
```
