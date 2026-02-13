# Writing Action Plans

## Skill Style
Generic guideline — this skill defines how Claude Code should operate across all tasks. You should always write an action plan for ANY piece of work.

## General Guidelines

- Create an action plan **before** starting any piece of work.
- Store action plans in `.plans/<branch>/<topic>.md`.
- If an action plan already exists for a topic, **append** new tasks to it. Never edit existing tasks or their statuses. Never delete a plan.
- Break work into the smallest possible tasks. Each task should be a single, atomic unit — implementing one function, fixing one bug, fixing one class of error.
- Always record user questions and answers in the plan.

## What Makes a Good Action Plan

### Summary Section (top of the plan)
- Describe what the action plan aims to achieve.
- State the total number of tasks.

### Tasks
- Each task must include detailed **acceptance criteria**. Another agent will execute the task and must be able to determine when it is complete and when it is successful.
- Each task must include a **how to test** section. See the **test-driven-development** skill for test plan standards.

### Results Summary Section (bottom of the plan)
- How many tasks succeeded vs. failed.
- What quality checks were run on the final output and their statuses.

## Task Formats

### Development Task

- **task name**:
- **task description**:
- **additional context**:
- **acceptance criteria**:
- **how to test**:
- **commit strategy**:
- **evidence of success**:
- **status**:
- **attempts to complete**:
- **notes**:

### Review Finding

- **finding name**:
- **finding description**:
- **file name**:
- **line number**:
- **additional context**:
- **tool used to detect**:
- **acceptance criteria**:
- **how to test**:
- **commit strategy**:
- **evidence of success**:
- **status**:
- **attempts to complete**:
- **notes**:

## Evidence of Success

The **evidence of success** field is mandatory for any task or finding marked as **success** ✅. It must contain the actual output (or a meaningful excerpt) from the test command specified in **how to test**.

- The executing agent is responsible for running the test command and pasting the output into this field.
- A task must **never** be marked as **success** ✅ if this field is empty or missing.
- If the test command fails, paste the failure output here instead — the task status should be **failure** ❌ or remain **in progress** 🔧.
- Acceptable evidence includes: compilation output, test runner output, linter output, or command exit status with relevant context.

This field exists to prevent agents from claiming success without actually verifying their work.

## Statuses

- **todo** 🚧
- **in progress** 🔧
- **success** ✅
- **unfixed review finding** ⁉️
- **failure** ❌
