---
description: Create a session handoff document for continuity across sessions
---

# /handoff

Create a structured handoff document that captures what happened this session so the next session can pick up exactly where you left off.

## Workflow

### Step 1: Summarize the Session

From the current conversation, identify:
- What was accomplished (concrete actions and results)
- Key decisions made and the reasoning behind them
- What is still open or unfinished
- Any blockers encountered

If the conversation has limited history, note what you can observe from the context.

### Step 2: Read Project Context

Read `.claude/memory/projects/{active}.json` to get the project name and current phase.

If the file does not exist, use "unknown" for project name and phase.

### Step 3: Create the Handoff File

Determine a short topic slug from the session focus (2-3 words, kebab-case).

Create the file at:
```
.claude/handoffs/{YYYY-MM-DD}-{topic-slug}.md
```

Use this structure:

```markdown
# Handoff: {YYYY-MM-DD} - {Topic}

**Project:** {name}
**Phase:** {phase}
**Date:** {YYYY-MM-DD}

## Session Summary

{2-4 sentences describing what was accomplished this session}

## Decisions Made

- **{Decision}:** {Reasoning in one sentence}
- {repeat for each key decision, or "None" if no decisions were made}

## Open Items

- {Task or question that still needs attention}
- {repeat for each open item, or "None"}

## Next Session

{One specific, concrete action to take at the start of the next session.
Be precise - "Continue building the auth flow starting with the token refresh endpoint"
is better than "Continue working on auth."}

## Blockers

{Anything currently preventing progress, or "None"}
```

### Step 4: Update Project Memory

Read `.claude/memory/projects/{active}.json`.

If the file exists, add a new entry to the `progress` array:

```json
{
  "date": "{YYYY-MM-DD}",
  "summary": "{one sentence from Session Summary}",
  "next": "{Next Session action}"
}
```

Write the updated file back.

If the file does not exist, skip this step.

### Step 5: Show the Handoff

Display the full handoff content to the user, then add:

```
Saved to: .claude/handoffs/{filename}

To continue next session, say: "continue" or share this file with @.claude/handoffs/{filename}
```

---

## Plain Text Triggers

- "create a handoff"
- "end of session"
- "save session"
- "wrap up"
- "switching context"
