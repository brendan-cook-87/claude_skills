# Project Setup

## Skill Style
Generic guideline — this skill defines how Claude Code should discover and record a project's tooling so that all other skills can reference concrete commands. Run this skill once when first working on a project, or when the developer asks to refresh the project configuration.

**This skill is a blocking prerequisite for all other work.** If you are about to start a task, STOP and complete project setup first. Step 1 of the **general-working-process** requires this.

## When to Run

- **STOP.** Before starting any work on a project whose `CLAUDE.md` does not yet document test, lint, and type-check commands. You MUST run this skill first.
- When the developer explicitly asks to re-discover or update project tooling.

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

## Persisting to CLAUDE.md

Once the developer confirms, update the project's `CLAUDE.md` with a `## Project Tooling` section. Use the following format:

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
- If `CLAUDE.md` already has a `## Project Tooling` section, update it rather than duplicating it.
- If `CLAUDE.md` does not exist, create it with this section.
