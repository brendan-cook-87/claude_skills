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
| **General Working Process** | Top-level orchestration — assess scope, route to the right workflow. |
| **Curious and Collaborative** | Behavioral baseline. Prioritizes collaboration, asks early questions, defers architectural and business decisions to the developer. |
| **Brainstorm** | Produces fully-specified epics through three stages: design → ticket breakdown → ticket detailing. Each stage is reviewed by planning agents before engineer approval. |
| **Writing Action Plans** | Defines the task format used in all action plans — explicit instructions, concrete validation, evidence of success. |
| **Executing Action Plans** | Three-agent execution protocol (implementer → reviewer → architect) with structured output templates that enforce evidence-based reasoning. |
| **Test-Driven Development** | Mandatory TDD methodology — write tests first, confirm red, implement, confirm green. |
| **Preparing Code for Production** | Quality gate before any PR: run all checks, create a findings plan, get developer approval. |
| **Creating a PR** | PR description standards: overview, diagrams, clarifying Q&A, test plans, code quality confirmation. |
| **Hello Claude** | Session initialization — verifies project setup, summarizes workflow. |
| **Project Setup** | Discovers project tooling, explores the codebase, ensures CLAUDE.md is complete. |

## Workflow

The skills define a workflow for implementing new features. Scope is assessed first, and work is routed appropriately.

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

### Scope Assessment

- **Minor** (typo fix, one-line bug fix, config tweak) — implement directly with TDD. No plan needed.
- **Medium** (single-ticket scope, clear requirements) — create a standalone action plan at `.claude/plans/<branch>/<topic>.md`, execute with the three-agent protocol.
- **Feature** (design decisions needed, multiple components) — run the full `/brainstorm` workflow.

### Brainstorm (Feature Workflow)

The brainstorm skill produces a self-contained epic folder:

```
./epics/<topic>/
├── design.md              (Stage 1 — problem, approach, architecture)
├── epic.md                (Stage 2 — ticket list with acceptance criteria)
└── tickets/
    ├── 01-<ticket-slug>.md  (Stage 3 — full action plan per ticket)
    ├── 02-<ticket-slug>.md
    └── ...
```

Each stage goes through a review cycle (creator → planning reviewer → planning architect) before being presented to the engineer. The engineer only reviews polished output and makes decisions on genuinely ambiguous requirements.

### Three-Agent Execution

All implementation (whether from epics or standalone plans) uses the three-agent protocol:

1. **Implementer** — executes each task following a structured template (TDD evidence, claims with verification)
2. **Reviewer** — assesses completed work against ticket acceptance criteria
3. **Architect** — corroborates reviewer findings, prevents false positives from triggering unnecessary rework

Non-compliant agent output is rejected and re-dispatched. Only confirmed findings trigger fixes.

### Throughout: Curious and Collaborative

Across every phase:

- Asks insightful questions early to surface hidden assumptions
- Uses multiple-choice questions wherever possible
- Defers to the developer on architecture and business context
- Never proceeds through ambiguity
- Challenges assumptions while maintaining collaborative approach

## Project Structure

```
skills/                  # Skill definitions (installed to ~/.claude/skills/)
  <skill-name>/
    SKILL.md             # The skill file Claude Code reads
    templates/           # Structured output templates (where applicable)
prompts/                 # Concise prompt references pointing to SKILL.md files
install.sh               # Copies skills to ~/.claude/skills/
```
