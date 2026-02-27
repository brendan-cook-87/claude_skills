---
name: project-setup
description: >
  Discover and record a project's tooling, explore the codebase, and ensure the README is up to date.
  Use when first working on a project whose CLAUDE.md does not yet have a Project Tooling section,
  or when the developer asks to refresh project configuration.
  This is a prerequisite for all other work — complete project setup before starting any task.
---

# Project Setup

Discover and record a project's tooling so that all other skills can reference concrete commands, then deep-dive into the codebase and ensure the README accurately represents the project.

## Discovery Process

1. **Scan the project root** for configuration files that indicate which tools are in use. Look for (but don't limit to):

   **Node / TypeScript:**
   - `package.json` — check `scripts` for test, lint, typecheck, and format commands
   - `tsconfig.json` — TypeScript compiler
   - ESLint config (`.eslintrc.*`, `eslint.config.*`)
   - Prettier config (`.prettierrc.*`)
   - Test runner config (`jest.config.*`, `vitest.config.*`)

   **Python:**
   - `pyproject.toml` — check for pytest, ruff, mypy, black, isort, pyright sections
   - `setup.cfg`, `tox.ini`, `Makefile`
   - `.flake8`, `mypy.ini`, `.mypy.ini`, `pyrightconfig.json`

   **General:**
   - `Makefile` — check for test, lint, check targets
   - CI config (`.github/workflows/*.yml`, `.gitlab-ci.yml`) — useful for confirming which commands the project already runs

2. **Present findings** to the developer. List each discovered tool with:
   - The tool name and version (if determinable).
   - The command to run it.
   - The config file it was discovered from.

3. **Ask the developer to confirm** which tools should be used and the exact commands to run them. Do **not** assume — the developer may have preferences that differ from what's in config.

## Project Ownership Discovery

1. **Search for project ownership metadata** in common locations. Look for `ath:owner` and `ath:product` fields in:
   - `package.json` (in custom fields or metadata sections)
   - `pyproject.toml` (in tool sections or custom fields)
   - `README.md` or other documentation files
   - CI config files (`.github/workflows/*.yml`, `.gitlab-ci.yml`)
   - Any `.metadata`, `OWNERS`, or similar organizational files
   - Root-level config files

2. **If ownership info is not found**, ask the developer to provide:
   - **Product name** — which product/service this project belongs to
   - **Owner** — the team or person responsible for this project

## Persisting to CLAUDE.md

Once the developer confirms, update the project's `CLAUDE.md`. If `CLAUDE.md` does not exist, create it. If sections already exist, update them rather than duplicating.

### Global Skills Reference

Add a `## Working Process` section at the **top** of `CLAUDE.md` that points Claude to the globally installed skills:

```markdown
## Working Process

Always read and follow the guideline and workflow skills defined in `~/.claude/skills/` before starting any work. These skills define the required working process for this project — including how to brainstorm, plan, implement, test, and create pull requests.
```

### Project Ownership

Add a `## Project Ownership` section with the discovered or confirmed ownership information:

```markdown
## Project Ownership

- **Product:** `<product-name>`
- **Owner:** `<owner-name>`
```

### Project Tooling

Add a `## Project Tooling` section with the confirmed commands:

```markdown
## Project Tooling

### Tests
- `<command>` — <description>

### Linting
- `<command>` — <description>

### Type Checking
- `<command>` — <description>

### Formatting
- `<command>` — <description>
```

- Only include sections for tools that are actually present in the project.
- Use the exact commands the developer confirmed.

## Project Deep Dive

After tooling discovery, perform a thorough exploration of the codebase to build a clear picture of the project.

### Exploration

1. **Understand the project structure:**
   - Directory layout and module organization.
   - Entry points and main execution flows.
   - Key components and how they interact.
   - External integrations (APIs, databases, message queues, etc.).
   - Key dependencies and their roles.

2. **Present a summary** of findings to the developer. Highlight anything unclear or surprising — this is a chance to learn the project's intent, not just its structure.

### README.md

1. Check if `README.md` exists at the project root.

2. **Propose an outline** to the developer before writing. A good README should cover whichever of the following are relevant:
   - **Project name and purpose** — what the project does and why it exists.
   - **Architecture overview** — high-level description of components and their relationships.
   - **Directory structure** — brief guide to where things live.
   - **Key technologies** — languages, frameworks, and significant libraries.
   - **Getting started** — prerequisites, installation, and how to run the project.
   - **Development** — how to run tests, lint, and other workflows (reference the commands persisted to `CLAUDE.md`).

   Only include sections that are relevant. Do not pad the README with empty or boilerplate sections.

3. **Ask the developer to confirm** the outline and flag any sections to add, remove, or adjust.

4. **Generate diagrams** where they add genuine clarity (architecture, data flow, component relationships, etc.):
   - Write Mermaid source to `.claude/diagrams/<name>.mmd`.
   - Render to `.claude/diagrams/<name>.png` — if the renderer is not available, stop and ask the developer to install it.
   - Embed the rendered `.png` in the README.
   - Do **not** generate diagrams for the sake of it — only where they help a new contributor understand the project faster than text alone.

5. **Create or update** `README.md` with the confirmed content.
   - If a README already exists, review it and propose specific improvements rather than rewriting from scratch.
   - Preserve any existing content the developer wants to keep.
