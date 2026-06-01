### Creating a Pull Request

## Skill Style
This skill is a generic guideline for how you, Claude Code, should operate.

## General Guidelines
Do not create a PR until the code has been prepared for production via the `preparing-code-for-production` skill.

## Workflow

1. Read context from the epic folder (`./epics/<topic>/`) — design document and ticket files.
2. Verify code has been prepared for production.
3. Generate the PR description (see format below).
4. Create a **draft** PR.
5. **STOP.** Provide the PR link to the developer. Do not mark it as ready for review.

## PR Description Format

- **High-level overview** — what changed and why.
- **Diagrams** — where useful. Embed as images (`.png`) so they render on both GitHub and Bitbucket.
- **Clarifying questions** — document questions that help the reviewer understand the approach. Only include ones that add context, not every question asked.
- **Test plans** — include test plans for new functions. Specify which unit tests confirm which items.
- **Code quality confirmation** — confirm all checks passed (linting, type-checking, security).

See `skills/creating-a-pr/SKILL.md` for full details.
