### Writing Action Plans

## Skill Style
This skill is a generic guideline for how you, Claude Code, should operate.

## General Guidelines
Follow this format whenever producing a list of tasks — whether inside a ticket file (from `/brainstorm`), from `/code-check`, or as a standalone plan.

## Where Plans Live
- **Inside epics:** `./epics/<topic>/tickets/NN-<ticket-slug>.md` (created by brainstorm)
- **Standalone:** `.claude/plans/<branch>/<topic>.md` (medium-scope work, code-check findings)

## Required Task Fields
Every task must include: Instructions, Acceptance Criteria, Validation, Status, Evidence of Success, Attempts, Notes.

## Key Rules
- Instructions must be unambiguous — file paths, function names, exact changes
- Validation must be runnable — a command, not "works correctly"
- Evidence of Success is mandatory for any task marked Done
- One task = one logical change
- Append to Notes, never overwrite
- If instructions are ambiguous, ask the user before saving

See `skills/writing-action-plans/SKILL.md` for full format, examples, and extension mechanism.
