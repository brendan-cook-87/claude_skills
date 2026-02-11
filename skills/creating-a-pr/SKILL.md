# Creating a Pull Request

## Skill Style
Generic guideline — this skill defines how Claude Code should operate when creating a pull request.

## General Guidelines

- Do **not** create a PR until the code has been prepared for production.
- Before generating a PR description, read all relevant context:
  - Design documents under `.designs/<branch>/`
  - Action plans under `.plans/<branch>/`

## PR Description

The PR description should include:

### High-Level Overview
- A clear summary of what the change does and why.

### Diagrams
- Include diagrams where useful for context.
- Format diagrams so they **render on GitHub**.

### Clarifying Questions
- Document questions asked of the developer that help the reviewer understand the PR. Focus on questions that:
  - Clarify the **approach taken**.
  - Clarify why certain actions were taken that would otherwise be **unclear to the reviewer**.
- Do **not** document every question — only the ones that add context for reviewers.

### Test Plans
- Include the test plans documented for new functions.
- Specify which added **unit tests** confirm which items on the test plan.

### Code Quality Confirmation
- Confirm that all changes have passed relevant code checks: **linting**, **type-checking**, **security findings**, etc.
