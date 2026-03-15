# Claude Vibe Coding Skills

Two Claude Code skills for bootstrapping AI-assisted development workflows — from blank page to production-ready project structure.

## Skills

### 1. `vibe-coding-new-project`
Start a brand new project from scratch. Guides you through a kickoff interview → PRD creation → governance files → project scaffold.

**Triggers when you say things like:**
- "I want to build X"
- "Start a new project"
- "Help me build a..."
- "Bootstrap new project"

### 2. `vibe-coding-bootstrap-existing-repo`
Add governance and AI context management to an existing codebase. Scans your repo, proposes `AGENTS.md`, `CLAUDE.md`, and task tracking files tailored to your stack.

**Triggers when you say things like:**
- "Bootstrap vibe coding for this repo"
- "Set up AGENTS.md for this project"
- "Add AI context management to this codebase"

---

## Installation

Each skill is a directory you copy into `~/.claude/skills/`. Claude Code picks them up automatically.

### Install both skills

```bash
# Clone the repo
git clone https://github.com/sreemakam/claude-vibe-coding-skills.git

# Copy skills to your Claude config
cp -r claude-vibe-coding-skills/skills/vibe-coding-new-project ~/.claude/skills/
cp -r claude-vibe-coding-skills/skills/vibe-coding-bootstrap-existing-repo ~/.claude/skills/
```

That's it. Restart Claude Code and the skills will be available.

### Install just one skill

```bash
# New project skill only
cp -r claude-vibe-coding-skills/skills/vibe-coding-new-project ~/.claude/skills/

# Existing repo skill only
cp -r claude-vibe-coding-skills/skills/vibe-coding-bootstrap-existing-repo ~/.claude/skills/
```

---

## What gets created

Both skills produce a consistent governance structure:

| File | Purpose | Git |
|------|---------|-----|
| `AGENTS.md` | Single source of truth for conventions, architecture, workflow rules | Committed |
| `CLAUDE.md` | Lightweight shim pointing Claude Code to AGENTS.md | Committed |
| `ai/PLAN.md` | Task planning for multi-step work | Gitignored (ephemeral) |
| `ai/STATUS.md` | Progress tracking across sessions | Gitignored (ephemeral) |
| `ai/DECISIONS.md` | Durable architectural decision log | Committed |

The `vibe-coding-new-project` skill also creates `docs/PROJECT_BRIEF.md` (your PRD), `README.md`, `.gitignore`, `.env.example`, and folder structure.

---

## Philosophy

- **AGENTS.md is immutable** — Claude never auto-modifies the stable contract section
- **Scan before creating** — the existing-repo skill proposes everything for your review before writing any files
- **Ephemeral vs. durable** — task plans are gitignored; decisions are committed
- **Specific, not generic** — all files are filled in with your actual stack and project details

---

## Requirements

- [Claude Code](https://claude.ai/code) CLI

---

## License

MIT
