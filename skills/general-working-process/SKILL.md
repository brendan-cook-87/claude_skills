# General Working Process

## Skill Style
Generic guideline — this skill defines how Claude Code should operate whenever asked to work on any new feature or task.

## Core Principle

Getting the **Brainstorm** and **Plan** phases right is critical. Mistakes or shallow thinking in these early phases compound into wasted implementation effort, wrong solutions, and costly rework. Invest heavily in upfront reasoning before writing any code.

## General Guidelines

- Follow all guidelines to be curious and collaborative. Ask lots of questions. Discuss alternatives.
- Present **multiple options** and discuss tradeoffs for each. The aim is to reach the best possible solution.
- Be clear and concise, and always give final say to the human developer.
- Decide whether the change is minor or needs a design document. **ASK** if unsure.

## Extended Thinking Requirement

**You MUST use extended thinking (deep reasoning / thinking blocks) during the Brainstorm and Plan phases.** These phases demand thorough, structured reasoning that cannot be rushed. Extended thinking ensures you:

- Fully explore the problem space before proposing solutions.
- Identify hidden assumptions, edge cases, and risks early.
- Evaluate multiple design alternatives with genuine depth rather than surface-level comparisons.
- Produce higher-quality design documents and action plans that hold up during implementation.

Specifically:
- During **Brainstorm**: Use extended thinking to reason through the problem domain, consider multiple architectural approaches, weigh tradeoffs, and identify questions you need to ask the developer. Think through what could go wrong with each approach before presenting options.
- During **Plan**: Use extended thinking to break work into the right granularity, reason about task dependencies and ordering, anticipate implementation challenges, and define precise acceptance criteria. Think through the test plan thoroughly — what inputs, edge cases, and failure modes need to be covered.

Do **NOT** skip or shortcut extended thinking in these phases. It is better to spend more time reasoning upfront than to rush into a flawed design or plan.

## Working Process

1. **Project Setup Check** — **STOP.** Before anything else, verify that the project's `CLAUDE.md` contains a `## Project Tooling` section documenting test, lint, and type-check commands. If it does not, you MUST run the **project-setup** skill first and get developer confirmation before proceeding. Do **NOT** skip this step.
2. **Brainstorm** *(extended thinking required)* — Understand the context of the request. Use deep reasoning to explore the problem space thoroughly. Discuss the problem, ask questions, present multiple options and tradeoffs. Always use the **writing-design-docs** skill to produce a quality design document. Do **NOT** proceed until the developer accepts the design.
3. **Plan** *(extended thinking required)* — Once the design is accepted, use deep reasoning to create a thorough, well-structured action plan in accordance with the **writing-action-plans** skill. Think carefully about task breakdown, dependencies, acceptance criteria, and test coverage. Do **NOT** attempt any implementation until the developer explicitly approves the action plan.
4. **Implement** — Once the developer gives explicit approval, execute the agreed-upon plan in accordance with the **executing-action-plans** skill.
5. **Prepare for Production** — After implementation is complete, follow the **preparing-code-for-production** skill to ensure the code is production-ready before moving on.
6. **Create a PR** — Once the code has been prepared for production, follow the **creating-a-pr** skill to open a pull request.
