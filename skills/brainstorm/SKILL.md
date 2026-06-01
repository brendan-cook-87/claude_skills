---
name: brainstorm
description: Guided brainstorming that produces a fully-specified epic — explores the problem, proposes solutions, breaks work into tickets, and details each ticket's action plan. All upfront, in stages, with engineer approval at each gate.
---

# /brainstorm

Guided brainstorming skill that produces a complete, implementation-ready epic. Works through three stages in order, each requiring engineer approval before proceeding to the next. No implementation happens until all three stages are complete.

Each stage artefact goes through a **review cycle** (creator → reviewer → architect) before being presented to the engineer. This ensures the engineer reviews polished, implementation-ready output rather than first drafts.

## Usage

```
/brainstorm                     # Start a new brainstorming session
/brainstorm user notifications  # Start with a topic in mind
```

## Output Structure

All artefacts are stored in a self-contained folder:

```
./epics/<topic>/
├── design.md              (Stage 1 — problem, approach, architecture)
├── epic.md                (Stage 2 — ticket list with scope + acceptance criteria)
└── tickets/
    ├── 01-<ticket-slug>.md  (Stage 3 — full action plan per ticket)
    ├── 02-<ticket-slug>.md
    └── ...
```

---

## Stage 1: Epic Scope & Approach

The goal is to fully understand the problem and agree on a solution before breaking work down. This stage produces `design.md`.

### 1.1 Discovery

Start by asking what the user wants to build or solve. If they provided a topic as an argument, use it as the starting point but still clarify.

Ask follow-up questions **one at a time** using the AskUserQuestion tool to understand:

- **Purpose** — What problem does this solve? Who is it for?
- **Success criteria** — How will you know this works? What does "done" look like?
- **Scope** — What is in scope and what is explicitly out of scope?
- **Constraints** — Are there technical constraints, deadlines, or dependencies?
- **Existing context** — Is there existing code, infrastructure, or patterns this needs to fit into?

#### Question Guidelines

- **One question at a time** — never ask multiple questions in a single message
- **Multiple choice where possible** — provide 2-4 concrete options using AskUserQuestion rather than open-ended questions
- **Keep it focused** — each question should build on the previous answer
- **Stop when clear** — do not over-question; move on once you have enough context to propose solutions
- **Read the codebase** — if the user references existing code, read it to inform your questions rather than asking the user to explain it
- **Record every question and answer** — this log is included in the design document

### 1.2 Summarise Understanding

Before proposing solutions, present a brief summary:

```
## Problem Summary

**What:** <one sentence>
**Why:** <one sentence>
**Success criteria:** <bulleted list>
**Scope:** <in/out>
**Constraints:** <if any>
```

Ask the user to confirm this is correct before continuing. If they correct anything, update and re-confirm.

### 1.3 Propose Solutions

Present **at least two** distinct approaches. For each:

```
### Option A: <name>

<How it works — 2-4 sentences>

| Pros | Cons |
|------|------|
| ...  | ...  |

### Option B: <name>

<How it works — 2-4 sentences>

| Pros | Cons |
|------|------|
| ...  | ...  |

**Recommendation:** Option A because <reason>.
```

Use AskUserQuestion to let the user pick their preferred approach (or suggest a combination).

### 1.4 Create the Design Document

Once the user selects an approach, create the design document internally. Do **not** present it to the engineer yet — it goes through the review cycle first (step 1.5), then is presented polished in the stage gate (step 1.6).

#### Design Document Format

```markdown
# Design: <topic>

**Date:** YYYY-MM-DD
**Status:** Draft

## Discovery

| # | Question | Answer |
|---|----------|--------|
| 1 | <question asked> | <user's response> |
| 2 | ... | ... |

## Overview

<2-3 sentences summarising the chosen approach>

## Architecture

<ASCII diagram showing major components and how they connect>

## Components

| Component | Responsibility | Inputs | Outputs |
|-----------|---------------|--------|---------|
| ...       | ...           | ...    | ...     |

## Data Flow

<ASCII diagram or numbered list showing how data moves through the system>

## Error Handling

| Error Case | Handling Strategy |
|------------|-------------------|
| ...        | ...               |

## Testing Strategy

| What to Test | How | Type |
|-------------|-----|------|
| ...         | ... | unit/integration/e2e |

## Open Questions

- <any unresolved items>
```

#### Diagram Guidelines
- Use ASCII box-and-arrow diagrams (they render well in markdown)
- Keep diagrams simple — show the key relationships, not every detail
- Label all arrows with what flows between components

### 1.5 Review Cycle

Before presenting the design to the engineer, run the planning review cycle:

1. **Planning Reviewer** — dispatch a subagent with `templates/planning-reviewer.md` + the design document. The reviewer assesses for: unstated assumptions, missing components, unclear acceptance criteria, feasibility issues, and scope creep.
2. **Planning Architect** — if the reviewer produces findings, dispatch a subagent with `templates/planning-architect.md` + the design document + the reviewer's findings. The architect confirms or refutes each finding.
3. **Incorporate confirmed findings** — update the design document to address all findings confirmed at Important or Blocking severity. Findings requiring user input are collected for the engineer review step.
4. **Repeat** until the reviewer returns PASS (max 3 cycles).

If the reviewer identifies findings marked "Requires user input: YES" and the architect confirms they genuinely need human direction, **present these questions to the engineer** using AskUserQuestion before finalising the design.

### 1.6 Stage Gate

Once the review cycle passes, present the polished design to the engineer for final approval. Present section by section in this order:

1. Discovery — confirm the Q&A log is accurate and complete
2. Overview and Architecture — confirm the high-level structure
3. Components and Data Flow — confirm the details
4. Error Handling and Testing Strategy — confirm the approach
5. Open Questions — resolve any remaining unknowns

Include any user escalation questions that were identified during the review cycle. After each section, ask if the engineer has feedback or wants changes.

Once approved:
- Save to `./epics/<topic>/design.md`
- Create the `./epics/<topic>/` directory if it doesn't exist
- Inform the user: "Design approved and saved. Moving to Stage 2: ticket breakdown."

**Do NOT proceed to Stage 2 without explicit approval of the design.**

---

## Stage 2: Ticket Breakdown

The goal is to break the approved design into discrete, implementable tickets. Each ticket is a self-contained unit of work an agent team can pick up without needing the full epic context. This stage produces `epic.md`.

### 2.1 Define Tickets

Break the design into tickets. Each ticket should:

- Deliver a **single, testable outcome** — one feature, one integration, one component
- Be **self-contained** — an agent reading only the ticket should know what to build, test, and deliver
- Have **clear boundaries** — no overlap with other tickets
- Have **verifiable acceptance criteria** — concrete, measurable outcomes (not "works correctly")
- Note **dependencies** — which tickets must complete before this one can start

Order tickets so that:
- Foundational pieces come first (data models, core logic)
- Dependent work comes after its prerequisites
- Integration/wiring tickets come after the components they connect

### 2.2 Epic Document Format

```markdown
# Epic: <topic>

**Date:** YYYY-MM-DD
**Design:** [design.md](design.md)
**Status:** Planning
**Total Tickets:** N

## Goal

<1-2 sentences — what this epic delivers when all tickets are complete>

## Epic Acceptance Criteria

<bulleted list — how you know the entire epic is done, not just individual tickets>

## Tickets

### Ticket 1: <title>

- **Scope:** <what this ticket delivers — one paragraph max>
- **Acceptance Criteria:**
  - <specific, testable criterion>
  - <specific, testable criterion>
- **Dependencies:** None / Ticket N
- **Estimated Complexity:** Small / Medium / Large

### Ticket 2: <title>

<same structure>

## Dependency Graph

<ASCII diagram or ordered list showing which tickets depend on which>

## Open Questions

- <any remaining unknowns that could affect ticket scope>
```

### 2.3 Present and Review

Present the ticket breakdown in logical groups (e.g., "foundation tickets", "feature tickets", "integration tickets"). After each group, ask:
- Does this look right?
- Should any tickets be split further or merged?
- Are the acceptance criteria specific enough?
- Are any dependencies missing?

Incorporate feedback before presenting the next group.

### 2.4 Review Cycle

Before presenting the epic breakdown to the engineer, run the planning review cycle:

1. **Planning Reviewer** — dispatch a subagent with `templates/planning-reviewer.md` + `epic.md` + `design.md` (for context). The reviewer assesses for: unclear acceptance criteria, overlapping ticket boundaries, missing tickets, incorrect dependencies, and implementation readiness.
2. **Planning Architect** — if findings are produced, dispatch with `templates/planning-architect.md` + the artefacts + findings. Confirms or refutes each finding.
3. **Incorporate confirmed findings** — update the epic breakdown. Collect any user escalations.
4. **Repeat** until PASS (max 3 cycles).

### 2.5 Stage Gate

Once the review cycle passes, present the polished epic breakdown to the engineer:
- Present ticket groups as described in 2.3
- Include any user escalation questions from the review cycle
- Save to `./epics/<topic>/epic.md` once approved
- Create the `./epics/<topic>/tickets/` directory
- Inform the user: "Epic breakdown approved. Moving to Stage 3: ticket detailing."

**Do NOT proceed to Stage 3 without explicit approval of the ticket breakdown.**

---

## Stage 3: Ticket Detailing

The goal is to produce a complete action plan for each ticket — detailed enough that an agent team can execute it without ambiguity. Each ticket becomes a file in `tickets/`.

### 3.1 Process

For each ticket, in dependency order:

1. **Read the ticket's scope and acceptance criteria** from `epic.md`.
2. **Break the ticket into tasks** following the `writing-action-plans` format.
3. **Define the test plan** — what tests prove each acceptance criterion is met.
4. **Present to the engineer** for review.
5. **Save** once approved.

### 3.2 Ticket Detail Format

Each ticket file is a complete action plan:

```markdown
# Ticket: <title>

**Epic:** [epic.md](../epic.md)
**Date:** YYYY-MM-DD
**Status:** Ready
**Dependencies:** <list or "None">

## Scope

<what this ticket delivers — copied from epic.md>

## Acceptance Criteria

<copied from epic.md — the measurable outcomes>

## Test Plan

| Criterion | Test | Type | Command |
|-----------|------|------|---------|
| <acceptance criterion> | <what test proves it> | unit/integration/e2e | <exact command to run> |

## Tasks

### Task 1: <title>

- **Instructions:** <explicit — file paths, function names, exact change>
- **Acceptance Criteria:** <what "done" looks like for this task>
- **Validation:** <specific command to verify>
- **Status:** Not Started
- **Evidence of Success:**
- **Attempts:** 0
- **Notes:**

### Task 2: <title>

<same structure>

## Status Summary

0/N tasks complete
```

### 3.3 Task Guidelines

- Each task implements **one logical change** — if you write "and also", split it.
- Instructions must be **unambiguous** — include file paths, line numbers where relevant, and the exact change required.
- Validation must be **runnable** — a command, a test assertion, or a specific check. Never "works correctly".
- If you cannot write unambiguous instructions for a task, **ask the user** before saving it.

### 3.4 Review Cycle (Per Ticket)

Before presenting each ticket's detail to the engineer, run the planning review cycle:

1. **Planning Reviewer** — dispatch with `templates/planning-reviewer.md` + the ticket detail + `epic.md` + `design.md` (for context). The reviewer assesses for: task clarity, completeness against acceptance criteria, validation quality, test plan coverage, and task ordering.
2. **Planning Architect** — if findings are produced, dispatch with `templates/planning-architect.md` + artefacts + findings.
3. **Incorporate confirmed findings** — update the ticket detail. Collect any user escalations.
4. **Repeat** until PASS (max 3 cycles).

### 3.5 Presentation

Once the review cycle passes, present each ticket's detail individually. After presenting, ask:
- Are the tasks at the right granularity?
- Are the instructions clear enough for an agent with no context?
- Is anything missing from the test plan?

Include any user escalation questions from the review cycle.

Once approved, save to `./epics/<topic>/tickets/NN-<ticket-slug>.md`.

### 3.6 Completion

After all tickets are detailed and approved:
- Update `epic.md` status to "Ready for Implementation"
- Inform the user: "All tickets detailed and saved. The epic is ready for implementation. Start with ticket 01."

---

## Key Principles

- **Keep it simple** — do not over-engineer the design or the plan
- **Ask, don't assume** — always prefer asking for clarification over deciding things yourself
- **One question at a time** — never batch questions together
- **Multiple choice where possible** — concrete options are faster than open-ended questions
- **Present in stages, review in sections** — never dump large documents at once; seek feedback incrementally
- **Each stage has a gate** — explicit engineer approval required before proceeding
- **Tickets must stand alone** — an agent reading a ticket file should have everything it needs
- **Tasks must be unambiguous** — another agent will execute them; vagueness wastes attempts
- **Strict validation** — every task must have criteria that can be verified with a command
- **Use diagrams** — where it makes sense, provide ASCII diagrams over large text blocks

## Important Rules

- **Always use AskUserQuestion** for questions — do not ask questions in plain text and wait for a response
- **Always save outputs** — design, epic, and tickets to `./epics/<topic>/`
- **Never skip stages** — even if the user seems to know what they want, confirm understanding first
- **Never write code** — this skill produces designs, epics, and action plans, not implementations
- **Respect user decisions** — present options and recommendations, but the user chooses
- **Never proceed past a gate without approval** — the three gates (design, tickets, detail) are mandatory
