# Changelog

## [1.1.0] - 2026-02-28

### Fixed
- plugin.json now passes `claude plugin validate` (author as object, removed invalid fields)
- Removed `rules/` directory (not supported by plugin system - auto-discovery only works for commands, agents, skills, hooks)
- All rules content (memory bootup, context management, delegation, session hygiene) folded into system-boot skill
- Quick Start updated with correct `--plugin-dir` installation method

### Changed
- system-boot skill now includes full session guidelines (Part 1: Context Loading + Part 2: Session Guidelines)

## [1.0.0] - 2026-02-28

### Added
- System boot skill - automatic project context loading + session guidelines
- /remember command - save learnings across sessions
- /handoff command - create session continuity documents
- /project-status command - view and initialize project tracking
- /context-stats command - visual context window usage
- Memory templates for quick project setup
