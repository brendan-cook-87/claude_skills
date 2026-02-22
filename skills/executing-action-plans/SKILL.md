# Executing Action Plans

## Skill Style
Generic guideline — this skill defines how Claude Code should orchestrate the execution of tasks from an action plan.

## General Guidelines

- **Spawn a new agent** to work on each task from the action plan.
- Follow the **test-driven-development** skill for all implementation work.
- You may attempt a task up to **3 times** before marking it as failed and moving on.

## Spawning Agents

When spawning a subagent for a task, you **must** include explicit instructions in the agent's prompt to:

1. Read this skill file (`skills/executing-action-plans/SKILL.md`) before doing any work.
2. Read the **test-driven-development** skill file before doing any implementation work.
3. Follow the Agent Task Workflow defined below.

Do not assume the agent will inherit context or knowledge of the required process — always pass it explicitly.

## Agent Task Workflow

Each spawned agent must follow this process:

1. Mark the task status as **in progress** 🔧.
2. Read the task description, acceptance criteria, and test plan carefully.
3. If there is **any ambiguity**, **stop** and ask for clarification before proceeding.
4. Implement the task following the **test-driven-development** skill.
5. Run any relevant **type checkers**, **linters**, and **unit tests**.
6. **Provide evidence of test results.** You must include the actual test output demonstrating pass or fail. **Never skip this step.** If you cannot execute the tests:
   - **Stop and ask for clarification** on how to run them, OR
   - **Mark the task as incomplete** and add detailed notes explaining why the tests were not runnable (e.g., missing dependencies, unclear test commands, environment issues) so the next agent can pick it up.
7. Update the task **status** to either **success** ✅ or **failure** ❌.
8. Update the **attempts to complete** field to reflect the number of attempts taken.
9. Provide detailed **notes** on the work done — and any errors that explain why an approach failed.
10. Make a **single commit** for the task.
