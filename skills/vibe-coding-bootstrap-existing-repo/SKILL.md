---
name: vibe-coding-bootstrap-existing-repo
description: Bootstrap context management best practices for existing codebases. Use when starting work on an existing repository with Claude Code to establish AGENTS.md governance, task tracking files (ai/PLAN.md, ai/STATUS.md, ai/DECISIONS.md), and project conventions. Creates a systematic approach to repo analysis, workflow contracts, and durable decision tracking.
---

# Vibe Coding Bootstrap - Existing Repo

Bootstrap context management and governance files for effective AI-assisted development in existing codebases.

## Overview

This skill helps establish "vibe coding" best practices by creating governance files that:
- Define immutable workflow contracts and coding conventions (AGENTS.md)
- Track multi-step task progress (ai/PLAN.md, ai/STATUS.md)
- Record architectural decisions durably (ai/DECISIONS.md)
- Provide a lightweight shim for Claude Code (CLAUDE.md)

## Core Principles

**Governance Philosophy:**
- AGENTS.md is the canonical source of truth (human-owned, immutable unless explicitly changed)
- CLAUDE.md is a lightweight shim that references AGENTS.md
- Separate "Stable Contract" (never auto-modified) from "Repo Map" (updated only on structural changes with approval)
- Task files in ai/ are ephemeral working memory (plan/status in .gitignore, decisions committed)

**Key Distinction:**
- **Stable Contract**: Workflow rules, coding conventions, security guardrails → immutable
- **Repo Map**: Architecture overview, directories, commands → only updated when structure changes
- **Task Files**: Transient plan/status tracking + durable decision log

---

## Bootstrap Workflow

### Phase 1: Repository Analysis

**DO NOT create any files yet.** First, scan and analyze the repository.

#### 1.1 Detect Repository Characteristics

Identify the following systematically:

**Languages and Frameworks:**
```bash
# Check for language indicators
ls -la | grep -E '\.(py|js|ts|go|java|rb|php|rs)'
cat package.json 2>/dev/null | head -20
cat requirements.txt 2>/dev/null | head -20
cat go.mod 2>/dev/null | head -10
cat Cargo.toml 2>/dev/null | head -10
cat Gemfile 2>/dev/null | head -10
cat composer.json 2>/dev/null | head -20
```

**Build and Tooling:**
```bash
# Check for build/task configurations
ls -la | grep -E 'Makefile|package.json|pyproject.toml|Cargo.toml|build.gradle|pom.xml'
cat Makefile 2>/dev/null | grep '^[a-zA-Z]' | head -20
cat package.json 2>/dev/null | jq '.scripts' 2>/dev/null
```

**Testing Infrastructure:**
```bash
# Identify test frameworks
grep -r "import.*pytest" --include="*.py" | head -5
grep -r "describe\|it(" --include="*.js" --include="*.ts" | head -5
find . -name "*test*" -type d | head -10
```

**Code Quality Tools:**
```bash
# Look for linting/formatting configs
ls -la | grep -E '\.eslintrc|\.prettierrc|\.pylintrc|\.flake8|\.black|\.ruff'
cat .eslintrc* 2>/dev/null | head -10
cat .prettierrc* 2>/dev/null | head -10
cat pyproject.toml 2>/dev/null | grep -A 5 'tool.black\|tool.ruff\|tool.mypy'
```

**Directory Structure:**
```bash
# Understand high-level organization
tree -L 2 -d 2>/dev/null || find . -maxdepth 2 -type d
```

#### 1.2 Infer Coding Conventions

From the analysis, extract:

- **Language(s)**: Primary and secondary languages
- **Formatting**: Prettier, Black, gofmt, rustfmt, etc.
- **Linting**: ESLint, Pylint, Flake8, Ruff, Clippy, RuboCop
- **Type Checking**: TypeScript, mypy, pyright, flow
- **Testing**: Jest, pytest, Go test, cargo test, RSpec
- **Package Management**: npm/yarn/pnpm, pip/poetry/uv, cargo, bundler

#### 1.3 Identify Architecture Patterns

Determine:
- **Monorepo or single-repo?** (presence of workspaces, multiple package.json, etc.)
- **Framework**: React/Next.js, Django/Flask, Express, Rails, etc.
- **Architecture style**: MVC, layered, microservices, serverless
- **Key modules**: Frontend, backend, database, API, CLI, workers
- **Entry points**: main.py, index.ts, app.js, cmd/main.go

#### 1.4 Locate Danger Zones

Identify security-sensitive areas:
- **Authentication/Authorization**: auth/, middleware/auth, session management
- **Payment/Billing**: billing/, payments/, stripe/, checkout/
- **Database Migrations**: migrations/, alembic/, db/migrate/
- **Infrastructure**: terraform/, k8s/, .github/workflows/, deploy/
- **Secrets Management**: .env handling, secrets/, vault/

#### 1.5 Extract Key Commands

From package.json scripts, Makefile, or README:
- Install/setup: `npm install`, `pip install -r requirements.txt`, `make setup`
- Development: `npm run dev`, `make dev`, `cargo run`
- Testing: `npm test`, `pytest`, `make test`, `go test ./...`
- Linting: `npm run lint`, `ruff check`, `make lint`
- Build: `npm run build`, `make build`, `cargo build --release`
- Database: `make migrate`, `alembic upgrade head`
- Deploy: `make deploy`, deployment scripts

---

### Phase 2: Propose File Contents

After analysis, propose the complete contents of each file in chat for user review.

#### 2.1 AGENTS.md Structure

```markdown
# AGENTS.md - Project Context for AI Assistants

This file is the **single source of truth** for project conventions, architecture, and workflow rules.

**CRITICAL**: Do not modify the "Stable Contract" section unless explicitly instructed by a human.

---

## 🔒 STABLE CONTRACT (Human-Owned, Do Not Auto-Modify)

### Coding Conventions

[Inferred from repo - language, formatting, linting, typing, testing details]

**Languages**: [List primary languages]
**Formatting**: [Tool and configuration]
**Linting**: [Tool and rules]
**Type Checking**: [Tool and strictness]
**Testing**: [Framework and patterns]

### Workflow Contract

**Before Making Changes:**
1. Create a brief plan (3-6 bullets) in ai/PLAN.md for multi-step tasks
2. For simple changes, proceed directly but explain the change first

**During Changes:**
- Prefer minimal diffs - only change what's necessary
- Avoid refactoring unless explicitly requested
- Keep functions/modules focused and single-purpose
- Match existing code style exactly

**After Changes:**
- Run relevant tests and report results
- Run linting/formatting and fix issues
- Update ai/STATUS.md after completing each step

**Before Architectural Changes:**
- STOP and ask clarifying questions
- Read ai/DECISIONS.md to understand prior choices
- Propose the change and wait for approval

### Security & Quality Guardrails

**NEVER:**
- Hardcode secrets, API keys, or credentials
- Commit .env files or expose sensitive data
- Make destructive changes to production infrastructure
- Modify auth/billing/payment logic without explicit approval

**Danger Zones** (extra caution required):
[List identified sensitive areas]

**Logging & PII:**
[Guidelines for handling user data, if applicable]

---

## 🗺️ REPO MAP (Update only on structural changes; propose diff first)

### Repository Purpose

[Brief description of what this system does]

### Architecture Overview

[High-level architecture description - modules, boundaries, key flows]

### Directory Structure

[Key directories and their purposes]

### Entry Points

[Main files that start the application]

### Key Commands

**Setup:**
```bash
[Installation commands]
```

**Development:**
```bash
[Dev server commands]
```

**Testing:**
```bash
[Test commands]
```

**Linting:**
```bash
[Lint commands]
```

**Build:**
```bash
[Build commands]
```

**Database:**
```bash
[Migration commands, if applicable]
```

**Deploy:**
```bash
[Deployment commands, if applicable]
```

---

## ✅ TASK MANAGEMENT

For multi-step tasks (>3 steps or multi-file changes):

1. **Create/Update Plan**: Write or update `ai/PLAN.md` with:
   - Goal
   - Constraints
   - Step-by-step plan (3-10 steps)
   - Validation checklist
   - Rollback plan

2. **Track Progress**: Update `ai/STATUS.md` after each step:
   - Current objective
   - Completed steps
   - In-progress step
   - Next steps
   - Any blockers
   - Files touched

3. **Source of Truth**: `ai/PLAN.md` + `ai/STATUS.md` are the canonical state, not chat history

4. **Decision Logging**: Record architectural decisions in `ai/DECISIONS.md`:
   - Date and context
   - Decision made
   - Alternatives considered
   - Reasoning
   - Impact and tradeoffs

5. **On Blockers**: STOP and ask - do not guess or assume

6. **Before Continuing**: Read `ai/STATUS.md` to resume context

7. **Before Architectural Changes**: Read `ai/DECISIONS.md` to understand prior choices

### Git Ignore Rules

**Add to .gitignore:**
- `ai/PLAN.md` (ephemeral task tracking)
- `ai/STATUS.md` (ephemeral task tracking)

**Keep in version control:**
- `ai/DECISIONS.md` (durable decision log)
- `AGENTS.md` (project conventions)
- `CLAUDE.md` (AI assistant shim)
```

#### 2.2 CLAUDE.md Structure

```markdown
# Claude Instructions

This project uses AGENTS.md as the single source of truth for all project conventions, architecture, and workflow rules.

Read AGENTS.md before performing any action. Follow it strictly. Do not modify AGENTS.md unless explicitly instructed.

All task management files (ai/PLAN.md, ai/STATUS.md, ai/DECISIONS.md) apply equally to Claude Code sessions.
```

#### 2.3 ai/PLAN.md Template

```markdown
# Task Plan

**Created**: [Date]
**Goal**: [Brief description of what we're trying to achieve]

## Constraints

- [Any limitations, requirements, or boundaries]
- [E.g., "Must maintain backward compatibility"]
- [E.g., "Cannot modify database schema"]

## Step-by-Step Plan

1. [ ] [First step - be specific]
2. [ ] [Second step]
3. [ ] [Third step]
4. [ ] [Continue as needed...]

## Validation Checklist

- [ ] Tests pass
- [ ] Linting passes
- [ ] Manual testing completed
- [ ] Documentation updated (if needed)
- [ ] [Any other validation criteria]

## Rollback Plan

If this change needs to be reverted:
1. [Step to undo]
2. [Step to restore previous state]
3. [Verification that rollback worked]
```

#### 2.4 ai/STATUS.md Template

```markdown
# Task Status

**Last Updated**: [Timestamp]
**Current Objective**: [What we're working on right now]

## Completed ✅

- [Steps that are fully done]

## In Progress 🔄

- [Current step being worked on]
  - [Sub-tasks or details]

## Next Steps ⏭️

- [Upcoming steps from the plan]

## Blockers 🚫

- [Any issues preventing progress]
- [Decisions needed from human]

## Files Touched 📝

- [List of files modified in this task]
- [Helps track scope and potential conflicts]

## Notes

[Any observations, workarounds, or context worth remembering]
```

#### 2.5 ai/DECISIONS.md Template

```markdown
# Architecture & Design Decisions

This file records significant technical decisions for future reference.

---

## [YYYY-MM-DD] [Short Decision Title]

**Context**: [What situation led to this decision?]

**Decision**: [What did we decide to do?]

**Alternatives Considered**:
- [Option 1]: [Why not chosen]
- [Option 2]: [Why not chosen]

**Reasoning**: [Why was this the best choice?]

**Impact**: [What are the consequences and tradeoffs?]

**Related Files**: [Which files/modules are affected?]

---

[Additional decisions follow the same format]
```

---

### Phase 3: Present Proposals

After completing repository analysis, present to the user:

**1. Proposed AGENTS.md** - Full content with:
   - Inferred coding conventions
   - Workflow contract
   - Security guardrails
   - Repo map with actual findings
   - Task management rules

**2. Proposed CLAUDE.md** - The shim file

**3. Proposed ai/PLAN.md template** - Ready to use for first task

**4. Proposed ai/STATUS.md template** - Ready to use for first task

**5. Proposed ai/DECISIONS.md template** - Ready for first decision

**6. Diff Checklist** - Summary of evidence used:
   ```
   Repository Analysis Evidence:
   ✓ Languages detected: [list]
   ✓ Package manager: [npm/pip/cargo/etc]
   ✓ Test framework: [jest/pytest/etc]
   ✓ Linting tools: [eslint/ruff/etc]
   ✓ Key scripts found: [from package.json/Makefile]
   ✓ Directory structure analyzed
   ✓ Danger zones identified: [list]
   ```

**STOP here and wait for approval before creating any files.**

---

### Phase 4: Create Files (Only After Approval)

Once the user approves the proposals:

#### 4.1 Create Directory Structure
```bash
mkdir -p ai
```

#### 4.2 Write Files
```bash
# Write approved contents to files
cat > AGENTS.md << 'EOF'
[approved content]
EOF

cat > CLAUDE.md << 'EOF'
[approved content]
EOF

cat > ai/PLAN.md << 'EOF'
[approved template]
EOF

cat > ai/STATUS.md << 'EOF'
[approved template]
EOF

cat > ai/DECISIONS.md << 'EOF'
[approved template]
EOF
```

#### 4.3 Update .gitignore
```bash
# Add ephemeral task files to .gitignore
if ! grep -q "ai/PLAN.md" .gitignore 2>/dev/null; then
  echo "" >> .gitignore
  echo "# AI task tracking (ephemeral)" >> .gitignore
  echo "ai/PLAN.md" >> .gitignore
  echo "ai/STATUS.md" >> .gitignore
fi
```

#### 4.4 Verify Configuration
```bash
# Confirm gitignore is correct
echo "Checking .gitignore configuration..."
grep "ai/PLAN.md" .gitignore && echo "✓ ai/PLAN.md ignored"
grep "ai/STATUS.md" .gitignore && echo "✓ ai/STATUS.md ignored"
! grep "ai/DECISIONS.md" .gitignore && echo "✓ ai/DECISIONS.md NOT ignored (correct)"
! grep "AGENTS.md" .gitignore && echo "✓ AGENTS.md NOT ignored (correct)"
! grep "CLAUDE.md" .gitignore && echo "✓ CLAUDE.md NOT ignored (correct)"
```

#### 4.5 Confirmation Message

Present a final summary:
```
✅ Vibe Coding Bootstrap Complete

Created files:
- AGENTS.md (canonical project context - 🔒 do not auto-modify)
- CLAUDE.md (lightweight shim for Claude Code)
- ai/PLAN.md (task planning template)
- ai/STATUS.md (task status tracking template)
- ai/DECISIONS.md (durable decision log template)

Updated:
- .gitignore (added ai/PLAN.md and ai/STATUS.md)

Next steps:
1. Review AGENTS.md to ensure accuracy
2. Commit these files to version control
3. Start your first task - the system will use ai/PLAN.md and ai/STATUS.md automatically

Remember:
- AGENTS.md is immutable unless you explicitly update it
- For multi-step tasks, update ai/PLAN.md and ai/STATUS.md
- Record architectural decisions in ai/DECISIONS.md
- Claude Code will read AGENTS.md before any action
```

---

## Repository Analysis Patterns

### Detecting Languages

**Python:**
- `requirements.txt`, `pyproject.toml`, `setup.py`, `Pipfile`
- `.py` files in src/, app/, or root

**JavaScript/TypeScript:**
- `package.json`, `tsconfig.json`
- `.js`, `.ts`, `.jsx`, `.tsx` files

**Go:**
- `go.mod`, `go.sum`
- `main.go`, directories in `cmd/`

**Rust:**
- `Cargo.toml`, `Cargo.lock`
- `.rs` files in `src/`

**Ruby:**
- `Gemfile`, `Gemfile.lock`
- `.rb` files, Rails structure

**PHP:**
- `composer.json`, `composer.lock`
- `.php` files

### Detecting Frameworks

**Frontend:**
- React: `react` in dependencies, `.jsx`/`.tsx` files
- Next.js: `next` in dependencies, `pages/` or `app/` directory
- Vue: `vue` in dependencies, `.vue` files
- Svelte: `svelte` in dependencies, `.svelte` files

**Backend:**
- Express: `express` in dependencies
- Django: `django` in requirements, `manage.py`
- Flask: `flask` in requirements, `app.py` or `wsgi.py`
- Rails: `rails` in Gemfile, `config/routes.rb`
- FastAPI: `fastapi` in requirements
- Gin/Echo: imports in `.go` files

### Detecting Infrastructure

**Docker:**
- `Dockerfile`, `docker-compose.yml`

**Kubernetes:**
- `k8s/`, directories with `.yaml` manifests

**Terraform:**
- `.tf` files, `terraform/` directory

**CI/CD:**
- `.github/workflows/`
- `.gitlab-ci.yml`
- `.circleci/config.yml`
- `Jenkinsfile`

---

## Maintenance & Updates

### When to Update AGENTS.md

**Stable Contract (Human-Initiated Only):**
- Coding conventions change (new linter, formatter config)
- Workflow rules evolve (team adopts new practices)
- Security policies change

**Repo Map (AI-Initiated with Approval):**
- Major refactoring changes directory structure
- New module or service added
- Entry points change
- Key commands change (new scripts added to package.json)

**Process for Repo Map Updates:**
1. Detect structural change
2. Generate diff showing proposed changes
3. Present diff to user in chat
4. Wait for approval
5. Apply changes only after approval

### Example Repo Map Update

```markdown
I've detected a structural change in the repository. Here's the proposed update to AGENTS.md:

```diff
 ### Directory Structure
 
 - `src/` - Main source code
+- `services/` - Microservices (new: auth-service, payment-service)
 - `tests/` - Test files
 
 ### Key Commands
 
 **Development:**
 ```bash
 npm run dev
+npm run dev:services  # Start all microservices
 ```
```

Shall I apply this update to AGENTS.md?
```

---

## Common Patterns

### Monorepo Detection
```bash
# Check for workspace configuration
cat package.json | jq '.workspaces'
ls -d packages/*/package.json 2>/dev/null
cat pnpm-workspace.yaml 2>/dev/null
```

### Migration System Detection
```bash
# Python (Alembic/Django)
find . -type d -name "migrations" -o -name "alembic"

# Node (Knex/TypeORM)
ls -la migrations/ 2>/dev/null
cat package.json | jq '.scripts' | grep migrate

# Go (golang-migrate)
ls -la migrations/ 2>/dev/null
grep "migrate" go.mod
```

### Environment Variable Detection
```bash
# Look for .env examples
ls -la | grep -E '\.env\.example|\.env\.template|\.env\.sample'

# Check for dotenv usage
grep -r "dotenv" package.json requirements.txt go.mod 2>/dev/null
```

---

## Error Handling

### Repository Too Large
If repository analysis is taking too long:
1. Focus on key indicators (package.json, requirements.txt, go.mod)
2. Sample directories rather than full traversal
3. Ask user for high-level architecture description

### Ambiguous Conventions
If conventions cannot be inferred:
1. Note the ambiguity in proposals
2. Ask user for clarification
3. Provide sensible defaults with disclaimer

### Missing Information
If key information is unavailable:
1. Mark as "[TO BE DETERMINED]" in proposals
2. List what's missing in the diff checklist
3. Request user input for critical gaps

---

## Advanced Features

### Custom Danger Zones

Users may want to add custom danger zones beyond standard ones:

```markdown
**Danger Zones** (extra caution required):
- `auth/` - Authentication logic
- `billing/` - Payment processing
- `migrations/` - Database migrations
- [USER-SPECIFIED] - [REASON]
```

### Project-Specific Conventions

Some repos may have unique conventions:

```markdown
**Project-Specific Patterns**:
- Feature flags: Check `config/features.yml` before adding
- API versioning: New endpoints go in `v2/` only
- Database queries: Use query builder, never raw SQL
```

### Integration with External Tools

AGENTS.md can reference external documentation:

```markdown
**Additional Resources**:
- Architecture: See `docs/architecture.md`
- Deployment: See `docs/deploy.md`
- API Contracts: See OpenAPI spec at `api/openapi.yml`
```

---

## Best Practices

1. **Start Simple**: Focus on accurate basics rather than comprehensive detail
2. **Iterate**: Refine AGENTS.md as you learn more about the codebase
3. **Be Specific**: "Run `npm run test:unit`" not "run tests"
4. **Capture Intent**: Explain *why* rules exist, not just *what* they are
5. **Stay Current**: Update Repo Map when structure changes
6. **Preserve History**: Keep decision log comprehensive and dated

---

## Troubleshooting

### "AGENTS.md doesn't reflect current state"
- User should explicitly request updates to Stable Contract
- For Repo Map, generate diff and request approval

### "Task files aren't being used"
- Verify multi-step task (>3 steps or multi-file)
- Check that Claude Code can read/write ai/ directory

### "Conventions detected incorrectly"
- Review proposals carefully before approval
- Manually edit AGENTS.md after creation
- Update via explicit user instruction

### ".gitignore not working"
- Verify paths are relative to repo root
- Check for conflicting patterns
- Ensure trailing newline in .gitignore

---

## Summary

This skill establishes a governance framework for AI-assisted development:

✅ **Immutable Workflow Contract** - Prevents drift in coding practices
✅ **Adaptive Repo Map** - Stays current with structural changes
✅ **Durable Decision Log** - Preserves architectural context
✅ **Ephemeral Task Tracking** - Maintains state across sessions
✅ **Lightweight Integration** - Minimal overhead, maximum benefit

The result: Consistent, high-quality AI collaboration that respects project conventions and preserves human control.
