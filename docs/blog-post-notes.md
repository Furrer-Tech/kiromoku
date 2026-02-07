# Blog Post Reference Notes: Building kiromoku with Claude as a Mentor

Use these notes as raw material for your blog post. Everything here is factual and pulled from our actual conversations, Linear tickets, project plan, and memory.

---

## The Project: kiromoku

- **What it is**: A modern, minimal anime and manga cataloguing application
- **Name origin**: "Kiro" from "kiroku" (record/archive) + "Moku" from "mokuroku" (catalogue/list)
- **Purpose**: Personal portfolio project to demonstrate full-stack development skills
- **Goal**: Apply for front-end and design engineer roles by end of 2026
- **Design philosophy**: Minimal, clean, calm, cozy, intentional — like a personal reading nook

## The Tech Stack

- **Runtime**: Bun (not Node.js)
- **Language**: TypeScript (strict mode)
- **Framework**: Waku (React Server Components)
- **Styling**: Tailwind CSS + shadcn/ui
- **Backend**: Waku API Routes (file-based, Web Standards)
- **Auth**: Better Auth
- **ORM**: Prisma
- **Database**: PostgreSQL
- **Containerization**: Docker / Docker Compose
- **Testing**: Vitest, Playwright, Testing Library
- **Linting/Formatting**: OXC (Oxlint, Oxfmt) — Rust-based, replaces ESLint/Prettier
- **CI/CD**: GitHub Actions
- **Cloud**: Azure (Container Apps, PostgreSQL Flexible Server)
- **Project Management**: Linear

## The Approach: AI as Mentor, Not Code Generator

This is the unique angle of your story. You configured Claude Code to act as a **Staff Software Engineer mentor**, not a code-writing bot.

### How Claude was configured (from CLAUDE.md):
- "Do not write code for me. Instead, explain concepts, point me to documentation, ask me questions, and help me think through problems."
- "Ask questions to gauge understanding before moving to the next step."
- "Make me think. When I ask 'how do I do X?', respond with guiding questions, not a code block."
- "Teach me to fish. Point me to official documentation and help me learn to read it myself."
- "Be honest. If my approach is wrong, say so directly. Explain why it's better, not just that it's better."
- "Think like a senior engineer reviewing a junior's work."

### Exceptions — when Claude CAN write code:
- Configuration files (tsconfig, Docker, CI/CD) — boilerplate-heavy, learning is in understanding them
- When explicitly asked: "can you write this for me" or "just give me the code"
- Small illustrative examples to demonstrate a concept

### What this looked like in practice:
- Claude would ask Walter to explain concepts before moving on
- Walter typed every command and wrote the code himself
- Claude challenged Walter to think through problems before giving answers
- Questions like "Why did you structure it this way?" were common
- Example from FB-21: Claude didn't just give the health check code — walked through what `export`, `async`, and `GET` mean individually, then had Walter build it piece by piece

## The Journey: How the Project Evolved

### The False Start (FB-1 through FB-15)
- Originally planned as a **monorepo** with separate `packages/server`, `packages/client`, `packages/shared`
- Used **Node.js + Express** initially
- Issues FB-1 through FB-15 were created under this architecture
- Completed FB-5 (monorepo structure), FB-6 (TypeScript config), FB-7 (Oxlint/Oxfmt), FB-8 (Express server), FB-9 (Dockerfile)
- Then made the decision to **pivot** to a simpler, integrated architecture

### The Pivot: Architecture Rethink
- Moved from monorepo (Node.js + Express) to a **single integrated project** (Bun + Waku)
- Why: monorepo complexity wasn't justified for a solo developer project
- Key decision: Use Waku as a single framework for both React pages AND API routes
- Waku uses Hono internally — get Hono's performance without manual integration
- Old issues (FB-1 through FB-15) were **archived** and new issues (FB-16 through FB-33) were created
- This was documented as **ADR-004** in the project plan

### The Hard Lesson: `bun create` Wiped Everything
- Running `bun create waku@latest` with `.` as the directory **wiped the `.git` directory** and existing files
- Lost uncommitted changes to CLAUDE.md, project plan, and copilot instructions
- Had to re-initialize git and re-add the remote
- **Lesson learned**: Commit early and often. Anything not committed is at risk.

## Phase 1: Foundation & Environment Setup (COMPLETE)

### FB-16: Install and configure Bun runtime
- Installed Bun, verified TypeScript runs natively
- **Learned**: Runtime vs compiler, Bun strips types but doesn't type-check, type safety comes from IDE + `tsc --noEmit` in CI

### FB-17: Initialize project structure with Waku
- Scaffolded Waku project with `bun create waku@latest`
- **Learned**: Server vs client components (server = default, client = `"use client"`), file-based routing (filename = URL path)

### FB-18: Configure TypeScript settings
- Set up tsconfig.json with strict mode, path aliases (`@/*` → `src/*`)
- **Learned**: Every tsconfig option and why it matters — strict, target, noEmit, module, moduleResolution, isolatedModules, paths/baseUrl

### FB-19: Set up development tooling (Oxlint/Oxfmt)
- Installed OXC tooling, configured lint/format scripts, VS Code format-on-save
- **Learned**: Linting catches code quality/bugs, formatting fixes visual style — they're different concerns

### FB-34: Configure accessibility tooling
- Enabled Oxlint's jsx-a11y plugin (native Rust, no extra package)
- Installed axe DevTools + WAVE browser extensions
- **Learned**: WCAG AA is the industry standard, a11y = accessibility (11 letters between a and y), linter checks source code statically vs browser extensions check rendered DOM at runtime

### FB-20: Initialize Git repository and create README
- Cleaned up .gitignore, updated README and CLAUDE.md to reflect Waku API routes architecture

### FB-21: Verify basic health check API route (THE LAST PHASE 1 TICKET)
- Created health check at `src/pages/_api/api/health.ts`
- Returned JSON with status and timestamp using `Response.json()`
- Verified API routes and React pages coexist on the same server

**Key troubleshooting moment during FB-21:**
1. Originally assumed API routes go in `src/pages/api/` (what the scaffolding tool created)
2. Docs at waku.gg showed `src/pages/_api/` (with underscore)
3. Researched and initially concluded the scaffolding was right and docs were wrong
4. Then got this error when running the dev server:
   ```
   [fsRouter] Migration required (v1.0.0-alpha.1): Move "./api/" to "./_api/".
   To preserve the old "/api/*" URL paths, move to "./_api/api/".
   ```
5. The **error message** was the ultimate source of truth — docs were correct, scaffolding was behind
6. Learned: `_api` tells Waku "this is an API directory, not a page route" — underscore stripped from URL
7. Used `_api/api/` nesting to preserve the `/api/` URL prefix

**Concepts learned during FB-21:**
- API routes export named functions (`GET`, `POST`) that return `Response`
- Page routes export React components that return JSX
- `Response.json()` is a Web Standard — works everywhere, no imports needed
- When docs and tooling disagree, error messages often have the latest info

## What's Ahead

### Phase 2: Database Design & Docker (FB-22 through FB-26)
- Learn Docker fundamentals (containers vs VMs, images vs containers)
- Create docker-compose.yml for PostgreSQL
- Set up Prisma ORM
- Design database schema (User, AnimeListEntry, MangaListEntry)
- Run first migration
- Create seed data

### Phase 3: Backend API Development (FB-27 through FB-33)
- API middleware and environment variables
- Prisma client integration
- RESTful CRUD endpoints for anime and manga lists
- Request validation with Zod
- Error handling patterns

### Phases 4-10 (Future)
- Phase 4: Authentication (Better Auth)
- Phase 5: MyAnimeList API integration
- Phase 6: Frontend foundation (Tailwind, shadcn/ui, routing)
- Phase 7: Core features and polish
- Phase 8: Testing (Vitest, Playwright)
- Phase 9: CI/CD (GitHub Actions)
- Phase 10: Cloud deployment (Azure)

## Concepts Learned So Far (Full List)

These are all things Walter can now explain in his own words:

1. Runtime vs compiler
2. What Bun is and how it differs from Node.js
3. Bun strips TypeScript types but doesn't type-check
4. Type safety layers: IDE (real-time) + tsc --noEmit (CI safety net)
5. Server vs client components in React
6. File-based routing (filename = URL path)
7. `bun create` vs `bun add`
8. tsconfig.json options and what they control
9. Path aliases for clean imports
10. `bunx` for running locally-installed binaries
11. Good commit message practices
12. Linting vs formatting (different concerns)
13. WCAG AA accessibility standards
14. a11y abbreviation
15. Static linting vs runtime browser extensions
16. API routes vs page routes (functions vs components)
17. Web Standards Request/Response pattern
18. Waku's `_api` convention
19. Error messages as the ultimate source of truth when docs/tooling conflict

## Possible Blog Post Angles

- **"Using AI as a Mentor, Not a Crutch"** — How you configured Claude to teach, not write code
- **"Building a Full-Stack App from Scratch as a Junior Developer"** — The honest journey, including mistakes
- **"What I Learned in Phase 1"** — Focused technical post on the foundation setup
- **"The Architecture Pivot"** — Why you scrapped the monorepo and started fresh, and what that teaches about decision-making
- **"When Docs, Tools, and Error Messages Disagree"** — The Waku `_api` debugging story as a microcosm of real development

## Key Quotes / Moments for the Post

- The `bun create` disaster — losing uncommitted work is a rite of passage
- The architecture pivot — having the courage to throw away work and start cleaner
- The `_api` vs `api` debugging — a perfect example of how real troubleshooting works (docs said one thing, scaffolding said another, error message was the truth)
- Writing your first API endpoint line by line — not copy-pasting, but understanding every keyword
- Linear for project management — treating a personal project like a real team project

## Timeline

- **Project started**: ~January 19, 2026 (FB-1 created)
- **Architecture pivot**: ~February 6, 2026 (old issues archived, new issues created)
- **Phase 1 completed**: February 7, 2026
- **All work in a single day** (Feb 7): FB-16 through FB-21 completed

## Links

- **GitHub**: https://github.com/Furrer-Tech/kiromoku.git
- **Linear project**: kiromoku on Linear (Furrer Bros team)
