# Vibe Coding Bootstrap - Existing Repo

Bootstrap context management best practices for existing codebases when working with Claude Code.

## What It Does

Automatically establishes "vibe coding" governance by:
1. Scanning your repository to understand its structure
2. Creating AGENTS.md (canonical project context with immutable conventions)
3. Creating CLAUDE.md (lightweight shim for Claude Code)
4. Setting up task tracking files (ai/PLAN.md, ai/STATUS.md, ai/DECISIONS.md)
5. Configuring .gitignore appropriately

## Quick Start

Just say to Claude Code:
```
"Bootstrap vibe coding for this existing repo"
```

The skill will:
1. Analyze your repository automatically
2. Propose complete file contents for your review
3. Wait for your approval
4. Create all files only after you approve

## Files Created

- **AGENTS.md** - Canonical project context (🔒 immutable contract + 🗺️ updateable repo map)
- **CLAUDE.md** - Lightweight shim that tells Claude Code to read AGENTS.md
- **ai/PLAN.md** - Task planning template (gitignored)
- **ai/STATUS.md** - Progress tracking template (gitignored)
- **ai/DECISIONS.md** - Architecture decision log (committed to git)

## Key Features

✅ **Scan-Then-Propose** - Never creates files without your approval
✅ **Comprehensive Analysis** - Detects languages, frameworks, testing, linting, danger zones
✅ **Smart Templates** - Pre-filled with actual repo conventions, not generic placeholders
✅ **Immutability Hierarchy** - Stable Contract (never auto-modified) vs. Repo Map (update with approval)
✅ **Task Tracking** - Multi-step task management with ephemeral plan/status, durable decisions
✅ **Evidence-Based** - Shows you exactly what repo evidence was used for proposals

## Installation

See the accompanying QUICKSTART guide for detailed usage instructions.

## License

MIT License - See LICENSE.txt
