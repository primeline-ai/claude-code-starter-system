---
description: Show context window usage with visual bar and recommendations
---

# /context-stats

Show how much of your context window is currently used, with a visual indicator and action recommendations.

## Workflow

### Step 1: Read Context Percentage

Run this bash command to find the latest context file:

```bash
ls -t /tmp/claude-context-pct-*.txt 2>/dev/null | head -1 | xargs cat 2>/dev/null
```

If the command returns nothing or fails, skip to the "Not Available" fallback at the bottom.

Store the result as `{pct}` (an integer 0-100).

### Step 2: Generate Visual Bar

Calculate the filled and empty portions for a 30-character wide bar:

```
filled = round(pct * 30 / 100)
empty  = 30 - filled
bar    = "█" * filled + "░" * empty
```

### Step 3: Determine Status

| Range | Status | Label |
|-------|--------|-------|
| 0-59% | Healthy | Healthy |
| 60-79% | Warning | Warning |
| 80-100% | Critical | Critical |

### Step 4: Render the Dashboard

```
╔═══════════════════════════════════════════════════════╗
║            CONTEXT WINDOW USAGE                       ║
╠═══════════════════════════════════════════════════════╣
║ {bar}  {pct}%                                        ║
║                                                       ║
║ Status: {status label}                               ║
╠═══════════════════════════════════════════════════════╣
║ {recommendations}                                     ║
╚═══════════════════════════════════════════════════════╝
```

### Step 5: Recommendations by Status

**Healthy (0-59%):**
```
CAPACITY
  Good capacity for complex tasks
  Multi-step operations safe
```

**Warning (60-79%):**
```
RECOMMENDATIONS
  Consider running /handoff soon
  Avoid starting new complex tasks
  Current work can continue safely
```

**Critical (80-100%):**
```
ACTION REQUIRED
  Run /handoff now to save your session
  Start a new session for fresh context
  Use: continue - to resume where you left off
```

---

## Not Available Fallback

If no context file was found, output:

```
Context percentage not available. Check your statusline configuration.

To enable context tracking, add the statusline script to your shell config.
See the plugin README for setup instructions.
```

---

## Plain Text Triggers

- "context stats"
- "context usage"
- "how full is context"
- "token usage"
- "how much context left"
