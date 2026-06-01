# Creating a Pull Request

## Skill Style
Generic guideline — this skill defines how Claude Code should operate when creating a pull request.

## General Guidelines

- Do **not** create a PR until the code has been prepared for production via the **preparing-code-for-production** skill.
- Before generating a PR description, read all relevant context:
  - Epic folder under `./epics/<topic>/` (design document, epic breakdown, ticket files)
  - Any standalone action plans under `.claude/plans/<branch>/`

## Workflow

1. Generate the PR description markdown (see format below).
2. Create a **draft** PR with the generated description.
3. **STOP.** Provide the PR link to the developer. Do **not** mark it as ready for review.

## PR Description Format

### High-Level Overview
- A clear summary of what the change does and why.

### Diagrams
- Include diagrams where useful for context.
- Embed diagrams as **images** (`.png`) rather than inline Mermaid code blocks, so they render on both GitHub and Bitbucket.

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
