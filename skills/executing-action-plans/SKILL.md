# Executing Action Plans

## Skill Style
Generic guideline — this skill defines how Claude Code should orchestrate the execution of tasks from an action plan.

## General Guidelines

- **Spawn a new agent** to work on each task from the action plan.
- Follow the **test-driven-development** skill for all implementation work.
- You may attempt a task up to **3 times** before marking it as failed and moving on.

## Agent Task Workflow

Each spawned agent must follow this process:

1. Mark the task status as **in progress** 🔧.
2. Read the task description, acceptance criteria, and test plan carefully.
3. If there is **any ambiguity**, **stop** and ask for clarification before proceeding.
4. Implement the task following the **test-driven-development** skill.
5. Run any relevant **type checkers**, **linters**, and **unit tests**.
6. Update the task **status** to either **success** ✅ or **failure** ❌.
7. Update the **attempts to complete** field to reflect the number of attempts taken.
8. Provide detailed **notes** on the work done — and any errors that explain why an approach failed.
9. Make a **single commit** for the task.
