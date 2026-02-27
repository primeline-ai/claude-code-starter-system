---
description: Show current project status or initialize a new project
---

# /project-status

Show the current project status, or set up project memory for the first time.

## Workflow

### Step 1: Check for Existing Memory

Read `.claude/memory/index.json`.

---

### Path A: Memory Exists

Read `.claude/memory/projects/{active}.json` where `{active}` is the project name from the index.

Display the project status:

```
Project: {name}
Phase:   {phase}
Goal:    {primary goal from the goals array}

Recent Progress:
- {most recent progress entry - date + summary}
- {second most recent}
- {third most recent}
(show up to 3 entries, most recent first)

Blockers: {blockers field, or "None"}
```

Then ask: "What would you like to work on?"

---

### Path B: No Memory Found - Initialize

If `.claude/memory/index.json` does not exist, start the initialization flow.

Ask these questions one at a time:

1. "What project are you working on? (This will be the project name)"
2. "What is the primary goal for this project?"
3. "What phase are you in? (planning / building / testing / launching)"

After all three answers, create the memory files:

**`.claude/memory/index.json`:**
```json
{
  "active": "{name-slug}",
  "updated": "{YYYY-MM-DD}"
}
```

Where `{name-slug}` is the project name in lowercase kebab-case (e.g., "My App" becomes "my-app").

**`.claude/memory/projects/{name-slug}.json`:**
```json
{
  "name": "{project name as given}",
  "phase": "{phase}",
  "goals": ["{primary goal}"],
  "progress": [],
  "blockers": []
}
```

Create the `.claude/memory/projects/` directory path as needed.

Confirm with:

```
Project initialized! Memory will persist across sessions.

Project: {name}
Phase:   {phase}
Goal:    {goal}

Use /handoff at the end of each session to save your progress.
Use /remember to save solutions, patterns, and decisions as you work.
```

---

## Plain Text Triggers

- "project status"
- "what project are we on"
- "show project"
- "initialize project"
- "set up memory"
