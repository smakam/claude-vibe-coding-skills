# Architecture & Design Decisions

This file records significant technical decisions for future reference.
Keep entries dated and honest — document the reasoning, not just the outcome.

---

## [YYYY-MM-DD] Tech Stack Selection

**Context**: Starting a new project ([Project Name]). Need to choose the foundational technology stack before writing any code. The stack choice will shape architecture, tooling, hiring, and long-term maintainability.

**Decision**: Use [Language] + [Framework] + [Database] + [Hosting].

**Alternatives Considered**:
- [Alternative stack 1]: [Why not chosen — e.g., "team has less experience", "overkill for MVP scale", "slower iteration speed"]
- [Alternative stack 2]: [Why not chosen]
- [Alternative stack 3]: [Why not chosen, if applicable]

**Reasoning**: [Why this stack was chosen — e.g., "FastAPI gives us async performance with excellent developer ergonomics. PostgreSQL is proven at scale and gives us flexibility with JSONB if needed. Railway simplifies deployment to near-zero ops for an MVP."]

**Impact**:
- All backend code will be in [Language]
- [Framework]-specific patterns will be used throughout (routers, dependency injection, etc.)
- Database schema managed via [migration tool, e.g., Alembic / Prisma / go-migrate]
- [Any other architectural consequences]

**Related Files**: AGENTS.md, docs/PROJECT_BRIEF.md

---

[Future decisions follow the same format]
