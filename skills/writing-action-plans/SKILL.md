# Writing Action Plans

## Skill Style
Generic guideline — this skill defines the task format used in action plans. Any skill or workflow that produces a list of tasks to complete should follow this format. This is not a slash command — it defines how action plans are written across all skills.

## When This Applies

Follow this guide whenever you produce a list of tasks that will be worked through sequentially — whether inside a ticket file (from `/brainstorm`), from `/code-check` findings, or for standalone medium-scope work.

## Where Action Plans Live

- **Inside an epic:** Action plans are embedded in ticket files at `./epics/<topic>/tickets/NN-<ticket-slug>.md`. The brainstorm skill creates these.
- **Standalone (medium-scope work):** For work that doesn't warrant a full epic, create a standalone action plan at `.claude/plans/<branch>/<topic>.md`.
- **From code-check or reviews:** Findings action plans go to `.claude/plans/<branch>/<topic>.md`.

Never delete a plan. If there is an existing action plan for a topic, append new tasks without editing existing tasks or their statuses.

## Core Principles

### 1. Instructions Must Be Explicit

Each task must contain instructions clear enough that an agent with no prior context could complete it. Do not assume the reader knows why something needs to change or what the correct fix looks like.

**Bad:**
> Fix the authentication bug.

**Good:**
> The `login()` function in `src/auth.py:45` catches `AuthError` but returns `None` instead of re-raising. Change the except block to re-raise the exception so callers can handle it.

If you are writing an instruction and are not confident it is unambiguous, **ask the user to clarify** before saving it.

### 2. Validation Must Be Concrete

Every task must specify exactly how to verify it is complete. Validation criteria should be runnable — a command to execute, a test to pass, or a specific observable outcome.

**Bad:**
> Check that it works correctly.

**Good:**
> Run `pytest tests/test_auth.py::test_login_reraises_auth_error` — should pass.
> Run `ruff check src/auth.py` — should report no errors.

If you cannot write a concrete validation criterion, the task is not well-defined enough. Break it down further or ask the user what "done" looks like.

### 3. Track Progress Religiously

Every task must have Status, Attempts, and Notes fields. Update these **immediately** after each attempt — never batch updates.

---

## Task Format

Each task in an action plan must include these fields:

```markdown
### Task N: <clear, specific title>

- **Instructions:** <explicit description of what to do — include file paths, line numbers, and the exact change required>
- **Acceptance Criteria:** <list of criteria to consider this task complete>
- **Validation:** <how to verify completion — specific commands, tests, or observable outcomes>
- **Status:** Not Started
- **Evidence of Success:** <empty until complete — paste actual test/command output here>
- **Attempts:** 0
- **Notes:**
```

### Field Definitions

| Field | Purpose | Rules |
|-------|---------|-------|
| **Title** | Short description of the deliverable | Specific enough to distinguish from other tasks |
| **Instructions** | What to do and how to do it | Must be unambiguous. Include file paths, function names, and the exact change. If not obvious, ask the user first |
| **Acceptance Criteria** | Define the outcomes | List of criteria to consider this task complete |
| **Validation** | How to verify the task is complete | Must be concrete and runnable — a command, a test assertion, or a specific check. Never "works correctly" |
| **Status** | Current state of the task | One of the defined status values (see below) |
| **Evidence of Success** | Proof of completion | Actual output from the validation command. Mandatory for any task marked Done. Never empty on a completed task |
| **Attempts** | How many times this task has been attempted | Integer, starts at 0, increment after each attempt |
| **Notes** | Log of what was tried and what happened | Append after each attempt. Include what was changed, why it failed, and error messages. Never overwrite existing notes |

### Status Values

| Status | Meaning |
|--------|---------|
| `Not Started` | Not yet attempted |
| `In Progress` | Currently being worked on |
| `Done` | Completed and validated |
| `Failed` | All attempts exhausted, needs manual intervention |
| `Skipped` | User chose to skip this task |

---

## Action Plan File Format

Every action plan (whether standalone or inside a ticket) must include a header with metadata:

### Standalone Action Plan

```markdown
# <Plan Title>

**Date:** YYYY-MM-DD
**Source:** <what generated this plan — e.g., `/code-check`, `/brainstorm`, manual>
**Total Tasks:** N
**Status Summary:** X done, Y in progress, Z not started, W failed

## Tasks

### Task 1: <title>
...

### Task 2: <title>
...
```

### Inside a Ticket (from /brainstorm)

The ticket file already provides the header (scope, acceptance criteria, test plan). Tasks appear under the `## Tasks` section of the ticket. See the `brainstorm` skill for the full ticket format.

---

## Extending the Format

Skills may add domain-specific fields to each task **in addition to** the required fields. Domain-specific fields should appear between the title and the Instructions field.

For example, a code review action plan might add:

```markdown
### Task 1: Fix unused import

- **File:** src/auth.py
- **Line:** 3
- **Category:** linting
- **Severity:** blocking
- **Detected By:** `ruff check src/ --select F401`
- **Instructions:** Remove the unused import `from datetime import timedelta` on line 3.
- **Acceptance Criteria:** No F401 violations in src/auth.py
- **Validation:** Run `ruff check src/auth.py --select F401` — should report no violations.
- **Status:** Not Started
- **Evidence of Success:**
- **Attempts:** 0
- **Notes:**
```

The required fields (Instructions, Acceptance Criteria, Validation, Status, Evidence of Success, Attempts, Notes) must always be present regardless of what domain-specific fields are added.

---

## Updating After Each Attempt

**After a successful attempt:**
```markdown
- **Status:** Done
- **Evidence of Success:** <paste actual validation output>
- **Attempts:** 1
- **Notes:** Fixed on attempt 1. <description of what was changed>
```

**After a failed attempt (retries remaining):**
```markdown
- **Status:** In Progress
- **Evidence of Success:**
- **Attempts:** 1
- **Notes:** Attempt 1 failed: <what was tried and why it failed, including error output>
```

**After final failed attempt:**
```markdown
- **Status:** Failed
- **Evidence of Success:**
- **Attempts:** 3
- **Notes:** Attempt 1: <what happened>. Attempt 2: <what happened>. Attempt 3: <what happened>. Requires manual intervention.
```

Always update the **Status Summary** in the plan header after each task completes.

---

## When to Ask the User

Ask the user for clarification **before** saving a task if:

- The instructions require interpreting ambiguous requirements
- There are multiple valid approaches and the right one is not obvious
- The validation criteria depend on a decision the user hasn't made
- You are unsure whether a task should be one step or multiple steps

Use the AskUserQuestion tool — do not write vague instructions and hope for the best.

## Writing Good Instructions

### Be Specific About Location

Include file paths and line numbers when the task involves code changes:
- Good: `In src/auth.py:45, change the return type of login() from Optional[User] to User`
- Bad: `Fix the return type of the login function`

### Be Specific About the Change

Describe the exact change, not just the problem:
- Good: `Add a guard clause at the top of process_order() that raises ValueError if order.items is empty`
- Bad: `Handle the empty order case`

### Be Specific About Validation

Name the exact command or test:
- Good: `Run pytest tests/test_orders.py::test_empty_order_raises — should pass`
- Bad: `Run the tests`

### One Task, One Change

Each task should make exactly one logical change. If you find yourself writing "and also" in the instructions, split into two tasks.
