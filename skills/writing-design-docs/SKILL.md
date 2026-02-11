# Writing Design Documents

## Skill Style
Generic guideline — this skill defines how Claude Code should operate. Always write a design document for any large piece of work, especially when there are architectural decisions to make.

## General Guidelines

- Write a design document **before** starting any large piece of work.
- If you are unsure whether the work warrants a design document, **ask** the developer if they would like to see a design before moving on to planning.
- Focus on **business context first**, then decide a technical approach.
- Do **not** go too deep on implementation details — that belongs in the action plan.
- Record all user questions and answers in the design document.

## Storage

- Store design documents in `.designs/<branch>/<topic>.md`.
- Store design diagrams in `.diagrams/`:
  - `.diagrams/<topic>_<diagram_type>.mmd` — Mermaid source
  - `.diagrams/<topic>_<diagram_type>.png` — rendered image

## Diagrams

- Use **Mermaid** format for all diagrams.
- After creating the `.mmd` file, attempt to render it to `.png`.
- If the Mermaid renderer is not available, **stop and ask** the user to install it before continuing.
- Link the rendered `.png` image into the design document.
- Use diagrams where appropriate to explain context, relationships, and flows.

## What Makes a Good Design Document

- Records all user questions and answers.
- Leads with business context — why this work matters and what problem it solves.
- Proposes a technical approach informed by that context.
- Uses diagrams where appropriate to explain context.
- Stays high-level — avoids deep implementation details.
