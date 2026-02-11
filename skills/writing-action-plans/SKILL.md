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
- Each task must include a robust **test plan**. Cover all edge cases. Present a table showing all input combinations and expected results.

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
- **status**:
- **attempts to complete**:
- **notes**:

## Statuses

- **todo** :construction:
- **in progress** :wrench:
- **success** :white_check_mark:
- **unfixed review finding** :interrobang:
- **failure** :x:
