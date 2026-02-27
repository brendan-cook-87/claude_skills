---
name: hello-claude
description: >
  Session initialization skill to ensure project setup is complete and working process is understood.
  Run at the start of each session to establish proper context and workflow.
---

# Hello Claude

Initialize a new Claude Code session by verifying project setup and establishing the working process context.

## Session Initialization Process

### 1. Project Setup Verification

**Check CLAUDE.md existence and completeness:**

1. **Verify CLAUDE.md exists** at the project root
2. **Check required sections** are present:
   - `## Working Process` - references to `~/.claude/skills/`
   - `## Project Ownership` - product and owner information
   - `## Project Tooling` - test, lint, type-check, format commands

3. **If CLAUDE.md is missing or incomplete:**
   - Inform the user that project setup is required before starting any work
   - Recommend running the **project-setup** skill first
   - **STOP** - do not proceed until project setup is complete

### 2. Working Process Familiarization

**Review and summarize the established working process:**

1. **Read the global skills** referenced in CLAUDE.md Working Process section
2. **Summarize the workflow** for the user:
   - **Brainstorm** → **Plan** → **Implement** → **Prepare for Production** → **Create PR**
   - Each phase requires explicit developer approval before proceeding
   - Extended thinking is required during Brainstorm and Plan phases
   - Test-driven development is mandatory

3. **Highlight key principles:**
   - Be curious and collaborative - ask questions, present options
   - Challenge assumptions while maintaining collaborative approach
   - Get developer approval at each phase transition
   - Follow organizational standards (naming conventions, security practices)

### 3. Context Gathering

**Understand the current project state:**

1. **Check git status** to see any work in progress
2. **Look for existing design docs** in `.designs/` directory
3. **Look for existing action plans** in `.plans/` directory
4. **Check for any background context** the developer wants to share

### 4. Session Readiness

**Confirm readiness to begin work:**

1. **Summarize findings** about project setup and current state
2. **Ask the developer** what they'd like to work on in this session
3. **Remind about the working process** - starting with brainstorming if it's a new feature/change
4. **Offer to help** with any immediate questions or clarifications

## Example Output

```
🚀 Hello! Starting new Claude Code session.

✅ Project Setup Status:
- CLAUDE.md found with all required sections
- Product: user-service (Owner: platform-team)
- Tooling configured: npm test, npm run lint, npm run type-check

📋 Working Process Reminder:
- Phase-based workflow: Brainstorm → Plan → Implement → Prepare → PR
- Each phase needs your explicit approval before proceeding
- I'll use extended thinking during brainstorming and planning
- Test-driven development is mandatory for implementation

📁 Current Project State:
- Working tree is clean
- No active design docs or plans found
- Ready for new work

What would you like to work on in this session?
```

## When Project Setup is Needed

If project setup is incomplete, provide clear guidance:

```
⚠️  Project Setup Required

CLAUDE.md is missing the Project Tooling section. Before we can start any work,
I need to run the project-setup skill to:

- Discover and confirm your project's tooling (tests, linting, etc.)
- Explore the codebase structure
- Ensure documentation is up to date
- Record organizational ownership information

Would you like me to run the project-setup skill now?
```

## Integration Notes

- This skill should be run **first** in each session
- It serves as a gateway to ensure proper workflow setup
- If project setup is incomplete, **block all other work** until resolved
- Once complete, the developer can proceed with their intended work following the established workflow

## Usage

Developers should run `/hello-claude` at the start of each Claude Code session to ensure proper context and workflow establishment.