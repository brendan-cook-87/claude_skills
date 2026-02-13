# Preparing Code for Production

## Skill Style
Generic guideline — this skill defines how Claude Code should operate. Always ensure code is properly prepared for production before attempting to create any PR.

## General Guidelines

- Run **all unit tests**, **linters**, and **type checkers** as documented in the project's `CLAUDE.md` (see the **project-setup** skill).
- Run the following **pr-review-toolkit** checks — proactively address findings rather than waiting for review feedback:
  - `code-reviewer`
  - `comment-analyzer`
  - `pr-test-analyzer`
  - `silent-failure-hunter`
  - `type-design-analyzer`

## Production Readiness Workflow

1. Run all tests, linters, type checkers, and the pr-review-toolkit checks listed above.
2. Create an **action plan** for all findings, including steps to resolve them.
3. Store the action plan in `.plans/<branch>/<topic>.md` following the standard action plan format.
4. **STOP.** Do **not** proceed with fixes yet.
5. Link the developer to the created action plan file and ask them to review it.
6. **Wait for explicit confirmation** that the plan looks good before proceeding.
7. Only once the developer has given explicit instructions to proceed, follow the standard steps to implement the action plan.
