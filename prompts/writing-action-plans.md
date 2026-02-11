### Writing Action Plans

## Skill Style
This skill is a generic guideline for how you, claude code, should operate.

## General Guidelines
You should always write an action plan for ANY piece of work.

Action plans are stored in .plans/<branch>/<topic>.nd
if there is an existing action plan for that topic, that is okay.
You may append tasks to this action plan without editing any existing tasks (or their statuses).
Do NOT ever delete a plan.  Just append.

A good action plan should:
 - record all user questions and answers
 - focus on creating a list of simple tasks, think:
   - implementing a single function
   - fixing a single bug
   - fixing a single class of a certain type of error
 - ALWAYS include detailed acceptance criteria - another agent will work on this task and MUST be able to know when:
   - it is complete
   - it is successful
 - Ensure the test plan is as robust as possible.  Include all edge cases.  Present a table showing all combinations and expected results.
 - Contain a summary section (at the top) that summarises what this action plan is aiming to achieve, number of tasks etc.
 - Contain a template summary section (at the bottom) that summarises: 
   - how many tasks succeeded vs. failed
   - what quality checks were run on the final output and their statuses


## Example formats:

# Development Task

- ** task name ** : 
- ** task description ** : 
- ** additional context ** :
- ** acceptance criteria ** :
- ** how to test ** :
- ** status ** :
- ** attempts to complete ** :
- ** notes ** :

# Review Finding :

- ** finding name ** : 
- ** finding description ** :
- ** file name ** :
- ** line number ** :
- ** additional context ** :
- ** tool used to detect ** :
- ** acceptance criteria ** :
- ** how to test ** :
- ** status ** :
- ** attempts to complete ** :
- ** notes ** :


# Example statuses:
- ** todo ** :construction:
- ** in progress ** :wrench:
- ** success ** :white_check_mark:
- ** unfixed review finding ** :interrobang:
- ** failure ** :x: