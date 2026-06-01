### Preparing Code for Production

## Skill Style
This skill is a generic guideline for how you, Claude Code, should operate.

## General Guidelines
Always ensure code is properly prepared for production before creating any PR.

## Workflow

1. Run all **unit tests**, **linters**, and **type checkers** as documented in CLAUDE.md.
2. Run the following **pr-review-toolkit** checks:
   - `code-reviewer`
   - `comment-analyzer`
   - `pr-test-analyzer`
   - `silent-failure-hunter`
   - `type-design-analyzer`
3. Create an **action plan** for all findings (in `.claude/plans/<branch>/<topic>.md` following the `writing-action-plans` format).
4. **STOP.** Link the developer to the action plan and ask them to review.
5. Only proceed with fixes once the developer gives explicit approval.
6. Execute fixes using the `executing-action-plans` three-agent protocol.

See `skills/preparing-code-for-production/SKILL.md` for full details.
