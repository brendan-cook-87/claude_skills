# Executing Action Plans

## Skill Style
Generic guideline — this skill defines how Claude Code should orchestrate the execution of tasks from an action plan. It is self-contained — covering the full execution protocol, structured output templates, and compliance rules.

## Overview

Every action plan (whether inside an epic ticket or standalone) is executed through a **three-agent protocol** with **mandatory structured output templates**. The protocol defines when to dispatch each role. The templates define what each agent must produce. The compliance rules define how the orchestrator validates output.

## Relationship to Other Skills

- **`writing-action-plans`** — defines the task format. Tasks come from action plans written to that format.
- **`brainstorm`** — produces epic tickets containing action plans. This skill executes them.
- **`test-driven-development`** — the implementation template enforces TDD. Agents following the implementation template will inherently follow TDD.

## Execution Protocol

### Roles

| Role | Purpose | When dispatched |
|------|---------|-----------------|
| Implementer | Executes tasks — writes code, researches, investigates, reviews, or summarises | Per-task |
| Reviewer | Assesses all completed work against ticket acceptance criteria | Per-ticket (after all tasks complete) |
| Architect | Corroborates reviewer findings — confirms or refutes | Per-ticket (only if reviewer produces findings) |

### Agent Selection

- Choose the appropriate `a-` agent type for each role based on the task domain (e.g., `a-backend` for Lambda implementation, `a-data` for Athena queries, `a-analytics` for dbt models).
- **Implementer and reviewer must be different agent types** where possible to ensure genuinely fresh perspective.
- The architect is always `a-architect`.

### Execution Modes

**Team Mode (Default):**
Uses three agent roles. Implementer builds per-task with TDD discipline. After all tasks in a ticket complete, the reviewer checks the full implementation against acceptance criteria. The architect corroborates any findings. A fresh implementer fixes confirmed issues. Cycle repeats until review passes.

**Solo Mode (Opt-in):**
Use only when the user explicitly requests it ("just implement", "no review needed") or for trivially small plans (1-2 tasks, mechanical changes). Skips the review cycle — implements and commits directly.

---

## The Implementation Loop (Per Task)

Process each pending task sequentially:

### 1. Classify the Task

Determine the task type to select the appropriate template:

| Type | Signals | Template |
|------|---------|----------|
| Implementation | Creates/modifies code, adds features, fixes bugs | `templates/implementer-implementation.md` |
| Research | Explores options, gathers information, evaluates approaches | `templates/implementer-research.md` |
| Investigation | Diagnoses problems, finds root causes, explains behaviour | `templates/implementer-investigation.md` |
| Review | Assesses quality, security, correctness of existing code | `templates/implementer-review.md` |
| Summarising | Condenses information, creates digests, distills findings | `templates/implementer-summarising.md` |

If a task spans multiple types (e.g., investigate then fix), dispatch as two sequential steps within the same task attempt.

### 2. Dispatch an Implementer

Launch a new agent. One agent per task, one task per agent. The agent prompt must include:

- The full task details (all fields from the action plan)
- The relevant worker template content
- The project's quality check commands (from CLAUDE.md)
- The current attempt number and any failure notes from previous attempts
- Explicit instruction that output must follow the template format

### 3. Validate Compliance

When the agent returns, validate its output follows the template structure (see Compliance Validation below).

- If **non-compliant** → reject and re-dispatch with feedback (up to 2 rejections per attempt).
- If **compliant** → proceed to step 4.

### 4. Assess Task Success

Check whether the task's validation criteria passed:

- If **validation passes** → mark task as `Done`, record evidence of success, commit.
- If **validation fails** → mark attempt as failed, record failure notes.

### 5. Update the Action Plan

After each attempt:
- Set **Status** to `Done` / `In Progress` / `Failed`
- Increment **Attempts**
- **Append** to **Notes** — never overwrite. Include what was changed, what commands were run, any error messages.
- Update the **Status Summary** in the plan header
- If `Done`, populate **Evidence of Success** with actual validation output

### 6. Retry on Failure

If the task failed and attempts are below 3:

1. Spawn a **new** implementing agent with:
   - The original task details
   - **All accumulated failure notes from every previous attempt** (verbatim)
   - Explicit instruction to try a different approach
   - The template again (agents start cold)

If attempts reach 3 without success, mark as `Failed` and move to the next task.

---

## The Review Cycle (Per Ticket)

After all tasks in a ticket are complete (Status = `Done`), run the review cycle.

### 1. Dispatch a Reviewer

Launch a reviewer agent (different type from the implementer). The prompt must include:

- The ticket's acceptance criteria
- All files created or modified during implementation
- The quality check commands
- The `templates/reviewer.md` template
- Instructions to check every acceptance criterion with evidence

### 2. Validate Reviewer Compliance

Check the reviewer's output follows `templates/reviewer.md` format. Reject if non-compliant.

### 3. Handle PASS

If the reviewer reports PASS on all criteria:
1. **Squash commits** — combine all per-task commits for this ticket into a single commit
2. **Move to the next ticket**

### 4. Handle FINDINGS — Architect Corroboration

If the reviewer reports any FINDINGS:

1. Dispatch an architect agent (`a-architect`) with:
   - The reviewer's findings
   - The relevant source files
   - The `templates/architect.md` template
   - Instructions to assess each finding as CONFIRMED or REFUTED

2. Validate architect compliance.

3. The architect classifies confirmed findings as:
   - **FIX NOW** — blocks the ticket, must be resolved
   - **DEFER** — real but acceptable, not worth fixing now

### 5. Fix Confirmed Issues

For each finding classified as FIX NOW:

1. Spawn a **fresh** implementing agent (never reuse the original)
2. Include: the specific finding, the reviewer's evidence, the architect's confirmation, the implementation template, quality check commands
3. The fix agent implements, validates, and commits
4. After all fixes commit → **return to step 1** (spawn a new reviewer)

### 6. Re-review

The cycle continues until the reviewer reports PASS. If a finding cycles more than 3 times (keeps failing), escalate to the user rather than looping indefinitely.

---

## Structured Output Templates

Every agent must follow a role-specific template that defines:
- What structured output they must produce
- What reasoning they must show
- What evidence they must provide
- What format their output must follow

Templates are stored in `templates/` relative to this skill file.

### Template Assignment

| Role | Task Type | Template |
|------|-----------|----------|
| Implementer | Code changes, features, fixes | `templates/implementer-implementation.md` |
| Implementer | Research, exploration | `templates/implementer-research.md` |
| Implementer | Debugging, root cause analysis | `templates/implementer-investigation.md` |
| Implementer | Code review, architecture review | `templates/implementer-review.md` |
| Implementer | Condensing, distilling information | `templates/implementer-summarising.md` |
| Reviewer | All task types (per-ticket) | `templates/reviewer.md` |
| Architect | Corroborating findings (per-ticket) | `templates/architect.md` |

---

## Compliance Validation

When an agent returns output, the orchestrator validates it follows the template structure before acting on it. **Non-compliant output is always rejected.**

Output is non-compliant if:
- It does not follow the structured format defined in the template
- Claims are stated without accompanying evidence
- Required sections are missing or empty
- Evidence fields contain deferrals ("will verify later", "should work", "appears to")
- The reasoning chain has gaps (conclusions that don't follow from stated evidence)
- For implementation: TDD evidence is missing (no red/green phase output)
- For research: claims lack sources or cross-references
- For investigation: hypotheses lack testable predictions or test results
- For review: findings lack code references or traced execution paths
- For summarising: key points lack source traceability

### On Rejection

1. Identify specifically which sections are non-compliant and why.
2. Re-dispatch the same agent role with:
   - The rejected output (so the agent can see what to fix)
   - Explicit feedback about what was missing or inadequate
   - The template again (agents start cold — they need it again)
3. A single agent may be rejected up to **2 times** before the attempt is marked failed.

### Rejection Does Not Count as an Attempt

Template compliance rejection is not a task attempt — it means the agent didn't follow instructions. The task attempt counter only increments when an agent produces compliant output that fails validation (the task itself didn't succeed).

---

## Commit Discipline

- **Per-task commits during implementation** — one commit per completed task with a descriptive message
- **Squash per ticket after review passes** — combine all task commits + fix commits into one clean commit
- **Never commit failed attempts** — only commit when validation passes
- **Commit message format:** `<ticket>: <summary of what the ticket delivers>`

---

## Rules

- **Team mode is the default** — always run the review cycle unless the user explicitly opts out
- **One agent per attempt** — spawn a fresh agent for each attempt. Never reuse agents across tasks.
- **Max 3 attempts per task** — after 3 failures, mark as `Failed` and move on.
- **TDD order is mandatory** — verify the problem exists before implementing. If validation already passes, skip.
- **Agents stay focused** — only change what the task describes. No surrounding cleanup or unrelated improvements.
- **Update the plan after every attempt** — never batch updates. The plan file must reflect reality at all times.
- **Append to Notes, never overwrite** — preserve the history of all attempts.
- **Feed failure context forward** — retry agents must see what previous attempts tried and why they failed.
- **Implementer and reviewer are different agent types** — fresh eyes catch what the builder misses.
- **Architect corroborates before fixing** — never trigger fixes based solely on reviewer findings.

---

## Template Maintenance

Templates define the contract between orchestrator and agent. When updating templates:
- Never remove required sections — agents depend on the structure for compliance.
- New sections should be clearly marked as required or optional.
- Test template changes by running a task through the cycle and checking that compliance validation still works.
