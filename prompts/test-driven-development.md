### Test Driven Development

## Skill Style
This skill is a generic guideline for how you, Claude Code, should operate.

## General Guidelines
TDD is **mandatory** for all implementation work. If you believe TDD is unimportant for a task (e.g. prototyping), **STOP** and ask the developer if they agree before skipping it.

## TDD Process

1. **Write tests first** — before implementing any new code or attempting a fix.
2. **Confirm the test fails** (Red) — paste the failure output as evidence.
3. **Implement the change** (Green) — minimum code to make the test pass.
4. **Confirm the test passes** — paste the passing output as evidence.

## Rules

- Always prioritise fixing the code to make a failing test pass.
- Never disable or modify a failing test to make it pass. Fix the code.
- If you believe there is a genuine exception, ask the developer to confirm.
- Present a test plan with documented cases and edge cases before implementing new functionality.
- The `implementer-implementation` template in `executing-action-plans` enforces this process with required evidence sections.

See `skills/test-driven-development/SKILL.md` for full details.
