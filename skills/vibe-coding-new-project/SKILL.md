---
name: vibe-coding-new-project
description: Bootstrap a brand new project from scratch with PRD creation, governance files, and project scaffolding. Use this skill when the user wants to start a new project, says things like "new project", "start a project from scratch", "vibe coding project", "bootstrap new project", "I want to build X", "help me build", or describes an app or tool they want to create. Always trigger this skill when the user is starting fresh and doesn't have an existing codebase — even if they don't say "vibe coding" explicitly. This skill guides through a full kickoff interview → PRD → governance → scaffold workflow.
---

# Vibe Coding New Project

Guide the user from blank page to a fully scaffolded project with a locked PRD, governance files, and a ready-to-code workspace.

## Overview

This skill runs in 6 phases. **Do not skip phases or rush ahead** — each phase gates the next. The interview prevents blank-page paralysis. The PRD approval gates governance. Governance gates the scaffold.

---

## Phase 1 — Kickoff Interview

Start with a warm, conversational opener. Ask these questions **one message at a time** — don't dump all 6 at once. Let the user respond before continuing. Adjust follow-ups based on what they share.

**Questions to ask (in roughly this order):**
1. What's the product name, and how would you describe it in one sentence?
2. What problem does it solve — and for whom?
3. If you had to pick the top 3–5 things the MVP absolutely must do, what are they?
4. Any strong preferences on tech stack? (language, framework, database, hosting/infra)
5. Are you building this solo, or is there a team?

**Tips:**
- If the user already volunteered some of this in their initial message, skip those questions and use what they said
- Ask follow-ups when answers are vague ("you mentioned an API — REST or GraphQL?")
- Keep the tone curious and collaborative, not interrogative

After collecting answers, give a brief summary back ("Here's what I heard — does this sound right?") and wait for confirmation before moving to Phase 2.

---

## Phase 2 — PRD Creation

Once the interview is confirmed, generate `docs/PROJECT_BRIEF.md` using the template at `assets/PROJECT_BRIEF_template.md`. Fill in every section from the interview answers — no generic placeholders.

**Key rule:** Every section should be specific to *this* project. If the user said "FastAPI + PostgreSQL + deployed on Railway", that goes in the Tech Stack section — not "TBD".

Present the full PRD in the chat for review before writing any files.

---

## Phase 3 — PRD Refinement Loop

Show the draft PRD and discuss with the user:

- Is the MVP scope right? (Common issue: scope creep — help them cut to the true MVP)
- Are success metrics concrete? ("users love it" → "50 signups in the first month")
- Is the tech stack finalized? (Confirm before creating governance — AGENTS.md depends on it)
- What's explicitly out of scope for v1?

**Loop:** Revise the PRD based on feedback. Repeat until the user explicitly says they're happy with it.

**Gate:** Do NOT proceed to Phase 4 until the user approves the PRD. Say something like: "Happy with this? Just say 'looks good' and I'll set up the project files."

---

## Phase 4 — Governance Files

Only after PRD approval: create the governance files. These must be **project-specific**, not generic. Use the confirmed tech stack and project context throughout.

### 4.1 `AGENTS.md`

Structure it with three sections:

**🔒 STABLE CONTRACT (Human-Owned, Do Not Auto-Modify)**
- Coding conventions (language, formatter, linter, type checker, test framework) — based on confirmed tech stack
- Workflow contract:
  - Before changes: create a plan in ai/PLAN.md for multi-step tasks
  - During: minimal diffs, match existing style, no unrequested refactoring
  - After: run tests, run linting, update ai/STATUS.md
  - Before architectural changes: STOP, read ai/DECISIONS.md, propose and wait for approval
- Security guardrails: no hardcoded secrets, no .env commits, no destructive infra changes without approval
- Project-specific danger zones (infer from the project — e.g., for a payments app: billing logic; for a multi-tenant SaaS: tenant isolation)

**🗺️ REPO MAP (Update only on structural changes; propose diff first)**
- Repository purpose (from PRD one-liner)
- Planned directory structure (based on tech stack)
- Entry points
- Key commands (install, dev, test, lint, build) — use the right commands for the tech stack

**✅ TASK MANAGEMENT**
- Standard task management rules (plan/status/decisions workflow)
- .gitignore rules: ai/PLAN.md and ai/STATUS.md are ephemeral (ignored), ai/DECISIONS.md is committed

### 4.2 `CLAUDE.md`

```markdown
# Claude Instructions

This project uses AGENTS.md as the single source of truth for all project conventions, architecture, and workflow rules.

Read AGENTS.md before performing any action. Follow it strictly. Do not modify AGENTS.md unless explicitly instructed.

All task management files (ai/PLAN.md, ai/STATUS.md, ai/DECISIONS.md) apply equally to Claude Code sessions.
```

### 4.3 `ai/PLAN.md`

Seed with the MVP features from the PRD as the first task list:

```markdown
# Task Plan

**Created**: [today's date]
**Goal**: Build [project name] MVP

## MVP Features to Implement

1. [ ] [Feature 1 from PRD]
2. [ ] [Feature 2 from PRD]
3. [ ] [Feature 3 from PRD]
[...etc]

## Constraints

- Stay within MVP scope defined in docs/PROJECT_BRIEF.md
- Follow conventions in AGENTS.md

## Validation Checklist

- [ ] All MVP features working end-to-end
- [ ] Tests passing
- [ ] Linting passing
- [ ] README updated with setup instructions
```

### 4.4 `ai/STATUS.md`

```markdown
# Task Status

**Last Updated**: [today's date]
**Current Objective**: Project setup — ready to start first feature

## Completed ✅

- Project scaffolded
- Governance files created
- PRD approved

## In Progress 🔄

- (nothing yet — pick a task from ai/PLAN.md to start)

## Next Steps ⏭️

- Review ai/PLAN.md and start first MVP feature

## Blockers 🚫

- None

## Files Touched 📝

- (none yet)
```

### 4.5 `ai/DECISIONS.md`

Seed with the first real decision — the tech stack choice. Use the template at `assets/DECISIONS_template.md`. Fill in the actual stack and reasoning from the interview.

---

## Phase 5 — Project Scaffold

After governance files are created, set up the project structure:

### 5.1 Initialize git
```bash
git init
```

### 5.2 Create `.gitignore`

Generate a `.gitignore` appropriate for the confirmed tech stack. Include:
- Standard ignores for the language/framework (node_modules/, __pycache__/, .venv/, target/, etc.)
- IDE files (.idea/, .vscode/ — optional, user preference)
- Environment files (.env, .env.local)
- AI task tracking:
  ```
  # AI task tracking (ephemeral)
  ai/PLAN.md
  ai/STATUS.md
  ```

### 5.3 Create `README.md`

```markdown
# [Project Name]

[One-liner tagline from PRD]

## Overview

[2-3 sentences from PRD problem/solution]

## Setup

> Instructions to be filled in as the project develops.

```bash
# Install dependencies
[appropriate install command for the stack]
```

## Development

```bash
# Start dev server
[appropriate dev command]
```

## See Also

- [Project Brief](docs/PROJECT_BRIEF.md) — PRD and MVP scope
- [Architecture Decisions](ai/DECISIONS.md) — technical decision log
```

### 5.4 Create folder structure

Base the structure on the tech stack. Examples:

**Python (FastAPI/Flask/Django):**
```
src/
tests/
docs/
ai/
```

**Node.js / Express:**
```
src/
  routes/
  middleware/
tests/
docs/
ai/
```

**Next.js:**
```
app/
components/
lib/
public/
docs/
ai/
```

**Go:**
```
cmd/
internal/
pkg/
docs/
ai/
```

**Generic / multi-language:**
```
src/
tests/
docs/
ai/
```

### 5.5 Create `.env.example` (if needed)

If the project likely needs environment variables (database, API keys, auth, etc.), create `.env.example` with commented-out placeholder variables:

```
# [Project Name] Environment Variables
# Copy to .env and fill in values

# Database
DATABASE_URL=postgresql://user:password@localhost:5432/dbname

# [Other variables inferred from the tech stack and project type]
```

### 5.6 Write `docs/PROJECT_BRIEF.md`

Write the approved PRD to disk.

### 5.7 Initial commit
```bash
git add .
git commit -m "chore: initial project scaffold

- Add project governance (AGENTS.md, CLAUDE.md)
- Add PRD (docs/PROJECT_BRIEF.md)
- Add ai/ task tracking scaffolding
- Add README and .gitignore"
```

---

## Phase 6 — Handoff Summary

Print a clear, friendly summary:

```
✅ Project "[Name]" is ready!

Files created:
  docs/PROJECT_BRIEF.md   — your approved PRD
  AGENTS.md               — coding conventions & workflow rules (🔒 immutable)
  CLAUDE.md               — lightweight shim for Claude Code
  ai/PLAN.md              — MVP task list (start here!)
  ai/STATUS.md            — progress tracker
  ai/DECISIONS.md         — tech stack decision logged
  README.md               — project readme
  .gitignore              — configured for [tech stack]
  .env.example            — environment variable template (if created)
  [src/ or app/ dirs]     — base folder structure

What to do next:
  1. Open ai/PLAN.md and pick your first feature to build
  2. Fill in the Open Questions in docs/PROJECT_BRIEF.md as you learn more
  3. Run `git log` to confirm the initial commit

Tips:
  - As you add collaborators, run /vibe-coding-bootstrap-existing-repo to help them onboard
  - Record every significant technical decision in ai/DECISIONS.md
  - AGENTS.md is your source of truth — update it intentionally, not automatically
```

---

## Tech Stack Reference

Use this to infer conventions when the user specifies a stack:

| Stack | Formatter | Linter | Tests | Package Manager |
|-------|-----------|--------|-------|-----------------|
| Python | black / ruff format | ruff / flake8 | pytest | pip / poetry / uv |
| Node.js | prettier | eslint | jest / vitest | npm / yarn / pnpm |
| TypeScript | prettier | eslint + tsc | jest / vitest | npm / yarn / pnpm |
| Next.js | prettier | eslint | jest / vitest | npm / yarn / pnpm |
| Go | gofmt | golangci-lint | go test | go modules |
| Rust | rustfmt | clippy | cargo test | cargo |
| Ruby | rubocop | rubocop | rspec | bundler |

For database choices, infer `.env.example` variables:
- PostgreSQL → `DATABASE_URL`
- MySQL/MariaDB → `DATABASE_URL`
- MongoDB → `MONGODB_URI`
- Redis → `REDIS_URL`
- SQLite → no env var needed (file path)

---

## Assets

- `assets/PROJECT_BRIEF_template.md` — PRD template for Phase 2
- `assets/DECISIONS_template.md` — first DECISIONS.md entry template for Phase 4
