---
description: Save a learning, decision, or insight to persistent memory
---

# /remember [type]

Save something worth keeping - a fix that worked, a pattern you want to reuse, a decision you made, or a pitfall to avoid.

## Argument

- `type` (optional): solution, pattern, decision, gotcha

## Workflow

### Step 1: Determine Type

If no type was provided, ask:

```
What do you want to save?
1. solution - An error you fixed (problem + root cause + fix)
2. pattern  - An approach that worked well
3. decision - An architecture or design choice you made
4. gotcha   - A pitfall or trap to avoid next time
```

### Step 2: Ask Type-Specific Questions

Keep this conversational. Ask all questions for the type at once, not one by one.

**Solution:**
- What was the problem or error?
- What was the root cause?
- What fixed it?

**Pattern:**
- What is this pattern called?
- When should it be used?
- How is it applied?

**Decision:**
- What question was decided?
- What was decided, and why?
- What alternatives were rejected?

**Gotcha:**
- What is the pitfall?
- Why is it a problem?
- What is the correct approach?

### Step 3: Generate Tags

Generate 3-5 tags based on:
- The active project (if known from `.claude/memory/index.json`)
- Technology or tool mentioned
- Keywords from the description

Format tags as lowercase slugs: `next-js`, `api-auth`, `caching`

### Step 4: Save the Learning

Create the file at:
```
.claude/memory/learnings/{type}-{YYYY-MM-DD}-{short-slug}.md
```

Where `{short-slug}` is a 2-3 word kebab-case summary of the content.

File format:

```markdown
---
date: {YYYY-MM-DD}
type: {type}
tags: [{tag1}, {tag2}, {tag3}]
project: {active project name, or "general" if unknown}
---

# {Title summarizing what was learned}

## Context

{Brief description of where/when this came up}

## {Type-specific content}

{The actual content based on what was answered in Step 2}
```

For each type, use these section headers inside the file:

- **solution**: `## Problem`, `## Root Cause`, `## Fix`
- **pattern**: `## When to Use`, `## How to Apply`
- **decision**: `## Decision`, `## Reasoning`, `## Rejected Alternatives`
- **gotcha**: `## The Pitfall`, `## Why It's a Problem`, `## Correct Approach`

### Step 5: Confirm

```
Saved! Type: {type} | Tags: {tags} | File: {filename}
```

---

## Plain Text Triggers

- "remember this"
- "save this solution"
- "note this pattern"
- "log this decision"
- "save this gotcha"
