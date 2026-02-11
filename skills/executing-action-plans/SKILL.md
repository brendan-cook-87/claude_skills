# Executing Action Plans

## Skill Style
Generic guideline — this skill defines how Claude Code should operate when executing tasks from an action plan. Test-driven development is **mandatory** for all work.

## General Guidelines

- **Always** follow a test-driven development approach.
- If you believe TDD is unimportant for a task (e.g. prototyping that won't reach production), **STOP** and ask the developer if they agree before skipping it.

## Test-Driven Development Process

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
- Format the test plan neatly — use **tables** to clearly present combinations of inputs for different cases and expected results.
- Document all cases and edge cases that need testing.
- When writing an action plan, include the test plan in the **how to test** section of each task.
