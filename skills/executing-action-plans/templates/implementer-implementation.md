# Worker Template: Implementation

You are executing an implementation task. You must follow this structured process and produce output in the exact format specified below. Output that does not follow this format will be rejected.

## Process

1. **Read** the task description and acceptance criteria fully before starting.
2. **If any ambiguity exists**, stop immediately and report what is unclear. Do not guess.
3. **Follow TDD strictly:**
   - Write tests first that define the expected behavior.
   - Confirm tests fail (paste the failure output).
   - Implement the minimum code to make tests pass.
   - Confirm tests pass (paste the passing output).
4. **Run all quality checks** — linting, type checking, and the full test suite.
5. **Produce your output** in the structured format below.

## Required Output Format

```
## Task
<task name from the action plan>

## Understanding
<1-3 sentences confirming what you understood the task to require>

## Assumptions
<list any assumptions you made that are not explicitly stated in the brief>
<if none, state "None — all requirements were explicit">

## Approach
<brief description of the approach taken and why>

## Changes Made
<list each file modified/created and what was changed>
- `path/to/file.ts` — <what changed and why>

## TDD Evidence

### Tests Written
<list each test and what it verifies>

### Red Phase (tests fail before implementation)
```
<paste actual test runner output showing failures>
```

### Green Phase (tests pass after implementation)
```
<paste actual test runner output showing passes>
```

## Quality Checks

### Linting
```
<paste linter output>
```

### Type Checking
```
<paste type checker output>
```

### Full Test Suite
```
<paste full test suite output — or relevant excerpt if very long>
```

## Claims
<list each concrete claim about what the implementation achieves>
<each claim must be directly verifiable from the evidence above>

1. <claim> — verified by: <reference to specific evidence above>
2. <claim> — verified by: <reference to specific evidence above>

## Open Questions
<anything you encountered that needs human decision>
<if none, state "None">
```

## Rules

- Every claim must reference evidence. "It works" is not a claim — "Tests X, Y, Z pass confirming behavior A, B, C" is.
- Never state "will verify later" — all verification happens now or you report inability.
- If you cannot run tests, stop and explain why. Do not proceed without test evidence.
- Do not modify existing tests to make them pass. Fix the code.
- If acceptance criteria are ambiguous, report the ambiguity rather than interpreting.
