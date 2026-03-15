# Vibe Coding Playbook

A methodology and toolkit for effective AI-assisted development — from blank page to production-ready project.

## Playbook

[`vibe-coding-playbook.html`](./vibe-coding-playbook.html) — the full operator playbook covering vibe coding best practices, workflow, and governance philosophy. Open it in a browser.

---

## Claude Code Skills

Ready-to-use skills for [Claude Code](https://claude.ai/code) that implement the playbook's workflow automatically.

### `vibe-coding-new-project`
Start a brand new project from scratch. Guides through a kickoff interview → PRD creation → governance files → project scaffold.

**Triggers when you say things like:**
- "I want to build X"
- "Start a new project"
- "Help me build a..."
- "Bootstrap new project"

### `vibe-coding-bootstrap-existing-repo`
Add governance and AI context management to an existing codebase. Scans your repo, proposes `AGENTS.md`, `CLAUDE.md`, and task tracking files tailored to your stack.

**Triggers when you say things like:**
- "Bootstrap vibe coding for this repo"
- "Set up AGENTS.md for this project"
- "Add AI context management to this codebase"

### Installation

```bash
# Clone the repo
git clone https://github.com/smakam/vibe-coding-playbook.git

# Copy skills to your Claude config
cp -r vibe-coding-playbook/claude-skills/vibe-coding-new-project ~/.claude/skills/
cp -r vibe-coding-playbook/claude-skills/vibe-coding-bootstrap-existing-repo ~/.claude/skills/
```

Restart Claude Code and the skills will be available.

---

## What the skills create

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

## License

MIT
