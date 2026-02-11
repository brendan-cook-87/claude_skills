### Preparing Code for Production

## Skill Style
This skill is a generic guideline for how you, claude code, should operate.

## General Guidelines
You should always ensure that code is prepared properly for production before attempting to create any PR.
To do this you should:
  - ensure you run all unit tests
  - run all code quality tools available in this repo (think: linters, type checkers etc.)
  - run all checks available in the pr review toolkit (we want to proactively address these).

And you should create an action plan for all findings which include steps to resolve them.
Then STOP.  
Ensure you have followed the action plan steps to put it in the appropriate folder.
Ask the human developer to review the action plan (just link them to the created file) and wait for them to confirm
it looks good before proceeding.

Only once you have received explicit instructions to proceed should you follow all the standard steps to implement this action plan.