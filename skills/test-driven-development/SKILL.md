# Test Driven Development

## Skill Style
Generic guideline — this skill defines how Claude Code should operate when executing tasks from an action plan.

## General Guidelines

- Always **spawn a new agent** to work on each task from an action plan.
- You may attempt to have an agent work on a task up to **3 times** before moving on and leaving it as failed.

## Agent Task Workflow

Each spawned agent must follow this process:

1. Mark the task status as **in progress** :wrench:.
2. Read the task description carefully.
3. If there is **any ambiguity**, **stop** and ask for clarification before proceeding.
4. Attempt to fix/implement the task using a **test-driven development** approach:
   - Write or update tests first.
   - Implement the change to make the tests pass.
5. Run the tests to confirm the fix.
6. Run any relevant **type checkers**, **linters**, and **unit tests**.
7. Update the task **status** to either **success** :white_check_mark: or **failure** :x:.
8. Update the **attempts to complete** field to reflect the number of attempts taken.
9. Provide detailed **notes** on the fix attempted — and any errors that explain why an approach failed.
10. Make a **single commit** for the task.
