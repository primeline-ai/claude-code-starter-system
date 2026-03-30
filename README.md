# Claude Code Starter System


![claude-code-starter-system](assets/hero.png)

**Build Systems, Not Sessions**

---

## The Problem

Claude Code sessions are ephemeral. Every new session starts from zero - no memory of what you built yesterday, what decisions you made, what failed last week. You repeat the same context-setting ritual, re-explain your project, and rediscover mistakes you already paid for. Free memory plugins solve one piece of this, but memory alone is not a system.

## The Solution

A complete session management system for Claude Code. Not just memory storage - handoffs, context awareness, delegation guidance, and project tracking working together. Extracted from a 650-node production AI system and distilled to the 20% that delivers 80% of the value.

No MCP servers. No API keys. No external dependencies. Pure markdown and JSON - drop it in and go.

---

## What's Included

**4 Commands**

| Command | What it does |
|---------|-------------|
| `/remember` | Save a learning, decision, or observation to project memory |
| `/handoff` | Create a session continuity document before you close Claude |
| `/project-status` | View current project state or initialize a new project |
| `/context-stats` | See how much of your context window is used |

**1 Skill** (auto-activates at session start)

- `system-boot` - loads project context, establishes session guidelines for memory updates, context awareness, delegation, and end-of-session hygiene

**2 Templates**

- `memory-index.json` - starter structure for your memory index
- `project-template.json` - starter structure for a new project entry

---

## Quick Start

### Session (try it now)

Start Claude Code with the plugin loaded for this session:

```bash
claude --plugin-dir /path/to/claude-code-starter-system
```

### Permanent Install

To load the plugin automatically in every session, add it to your Claude Code settings:

```json
{
  "pluginDirectories": ["/path/to/claude-code-starter-system"]
}
```

Or install via the marketplace once listed there.

Once loaded:

1. Run `/project-status` to initialize your first project
2. Start working - your memory persists across sessions

That's it. No configuration required.

---

## Commands Reference

### `/remember`

Save something worth keeping before it gets lost.

```
/remember The API rate limit is 100 req/min per user - use exponential backoff
/remember Decision: chose Postgres over SQLite because we need concurrent writes
/remember Gotcha: the auth middleware runs before the rate limiter, not after
```

Saves to `.claude/memory/learnings/` as structured markdown with YAML frontmatter. Browse your learnings anytime.

### `/handoff`

Creates a continuity document at session end. Run this before closing Claude.

```
/handoff
```

Outputs a structured summary: what was done, what's next, active blockers, and open decisions. The next session picks this up automatically via `system-boot`.

### `/project-status`

View current project state or set up a new one.

```
/project-status
/project-status
```

Shows: project name, current phase, last progress entry, blockers, and next suggested action.

### `/context-stats`

Check how full your context window is before hitting the limit by surprise.

```
/context-stats
```

Returns a visual indicator and percentage. Useful before starting a large task - if you are at 70%+ consider running `/handoff` first.

---

## How It Works

**Session start**
`system-boot` reads `.claude/memory/index.json` to find your active project, then loads `.claude/memory/projects/{name}.json`. Claude gets oriented before your first message: current phase, last progress, known blockers, next suggested step.

**During work**
`/remember` saves learnings as you go. The `system-boot` guidelines steer Claude toward using sub-agents for exploration and research tasks - keeping your main context clean - and flag when you are approaching context limits.

**Session end**
`/handoff` writes a structured continuity document. It captures what was completed, what is in progress, any decisions made, and what to tackle next.

**Next session**
`system-boot` loads the project state and finds the latest handoff file. Claude picks up where you left off without you re-explaining anything.

---

## What to Expect

The system builds itself through use. Here is what your memory looks like over time:

**Day 1 (after install):**
You run `/project-status`, answer 3 questions, and get an empty project file. That's it. The value is not obvious yet.

**After 3 sessions:**
Your project file has a progress log. Claude opens each session already knowing what you did yesterday and what's next. You stop re-explaining context.

**After 1 week (~8 sessions):**
Your memory looks like this:

```
Project: TaskFlow | Phase: building
Last progress: Deployed to staging, auth and real-time updates working
Next step: Fix magic link redirect, then deploy to production
Known blockers: Supabase site URL still set to localhost

Last handoff: 2026-02-27
Next action: Fix magic link redirect URL in Supabase dashboard settings
```

You have a progress log tracking every session, a failures log that prevents repeating mistakes, learnings saved via `/remember`, and a chain of handoff documents linking each session to the next.

**Check the `examples/` directory** for a complete example project with 8 sessions of progress, 3 failure entries, 3 learnings, and a handoff document. This is what your system looks like after a week of real use.

---

## Directory Structure

After setup, the plugin manages this structure in your project:

```
.claude/
  memory/
    index.json              # Active project pointer
    projects/
      your-project.json     # Per-project state, progress, failures, blockers
    learnings/
      solution-2026-02-22-drag-drop-perf.md   # Saved via /remember
      gotcha-2026-02-24-supabase-rls.md
      decision-2026-02-27-separate-supabase.md
  handoffs/
    2026-02-27-staging-deploy.md   # Session continuity documents (auto-generated)
```

Your existing `.claude/` setup is not touched. The plugin adds to it, does not overwrite.

---

## FAQ

**How is this different from other memory plugins?**

Most memory plugins store facts. This is a system: memory + handoffs + context awareness + delegation guidance. The difference is that when you open a new session, Claude already knows where you are, what failed, and what to do next - without you saying a word.

**Will it conflict with my existing Claude Code setup?**

No. The plugin adds commands and a skill - it does not override anything. If you already have memory or handoff workflows, they continue to work. This adds on top.

**Do I need anything else installed?**

Nothing. No MCP servers, no npm packages, no API keys. Every file is plain markdown or JSON. It works in any Claude Code project on any machine.

**Can I use this across multiple projects?**

Yes. Each project gets its own entry in `.claude/memory/projects/`. Switch active projects by updating `index.json` or running `/project-status` in a project without memory.

**What if I already have a `.claude/` directory?**

The plugin will coexist with your existing setup. The `memory/` subdirectory it uses is unlikely to conflict with anything already there. Check the directory structure above before installing if you want to be sure.

---

## The Ecosystem

This starter system is the foundation. Each tier works independently - no hard dependencies.

```
You're here          You want this            Install this
-----------          -------------            ------------
Raw Claude Code  ->  Session memory       ->  Starter System (free) <- you are here
                 ->  Workflow skills      ->  + Skills Bundle (free)
                 ->  Deep planning        ->  + UPF (free)
                 ->  Deep analysis        ->  + Quantum Lens (free)
                 ->  AI-powered system    ->  + Course (paid)
```

| Component | What It Does | Link |
|-----------|-------------|------|
| **Starter System** | Session memory, handoffs, context awareness | You're reading it |
| **Skills Bundle** | 5 workflow skills: debugging, delegation, planning, code review, config architecture | [GitHub](https://github.com/primeline-ai/primeline-skills) |
| **UPF** | Universal Planning Framework with deep multi-stage planning | [GitHub](https://github.com/primeline-ai/universal-planning-framework) |
| **Quantum Lens** | Multi-perspective analysis + solution engineering (7 cognitive lenses) | [GitHub](https://github.com/primeline-ai/quantum-lens) |
| **Course** | Kairn + Synapse: AI-powered memory and knowledge graphs | [primeline.cc](https://primeline.cc) |

### How They Work Together

The Skills Bundle references Starter System commands with "if available" language:

- **Debugging** skill uses `/remember` to log known failures
- **Plan & Execute** skill uses `/handoff` to save plan progress between sessions
- **All skills** use `/context-stats` to manage context budget

If the Skills Bundle isn't installed, the Starter System works perfectly on its own. If it is installed, the skills automatically leverage your memory and handoff infrastructure.

---

## Built By

Extracted from a production AI system powering 4 active projects across 110+ sessions. The patterns here are the ones that proved durable in daily use: the ones that survived context compactions, model updates, and weeks of iteration.

Learn more at [primeline.cc](https://primeline.cc)

---

## License

MIT - use it, fork it, build on it.

---

## Part of the PrimeLine Ecosystem

| Tool | What It Does | Deep Dive |
|------|-------------|-----------|
| [**Evolving Lite**](https://github.com/primeline-ai/evolving-lite) | Self-improving Claude Code plugin — memory, delegation, self-correction | [Blog](https://primeline.cc/blog/knowledge-architecture) |
| [**Kairn**](https://github.com/primeline-ai/kairn) | Persistent knowledge graph with context routing for AI | [Blog](https://primeline.cc/blog/knowledge-architecture) |
| [**tmux Orchestration**](https://github.com/primeline-ai/claude-tmux-orchestration) | Parallel Claude Code sessions with heartbeat monitoring | [Blog](https://primeline.cc/blog/tmux-orchestration) |
| [**UPF**](https://github.com/primeline-ai/universal-planning-framework) | 3-stage planning with adversarial hardening | [Blog](https://primeline.cc/blog/planning-framework-dsv-reasoning) |
| [**Quantum Lens**](https://github.com/primeline-ai/quantum-lens) | 7 cognitive lenses for multi-perspective analysis | [Blog](https://primeline.cc/blog/quantum-lens-multi-agent-analysis) |
| [**PrimeLine Skills**](https://github.com/primeline-ai/primeline-skills) | 5 production-grade workflow skills for Claude Code | [Blog](https://primeline.cc/blog/score-based-auto-delegation) |
| [**Starter System**](https://github.com/primeline-ai/claude-code-starter-system) | Lightweight session memory and handoffs | [Blog](https://primeline.cc/blog/session-management) |

**[@PrimeLineAI](https://x.com/PrimeLineAI)** · [primeline.cc](https://primeline.cc) · [Free Guide](https://primeline.cc/guide)