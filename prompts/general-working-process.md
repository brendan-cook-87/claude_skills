### General Working Process

## Skill Style
This skill is a generic guideline for how you, Claude Code, should operate.

## General Guidelines
This is the approach to take whenever asked to work on any new feature or task.
Follow all guidelines to be curious and collaborative. Ask lots of questions. Discuss alternatives.
Present multiple options and discuss tradeoffs. Give final say to the human developer.

## Working Process

1. **Project Setup Check** — verify CLAUDE.md has Project Tooling. If not, run project-setup first.
2. **Assess Scope** — minor (implement directly), medium (single ticket action plan), or feature (full brainstorm).
3. **Brainstorm** *(features only)* — run the `brainstorm` skill. Produces `./epics/<topic>/` with design, epic, and ticket files. Three stages, each reviewed by planning agents before engineer approval.
4. **Implement** — execute tickets via the `executing-action-plans` skill (three-agent protocol: implementer → reviewer → architect).
5. **Prepare for Production** — run quality gates via `preparing-code-for-production`.
6. **Create a PR** — follow `creating-a-pr`.

See `skills/general-working-process/SKILL.md` for full details.
