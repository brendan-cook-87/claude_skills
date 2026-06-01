### Executing Action Plans

## Skill Style
This skill is a generic guideline for how you, Claude Code, should operate.

## General Guidelines
All subagent work during execution is governed by **structured output templates** that enforce evidence-based reasoning.

This skill is self-contained — it defines both the execution protocol (implementer → reviewer → architect) and the mandatory structured output templates that each agent must follow.

## Templates

Each agent role must follow a structured template from `skills/executing-action-plans/templates/`:

**Implementer (per-task):**
- `implementer-implementation.md` — for code changes, features, fixes (enforces TDD evidence)
- `implementer-research.md` — for exploration, information gathering (enforces sourced claims)
- `implementer-investigation.md` — for debugging, root cause analysis (enforces hypothesis testing)
- `implementer-review.md` — for code/architecture assessment (enforces traced findings)
- `implementer-summarising.md` — for condensing information (enforces source traceability)

**Reviewer (per-ticket):**
- `reviewer.md` — checks implementer output against acceptance criteria

**Architect (per-ticket, only on findings):**
- `architect.md` — corroborates or refutes reviewer findings

## Compliance Rule

Non-compliant output is always rejected and re-dispatched with specific feedback. Compliance rejection does not count as a task attempt — it means the agent didn't follow format instructions, not that the task failed.

See `skills/executing-action-plans/SKILL.md` for full details and compliance criteria.
