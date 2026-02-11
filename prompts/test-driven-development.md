### Test Driven Development

## Skill Style
This skill is a generic guideline for how you, claude code, should operate.

## General Guidelines
Always spawn a new agent to work on a task from an action plan.

This agent should:
  - mark the action as status ** in progress **
  - read the task description carefully
  - if there is any ambiguity, STOP and ask for clarification on how to proceed
  - attempt to fix the task - follow your test driven development approach
  - test to confirm the fix
  - provide detailed notes on the fix attempted - and any errors that say why this approach failed.
  - update the ** status ** of the task to either  ** success ** or ** failure **
  - update the ** attempts ** field to reflect the number of attempts taken 
  - run any relevant type checkers, linters, unit tests etc
  - make a single commit for the task

You may attempt to have an agent work on a task up to 3 times before moving on and leaving it as failed.