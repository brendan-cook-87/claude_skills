# Test Driven Development

## Skill Style
Generic guideline — this skill defines the test-driven development methodology that must be followed during all implementation work. It is applied during task execution — see the **executing-action-plans** skill for how tasks are orchestrated.

## General Guidelines

- TDD is **mandatory** for all implementation work.
- If you believe TDD is unimportant for a task (e.g. prototyping that won't reach production), **STOP** and ask the developer if they agree before skipping it.

## TDD Process

1. **Write tests first** — before implementing any new code or attempting a fix.
2. **Confirm the test fails** (or the tool reproduces the finding) before attempting the fix.
3. **Implement the change** to make the test pass.
4. **Confirm the test passes** (or the tool no longer produces the finding) after the change.

## Failing Tests

- **Always** prioritize fixing the code to make a failing test pass.
- **Never** disable or modify a failing test to make it pass.
- If you believe there is a genuine exception, ask the developer to confirm before touching the test.

## Test Plans

- Before implementing any new functionality, present a **test plan** to the developer for review.
- Format the test plan neatly — use **tables** to clearly present combinations of inputs and expected results.
- Document all cases and edge cases that need testing.
