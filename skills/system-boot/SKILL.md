---
name: system-boot
description: Auto-loads project context at session start. Reads memory, shows project status, establishes session guidelines for memory, context, delegation, and handoffs.
---

# System Boot - Context Loader + Session Guidelines

You are loading the project context for the current session. Follow these steps in order, then apply the session guidelines throughout.

---

## Part 1: Context Loading

### Step 1: Read the Memory Index

Read `.claude/memory/index.json` to get the active project name.

If this file does not exist, skip to the "No Memory Found" fallback at the bottom.

### Step 2: Read Project State

Read `.claude/memory/projects/{active}.json` where `{active}` is the project name from Step 1.

This file contains the project goals, current phase, progress log, and any known blockers.

If this file does not exist, skip to the "No Memory Found" fallback.

### Step 3: Present Context Block

Output a compact context block in this format:

```
Project: {name} | Phase: {phase}
Last progress: {summary of the most recent entry in the progress array}
Next step: {suggested_next from the latest progress entry, or the first unfinished goal}
Known blockers: {any blocking issues listed in the blockers field, or "none"}
```

Rules for the context block:
- Keep it under 200 tokens total
- Summarize progress entries - do not paste them verbatim
- If there are multiple goals, show only the primary one
- If blockers is empty or missing, show "none"

### Step 4: Check for Recent Handoff

Look for the most recent file in `.claude/handoffs/` (sort by filename, newest first).

If a handoff file exists:
- Read it
- Add a brief summary to the context block:
  ```
  Last handoff: {date from filename}
  Next action: {content from the "Next Session" section of the handoff}
  ```

If no handoff files exist, skip this step.

### Step 5: Ready to Work

After showing the context block, say one sentence about what you are ready to help with, based on the next step from the handoff or the latest progress entry.

Example: "Ready to continue with {next step}. What would you like to tackle first?"

---

## Part 2: Session Guidelines

These guidelines apply throughout the entire session, not just at boot.

### Memory Updates

| Event | Action |
|-------|--------|
| Task completed | Append to `progress` array in `.claude/memory/projects/{active}.json` |
| Feature finished | Update status to "done" in project file |
| Error or setback | Append to `failures` array |
| Blocker found | Update `blockers` field |

Progress entry format:
```json
{"date": "YYYY-MM-DD", "action": "what was done", "result": "outcome", "next": "next step"}
```

Failure entry format:
```json
{"date": "YYYY-MM-DD", "what": "what broke", "why": "root cause", "learned": "lesson"}
```

### Context Awareness

| Context % | Status | Action |
|-----------|--------|--------|
| < 60% | Healthy | Continue working normally |
| 60-79% | Warning | Finish current task, avoid starting large new tasks |
| 80%+ | Critical | Run /handoff now, continue in a new session |

When context is high, delegate work to sub-agents. They work in separate context windows - the work still happens but your main context stays clean.

### Delegation Basics

Use a sub-agent when the task involves:
- 2+ files to read or modify
- Bulk operations (rename, refactor, search)
- Research or learning something new
- Code review or analysis
- Exploration ("find all X", "how does Y work?")

Model selection:
- Search, exploration, simple lookups: haiku (fast, cheap)
- Implementation, debugging, review: sonnet (capable, default)
- Highly complex work (7+ complexity): keep in main context

Do NOT delegate: production/deploy operations, destructive operations, secrets/passwords, or when the user wants to see work step-by-step.

After delegation, always verify: Does it work? Were requirements met? Any missed edge cases?

### End-of-Session Checklist

Before closing a session:
1. **Log progress** - append to `progress` array in the project file
2. **Update state** - update `phase`, task statuses, and `blockers`
3. **Log failures** - if anything went wrong, append to `failures` array
4. **Run /handoff** - creates a handoff file for the next session

Resuming: say "continue" at session start. This skill reads the handoff automatically.

---

## No Memory Found - First-Time Onboarding

If `.claude/memory/index.json` does not exist, this is a first-time user. Give them a guided welcome:

```
Welcome to the Claude Code Starter System!

This plugin gives you session memory - so I remember where you left off between sessions.

Here's how it works:
1. Run /project-status now to set up your project
2. Work normally - I'll track progress and blockers automatically
3. Run /handoff before closing - this saves your state for next time
4. Next session, say "continue" - I'll pick up where you left off

After a few sessions, your memory will look like this:

  Project: TaskFlow | Phase: building
  Last progress: Deployed to staging, auth and real-time updates working
  Next step: Fix magic link redirect, then deploy to production
  Known blockers: Supabase site URL still set to localhost

Use /remember to save solutions, gotchas, and decisions as you work.
They persist forever and help me avoid repeating your past mistakes.

Ready? Run /project-status to get started.
```

Do not attempt to create any files during this skill. The /project-status command handles initialization.

---

## Rules

- English only in all output
- Never reference external systems, knowledge graphs, or experience databases
- Never dump raw JSON - always summarize
- Never create or modify files during the context loading steps (Part 1)
- If any field is missing from the JSON, use "unknown" or skip that line gracefully
