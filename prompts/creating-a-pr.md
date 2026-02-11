### Creating a Pull Request

## Skill Style
This skill is a generic guideline for how you, claude code, should operate.

## General Guidelines
When you want to create a pull request for a branch you should:
  - read any designs under .designs/<branch> to understand the context
  - read any action plans under .plans/<branch> to understand the context

Then you may check that the code has been prepared for production.
If it has been prepared for production you may proceed to generate a PR description.
This description should:
 - offer a high level overview
 - include diagrams (formatted to render on github) where useful for context.
 - document clarifying questions asked of the developer - you do not need to document ALL questions, but focus on ones which:
   - clarify the approach taken
   - clarify why certain actions were taken that would be unclear to the reviewer
 - include the test plans documented for new functions:
   - specify which added unit tests confirm which items on the test plan
 - include confirmation that all changes have past relevant code checks (linting, type-checking, security findings etc)
