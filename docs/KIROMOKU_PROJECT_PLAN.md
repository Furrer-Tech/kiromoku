# kiromoku Project Plan

> A modern, minimal anime and manga cataloguing application.

---

## Table of Contents

1. [Project Overview](#project-overview)
2. [Design Philosophy](#design-philosophy)
3. [Tech Stack](#tech-stack)
4. [Architecture Decisions](#architecture-decisions)
5. [Project Phases](#project-phases)
6. [Phase 1: Foundation & Environment Setup](#phase-1-foundation--environment-setup)
7. [Phase 2: Database Design & Docker](#phase-2-database-design--docker)
8. [Phase 3: Backend API Development](#phase-3-backend-api-development)
9. [Phase 4: Authentication & Authorization](#phase-4-authentication--authorization)
10. [Phase 5: MAL API Integration](#phase-5-mal-api-integration)
11. [Phase 6: Frontend Foundation](#phase-6-frontend-foundation)
12. [Phase 7: Core Features & Polish](#phase-7-core-features--polish)
13. [Phase 8: Testing](#phase-8-testing)
14. [Phase 9: CI/CD Pipeline](#phase-9-cicd-pipeline)
15. [Phase 10: Cloud Deployment](#phase-10-cloud-deployment)
16. [Learning Objectives by Phase](#learning-objectives-by-phase)
17. [Resources](#resources)

---

## Project Overview

**kiromoku** is a personal cataloguing application for anime and manga enthusiasts. Users can browse content via the MyAnimeList API, add items to their personal lists, track their progress, and maintain a curated archive of what they've watched, read, or want to experience.

### Etymology

- **Kiro** — From "kiroku" (記録), meaning "record" or "archive"
- **Moku** — From "mokuroku" (目録), meaning "catalogue" or "list"

### Core Features

- User registration and authentication
- Browse anime and manga via MAL API search
- Add items to personal "Watch List" or "Read List"
- Track status: Planning, In Progress, Completed, Dropped
- Personal notes and ratings
- Minimal, intentional user interface

---

## Design Philosophy

The application should feel:

- **Minimal** — No clutter, no unnecessary elements
- **Clean** — Generous whitespace, clear typography
- **Down to earth** — Organic, natural feeling
- **Calm** — No aggressive colors or animations
- **Cozy** — Warm, inviting, like a personal reading nook
- **Intentional** — Every element serves a purpose

### Color Palette

| Element | Color Name | Hex Code | Purpose |
|---------|------------|----------|---------|
| Background | Canvas | `#F4F1ED` | A desaturated, grounded base |
| Primary Text | Night Sky | `#32302F` | Deep charcoal for high readability |
| Accent | Matcha/Sage | `#8B9474` | Calm, organic green for active states |
| Warm Neutral | Cedar | `#D9C5B2` | Hover states, progress bars |

### Typography Considerations (TBD)

- Primary: A clean sans-serif for UI elements
- Secondary: Consider a subtle serif for headings to add warmth
- Generous line-height for readability

---

## Tech Stack

### Runtime & Package Manager

| Technology | Purpose |
|------------|---------|
| [Bun](https://bun.sh/) | JavaScript/TypeScript runtime and package manager. Replaces Node.js and npm. Native TypeScript support, fast installs, built-in test runner compatibility. |

### Frontend

| Technology | Purpose |
|------------|---------|
| [TypeScript](https://www.typescriptlang.org/) | Type safety across the full stack |
| [React](https://react.dev/) | UI component library |
| [Waku](https://waku.gg/) | Minimal React framework with React Server Components support (built on Vite) |
| [Tailwind CSS](https://tailwindcss.com/) | Utility-first styling |
| [shadcn/ui](https://ui.shadcn.com/) | Accessible, customizable component primitives |

### Backend

| Technology | Purpose |
|------------|---------|
| [Hono](https://hono.dev/) | Lightweight, fast web framework for API routes. Built for modern runtimes including Bun. |
| [Better Auth](https://www.better-auth.com/) | Authentication library. Handles user accounts, sessions, and auth flows with Prisma and Hono integration. |
| [Prisma](https://www.prisma.io/) | Type-safe ORM for PostgreSQL. Handles schema definition, migrations, seeding, and queries. |
| [PostgreSQL](https://www.postgresql.org/) | Relational database |

### Testing

| Technology | Purpose |
|------------|---------|
| [Vitest](https://vitest.dev/) | Unit and integration testing (pairs with Vite/Waku) |
| [Playwright](https://playwright.dev/) | End-to-end browser testing for full user flows |
| [@hono/testing](https://hono.dev/docs/guides/testing) | Hono's built-in test helpers for API endpoint testing (works with Vitest) |
| [Testing Library](https://testing-library.com/) | React component testing |

### Tooling

| Technology | Purpose |
|------------|---------|
| [OXC (Oxlint)](https://oxc.rs/) | Fast linting written in Rust (replaces ESLint) |
| [OXC (Oxfmt)](https://oxc.rs/) | Fast formatting written in Rust (replaces Prettier) |

### DevOps & Infrastructure

| Technology | Purpose |
|------------|---------|
| [Docker](https://www.docker.com/) | Containerization for development and deployment |
| Docker Compose | Multi-container orchestration (local dev) |
| Git / GitHub | Version control |
| [GitHub Actions](https://github.com/features/actions) | CI/CD pipeline automation |
| [Azure](https://azure.microsoft.com/) | Cloud hosting platform |

### Project Management

| Technology | Purpose |
|------------|---------|
| [Linear](https://linear.app/) | Issue tracking, project management, sprint planning |

### External APIs

| API | Purpose |
|-----|---------|
| [MyAnimeList API v2](https://myanimelist.net/apiconfig/references/api/v2) | Anime and manga data source |

---

## Architecture Decisions

This section documents key architectural choices and the reasoning behind them. These decisions were made intentionally and should be revisited only with good reason.

### ADR-001: Bun as Runtime

**Decision:** Use Bun instead of Node.js.

**Why:**
- Native TypeScript execution (no compilation step for development)
- Significantly faster package installs
- Built-in bundler and test runner compatibility
- Growing ecosystem and production-ready for this scale
- Learning a modern runtime is a resume differentiator

**Trade-offs:**
- Smaller community than Node.js
- Some npm packages may have edge-case compatibility issues
- Less established in enterprise environments (though growing)

### ADR-002: Hono as Backend Framework

**Decision:** Use Hono instead of Express.js.

**Why:**
- Built for modern runtimes (Bun, Deno, Cloudflare Workers, Node.js)
- Lightweight with excellent TypeScript support out of the box
- Middleware patterns similar to Express but more modern
- Built-in testing helpers that integrate with Vitest
- Strong ecosystem of middleware (CORS, JWT, Zod validation, etc.)

**Trade-offs:**
- Smaller community and fewer tutorials than Express
- Less "googleable" when stuck — will need to rely on official docs
- Some Express-specific middleware won't be directly compatible

### ADR-003: Prisma as ORM

**Decision:** Use Prisma instead of raw SQL with a migration tool.

**Why:**
- Declarative schema definition (`schema.prisma`)
- Auto-generated, type-safe client — queries return typed objects
- Built-in migration system (`prisma migrate`)
- Seeding support for development data
- Prisma Studio for visual database browsing during development
- Industry-standard ORM that's widely used in TypeScript projects

**Trade-offs:**
- Adds abstraction over raw SQL — less direct control
- Generated queries may not always be optimally performant
- Learning Prisma's schema language is its own thing
- Understanding raw SQL underneath is still important (and we will learn it)

### ADR-004: Integrated Project Structure (Waku with Built-in API Routes)

**Decision:** Use Waku as a single integrated framework for both React pages and API routes. Waku uses Hono internally, so API routes benefit from Hono's performance without manual integration. Single project, single deploy.

```
kiromoku/
├── src/
│   ├── pages/          # Waku routes (React pages + API routes)
│   │   └── api/        # API routes (file-based, e.g. api/health.ts → /api/health)
│   ├── components/     # React components
│   └── lib/            # Shared utilities
├── prisma/
│   └── schema.prisma
├── package.json
└── waku.config.ts
```

**Why:**
- Single process, single port — Waku serves React pages and API routes together
- Waku's built-in API routes use stable, documented APIs (unlike `waku/unstable_hono`)
- API routes use Web Standards (Request/Response) — portable knowledge
- Two Docker containers total (app + Postgres) instead of three
- No cross-service networking to configure
- Shared types and utilities without workspace wiring
- Simpler CI/CD — one image to build, one thing to deploy
- Appropriate complexity for a solo developer

**Trade-offs accepted:**
- A change to *any* code (frontend or API) redeploys the entire application
- Frontend and backend are coupled in the same build — can't scale them independently
- If the project grows significantly, extracting API routes into a standalone Hono service is a migration, not a rewrite

**Alternative considered:** Monorepo with separate `apps/web` and `apps/api` packages. Rejected because the added complexity (workspace config, separate Docker containers, cross-service communication) isn't justified at this scale.

**Alternative considered:** Manually mounting Hono as middleware via `waku/unstable_hono`. Rejected because the `unstable_` prefix indicates the API may change, and Waku's built-in API routes provide sufficient functionality for our needs.

### ADR-005: Not Storing MAL Metadata Locally

**Decision:** Fetch anime/manga metadata from MAL API on demand. Do not cache in our database for MVP.

**Why:**
- Keeps our schema simple and focused on user data
- Always shows fresh data (titles, images, synopses)
- Smaller database footprint
- Learning focus is elsewhere

**Trade-offs:**
- Dependent on MAL API availability
- Slower list page loads (multiple API calls)
- Subject to MAL API rate limits
- Can revisit with a caching layer later if needed

### ADR-006: Better Auth for Authentication

**Decision:** Use Better Auth as the authentication library instead of building auth from scratch or using Lucia/Auth.js.

**Why:**
- Framework-agnostic with first-class Hono integration
- Built-in Prisma adapter — auth tables managed alongside our schema
- Handles the hard security details: password hashing, session management, CSRF protection, secure cookies
- Supports email/password auth out of the box, with social auth (Discord, etc.) available as plugins
- TypeScript-first with strong type safety
- Active development and good documentation
- Lets us focus on *understanding* auth concepts rather than implementing cryptographic primitives

**Trade-offs:**
- Adds a dependency for something we *could* build from scratch
- Better Auth manages its own database tables (user, session, account, verification) — less control over schema
- Newer library with a smaller community than Auth.js
- Need to understand how it works under the hood, not just treat it as a black box

**Important:** Using Better Auth does NOT mean skipping auth fundamentals. Phase 4 includes dedicated learning on authentication vs. authorization, session mechanics, password security, and cookie behavior. The library handles implementation, but we need to understand *what* it's doing and *why*.

---

## Project Phases

```
Phase 1: Foundation & Environment Setup
    ↓
Phase 2: Database Design & Docker
    ↓
Phase 3: Backend API Development
    ↓
Phase 4: Authentication & Authorization
    ↓
Phase 5: MAL API Integration
    ↓
Phase 6: Frontend Foundation
    ↓
Phase 7: Core Features & Polish
    ↓
Phase 8: Testing
    ↓
Phase 9: CI/CD Pipeline
    ↓
Phase 10: Cloud Deployment
```

---

## Phase 1: Foundation & Environment Setup

**Goal:** Set up the development environment, configure tooling, and establish the project structure.

### Tasks

1. **Install and configure Bun**
   - Install Bun runtime
   - Understand differences from Node.js and npm
   - Verify TypeScript runs natively without a build step

2. **Initialize the project structure**
   - Scaffold the integrated project structure (see ADR-004)
   - Create root `package.json`
   - Set up `src/` directory with `pages/`, `components/`, `api/`, and `lib/` directories

3. **Configure TypeScript**
   - Root `tsconfig.json` with base settings
   - Understand key `tsconfig` options and why they matter

4. **Set up development tooling**
   - Linting with Oxlint
   - Formatting with Oxfmt
   - Configure editor integration (VS Code settings)

5. **Initialize Git repository**
   - Create `.gitignore` (Bun-specific entries, Prisma, Docker, etc.)
   - Set up initial commit structure
   - Create `README.md`
   - Set up Linear project and link to repository

6. **Configure accessibility tooling**
   - Enable Oxlint's built-in `jsx-a11y` plugin (native Rust — no extra package needed)
   - Configure `settings.jsx-a11y.components` to map custom components to DOM elements
   - Install browser extensions for development: axe DevTools, WAVE
   - Understand WCAG 2.1 AA standard basics — what it is and why it matters

7. **Set up Waku with API routes**
   - Initialize Waku project
   - Create API routes using Waku's built-in file-based API routing
   - Verify a basic API route (`GET /api/health`) works alongside a React page

### Deliverables

- [ ] Bun installed and working
- [ ] Integrated project structure scaffolded (Waku with API routes)
- [ ] Basic health check API route returning a response
- [ ] TypeScript compiling across the project
- [ ] Oxlint and Oxfmt configured and running
- [ ] Oxlint jsx-a11y rules enabled
- [ ] Git repository initialized with clean history
- [ ] Linear project created with initial backlog

### Key Learnings

- What is Bun and how does it differ from Node.js?
- How does `bun install` differ from `npm install`?
- What do `tsconfig.json` options actually control?
- How does TypeScript compilation work?
- How Waku serves React pages and API routes using its built-in file-based routing
- What is WCAG and why does accessibility matter from day one?

---

## Phase 2: Database Design & Docker

**Goal:** Design the database schema with Prisma, set up PostgreSQL in Docker, and learn containerization fundamentals.

### Tasks

1. **Learn Docker fundamentals**
   - What is a container vs. a virtual machine?
   - What is an image vs. a container?
   - Understanding Dockerfile instructions
   - Understanding Docker Compose

2. **Create Docker development environment**
   - `docker-compose.yml` for local development
   - PostgreSQL container with volume for data persistence
   - Optional: Dockerfile for the Bun application (can defer to Phase 9/10)

3. **Set up Prisma**
   - Install Prisma CLI and client
   - Initialize Prisma with PostgreSQL provider
   - Understand `schema.prisma` syntax and conventions
   - Configure Prisma to connect to Docker PostgreSQL

4. **Design and implement the database schema**
   - Define models in `schema.prisma`
   - Understand Prisma relations (`@relation`)
   - Run initial migration (`prisma migrate dev`)
   - Explore Prisma Studio for visual database inspection

5. **Create seed data**
   - Write a `prisma/seed.ts` file
   - Populate development database with test data
   - Understand when and why seeding matters

### Deliverables

- [ ] `docker compose up` starts PostgreSQL and it persists data
- [ ] Prisma schema defined with all models
- [ ] Initial migration created and applied
- [ ] Seed script populates development data
- [ ] Prisma Studio accessible for database inspection
- [ ] Schema documented in this plan (see below)

### Key Learnings

- Docker vocabulary and mental model
- Docker Compose for local development services
- Prisma schema language and conventions
- Database relations and foreign keys
- Migration workflows (creating, applying, rolling back)
- ERD design and normalization basics

### Database Schema

Defined in `prisma/schema.prisma`. Below is the planned model structure:

**Auth Tables (managed by Better Auth)**

Better Auth creates and manages its own tables when using the Prisma adapter. These include:

| Table | Purpose |
|-------|---------|
| user | User accounts (id, name, email, emailVerified, image, createdAt, updatedAt) |
| session | Active sessions tied to users |
| account | Auth provider accounts (email/password, OAuth providers) |
| verification | Email verification and password reset tokens |

These tables are defined in your `schema.prisma` file but their structure is dictated by Better Auth. You extend the user model with additional fields (like `username`) as needed.

**AnimeListEntry**

| Field | Type | Notes |
|-------|------|-------|
| id | String (UUID) | `@id @default(uuid())` |
| userId | String | Foreign key → User |
| malId | Int | MAL's anime ID |
| status | Enum | PLANNING, WATCHING, COMPLETED, DROPPED, ON_HOLD |
| score | Int? | 1–10, nullable |
| episodesWatched | Int | Default 0 |
| notes | String? | Optional |
| createdAt | DateTime | `@default(now())` |
| updatedAt | DateTime | `@updatedAt` |
| `@@unique([userId, malId])` | | Composite unique constraint |

**MangaListEntry**

| Field | Type | Notes |
|-------|------|-------|
| id | String (UUID) | `@id @default(uuid())` |
| userId | String | Foreign key → User |
| malId | Int | MAL's manga ID |
| status | Enum | PLANNING, READING, COMPLETED, DROPPED, ON_HOLD |
| score | Int? | 1–10, nullable |
| chaptersRead | Int | Default 0 |
| volumesRead | Int | Default 0 |
| notes | String? | Optional |
| createdAt | DateTime | `@default(now())` |
| updatedAt | DateTime | `@updatedAt` |
| `@@unique([userId, malId])` | | Composite unique constraint |

### Design Decision: Not Storing Metadata

We intentionally do NOT store anime/manga metadata (titles, images, synopses) in our database. See [ADR-005](#adr-005-not-storing-mal-metadata-locally) for reasoning.

---

## Phase 3: Backend API Development

**Goal:** Build a RESTful API with Hono, implementing proper patterns, middleware, and error handling.

### Tasks

1. **Configure API middleware and environment variables**
   - Understand how Waku's API routes work (Hono under the hood)
   - Configure middleware (CORS, logging)
   - Environment variables with Bun's built-in `process.env` or `.env` support

2. **Integrate Prisma client**
   - Generate Prisma client (`prisma generate`)
   - Set up a shared Prisma instance
   - Understand connection pooling with Prisma

3. **Build API routes**
   - RESTful route structure using Waku's file-based API routing
   - Request validation with Zod
   - Error handling patterns
   - Response formatting and typing

4. **Implement CRUD operations**
   - Health check endpoint
   - Anime list entry CRUD
   - Manga list entry CRUD
   - Proper HTTP status codes

### Deliverables

- [ ] Organized API route structure
- [ ] Prisma queries working from API routes
- [ ] Full CRUD for anime and manga list entries
- [ ] Request validation on all mutation endpoints
- [ ] Consistent error handling throughout
- [ ] API documentation (Markdown in repo)

### Key Learnings

- Waku API routing and middleware patterns (Hono under the hood)
- RESTful API design principles
- Type-safe database queries with Prisma
- Request validation strategies
- Error handling patterns in APIs
- Environment variable management

### Accessibility Considerations for APIs

While backend APIs don't directly affect screen readers or keyboard navigation, they impact the accessible user experience:

- **Error messages:** API error responses should be clear, human-readable, and actionable. A screen reader user needs to understand what went wrong and how to fix it.
- **Validation messages:** Field-level validation errors should specify *which* field has the problem and *why* — the frontend will relay these to assistive technology.
- **Status codes:** Use proper HTTP status codes so the frontend can provide appropriate, accessible feedback to users (e.g., distinguishing "not found" from "validation error" from "server error").

**Practice:** When writing error responses, imagine explaining the problem to a friend, not a machine.

### API Routes (Draft)

```
Health
  GET     /api/health

Authentication (Phase 4 — handled by Better Auth)
  POST    /api/auth/sign-up
  POST    /api/auth/sign-in
  POST    /api/auth/sign-out
  GET     /api/auth/session
  (Better Auth exposes additional endpoints automatically)

Anime List
  GET     /api/anime-list              (user's full list)
  GET     /api/anime-list/:id          (single entry)
  POST    /api/anime-list              (add entry)
  PATCH   /api/anime-list/:id          (update entry)
  DELETE  /api/anime-list/:id          (remove entry)

Manga List
  GET     /api/manga-list
  GET     /api/manga-list/:id
  POST    /api/manga-list
  PATCH   /api/manga-list/:id
  DELETE  /api/manga-list/:id

MAL Proxy (Phase 5)
  GET     /api/search/anime?q=...
  GET     /api/search/manga?q=...
  GET     /api/anime/:malId
  GET     /api/manga/:malId
```

---

## Phase 4: Authentication & Authorization

**Goal:** Understand authentication and authorization fundamentals, then implement user accounts using Better Auth.

### Authentication vs. Authorization — What's the Difference?

These two terms get thrown around together so often that it's easy to conflate them. They are distinct concepts:

**Authentication (AuthN)** answers: *"Who are you?"*
This is the process of verifying a user's identity. When someone types in their email and password and clicks "Log in," the system is authenticating them — confirming they are who they claim to be. Other forms of authentication include OAuth (signing in with Google/Discord), magic links, biometrics, and multi-factor authentication (MFA).

**Authorization (AuthZ)** answers: *"What are you allowed to do?"*
Once we know *who* someone is, authorization determines what they can access. Can this user view their own anime list? Can they edit someone else's list? Can they access admin settings? Authorization is about permissions and access control.

**In kiromoku's case:**
- **AuthN:** A user registers an account and logs in with email/password. Better Auth handles this.
- **AuthZ:** A logged-in user can only view and modify *their own* lists. Our API middleware handles this.

For the MVP, authorization is straightforward (users can only access their own data). But understanding the distinction matters because in a team or production app, authorization gets complex fast — roles, permissions, policies, resource-based access control.

### Tasks

1. **Learn authentication fundamentals (conceptual)**
   - What is a session? How does session-based auth differ from token-based (JWT)?
   - How are passwords stored securely? What is hashing vs. encryption?
   - What makes a cookie secure? (HttpOnly, Secure, SameSite flags)
   - What is CSRF and how is it prevented?
   - What is OAuth and when would you use it?

2. **Set up Better Auth**
   - Install Better Auth and its Prisma adapter
   - Configure Better Auth with Hono integration
   - Understand the database tables Better Auth creates (user, session, account, verification)
   - Run Prisma migrations to add auth tables

3. **Implement registration and login**
   - Email/password registration
   - Email/password login
   - Session creation and cookie management (handled by Better Auth)
   - Logout and session destruction

4. **Implement route protection (authorization)**
   - Auth middleware in Hono — extract user from session
   - Protect list endpoints: require authentication
   - Ensure users can only access their own data (resource-level authorization)
   - Return proper 401 (Unauthenticated) vs. 403 (Unauthorized) status codes

5. **Understand what Better Auth is doing under the hood**
   - Inspect the database after registration — what did it create?
   - Look at the cookies in your browser dev tools — what's being set?
   - Read through Better Auth's source or docs to understand the session flow
   - Be able to explain: what happens between "user clicks login" and "user sees their dashboard"?

### Deliverables

- [ ] Better Auth configured with Hono and Prisma
- [ ] User registration working
- [ ] User login/logout working
- [ ] Protected routes requiring authentication
- [ ] Resource-level authorization (users access only their own data)
- [ ] Can articulate the difference between authn and authz
- [ ] Can explain what Better Auth does at each step of the auth flow

### Key Learnings

- Authentication vs. authorization — conceptual clarity
- How passwords are securely stored (hashing, salting)
- Session-based authentication flow
- Cookie security flags and their purpose
- Middleware patterns for route protection in Hono
- The 401 vs. 403 distinction and when to use each
- How an auth library works under the hood (not just how to configure it)

### Security Notes

Better Auth handles the critical security concerns (password hashing, session tokens, CSRF). For production hardening, also consider:

- Rate limiting on auth endpoints (prevent brute force)
- Account lockout after repeated failed attempts
- Password strength requirements
- Secure password reset flow (Better Auth supports this via plugins)
- Email verification (Better Auth supports this via plugins)
- Social auth (Discord OAuth would be thematic for this app)

---

## Phase 5: MAL API Integration

**Goal:** Integrate with MyAnimeList API to fetch anime and manga data.

### Tasks

1. **Register for MAL API access**
   - Create MAL account if needed
   - Register application for API credentials

2. **Understand the MAL API**
   - Authentication method (client credentials)
   - Rate limits and usage policies
   - Available endpoints and response shapes
   - What data fields are available for anime vs. manga

3. **Build API proxy layer**
   - Search endpoints (anime and manga)
   - Detail fetch endpoints (individual anime/manga by MAL ID)
   - Type definitions for MAL API responses

4. **Handle API errors gracefully**
   - Rate limit handling (respect retry-after headers)
   - Timeout handling
   - Fallback behavior when MAL is unavailable
   - User-facing error messages

### Deliverables

- [ ] Anime search working through our API
- [ ] Manga search working through our API
- [ ] Individual anime/manga details fetchable
- [ ] Proper error handling for API failures
- [ ] MAL response types defined

### Key Learnings

- Working with third-party APIs
- API key management and environment variables
- The proxy pattern — why not call MAL directly from the frontend?
- Rate limiting awareness and respectful API usage
- Typing external API responses

---

## Phase 6: Frontend Foundation

**Goal:** Set up React with Waku, configure the design system, and establish the component architecture.

### Tasks

1. **Initialize React application with Waku**
   - Scaffold project with Waku
   - Understand Waku's project structure and conventions
   - Configure path aliases
   - Set up environment variables for API URL

2. **Configure Tailwind CSS**
   - Install and configure Tailwind
   - Implement custom color palette from design system
   - Configure font families
   - Set up any design tokens as CSS custom properties

3. **Install and configure shadcn/ui**
   - Initialize shadcn/ui with Tailwind
   - Customize theme to match design philosophy
   - Add initial components (Button, Input, Card, etc.)

4. **Set up routing**
   - Understand Waku's routing approach
   - Create pages: home, login, register, anime list, manga list
   - Create layouts with shared navigation
   - Understand server vs. client components (`'use client'` directive)

5. **Build foundational components**
   - Layout (header, main content area, footer)
   - Navigation
   - Common UI patterns (loading states, error boundaries)

6. **Establish accessible component patterns**
   - Verify shadcn/ui's built-in accessibility features (built on Radix UI primitives)
   - Test keyboard navigation in base components (Button, Input, Select, etc.)
   - Establish focus-visible styles that match the design system
   - Verify color contrast ratios against WCAG AA (4.5:1 for normal text, 3:1 for large text)

7. **Semantic HTML and document structure**
   - Use semantic HTML5 elements (`<nav>`, `<main>`, `<header>`, `<footer>`, `<article>`)
   - Set up proper heading hierarchy (h1–h6 used correctly, no skipped levels)
   - Add a skip-to-content link for keyboard users
   - Configure landmark regions with ARIA labels where needed

8. **Connect to backend API**
   - API client setup (typed fetch wrapper)
   - Error handling patterns for API calls
   - Loading and error state management

### Deliverables

- [ ] Waku app running with hot reload
- [ ] Tailwind configured with custom design tokens
- [ ] shadcn/ui integrated and themed to match design philosophy
- [ ] Routing working (home, auth pages, list pages)
- [ ] Server vs. client component boundaries understood
- [ ] API connection established with typed responses
- [ ] Basic layout implemented matching design philosophy
- [ ] All base shadcn/ui components tested with keyboard navigation
- [ ] Color contrast verified against WCAG AA
- [ ] Semantic HTML structure implemented in layouts
- [ ] Skip-to-content link functional

### Key Learnings

- Waku as a minimal React Server Components framework
- React Server Components vs. client components — when to use which
- File-based routing patterns
- Tailwind CSS methodology and customization
- Component architecture and composition
- Design system implementation from palette to code
- Semantic HTML and why it matters for screen readers
- Keyboard navigation patterns (Tab, Enter, Escape, Arrow keys)
- Focus management fundamentals
- Color contrast ratios and WCAG AA standards
- How shadcn/ui (Radix UI) provides accessible primitives out of the box

---

## Phase 7: Core Features & Polish

**Goal:** Build out the full user experience and polish the interface to portfolio quality.

### Tasks

1. **Search experience**
   - Search input with debouncing
   - Results display (cards or list view, matching design philosophy)
   - "Add to list" flow — choosing status, optional score

2. **List management**
   - View anime and manga lists
   - Filter by status (planning, watching/reading, completed, etc.)
   - Update entry status, score, progress
   - Edit notes
   - Remove entries with confirmation

3. **Polish and refinement**
   - Loading skeletons matching the layout
   - Error states with helpful, friendly messages
   - Empty states ("Your watch list is empty — search for something!")
   - Responsive design (mobile-first approach)
   - Transitions and micro-interactions (subtle, calm — matching design philosophy)

4. **Accessibility audit and polish**

   By this phase, you've been building with accessibility in mind since Phase 6 — semantic HTML, keyboard-navigable components, proper contrast. This task is about auditing what you've built, catching gaps, and polishing the experience.

   **Audit:**
   - Run automated tools (axe DevTools, Lighthouse accessibility audit)
   - Manual keyboard-only test: can you complete every task without a mouse?
   - Screen reader test with VoiceOver (Mac) — navigate the full user flow
   - Color contrast verification for all states (hover, disabled, error, focus)

   **Polish:**
   - Add ARIA live regions for dynamic content (search results loading, list updates)
   - Verify focus management for modals, dialogs, and route changes
   - Ensure loading states are announced to screen readers
   - Verify all interactive elements have accessible names
   - Ensure error messages are associated with form fields (`aria-describedby`)
   - Fix any issues identified in the audit

### Deliverables

- [ ] Complete user flow: register → login → search → add to list → manage list
- [ ] Polished UI matching the design philosophy
- [ ] Responsive across mobile, tablet, and desktop
- [ ] Accessibility audit completed (automated + manual)
- [ ] All audit issues resolved
- [ ] No dead ends — every state has a clear path forward

### Key Learnings

- UX patterns: debouncing, optimistic updates, skeleton loading
- State management patterns in React
- Responsive design methodology with Tailwind
- How to conduct a comprehensive accessibility audit
- Difference between automated and manual accessibility testing
- How screen readers actually navigate web applications
- Common SPA accessibility challenges (focus management, dynamic content)
- When ARIA is needed vs. when semantic HTML is sufficient
- Attention to detail that separates portfolio projects from tutorials

---

## Phase 8: Testing

**Goal:** Learn testing fundamentals and implement a practical, high-value test suite.

### Understanding Testing Types

| Type | What It Tests | Tools | Speed |
|------|---------------|-------|-------|
| Unit | Individual functions, utilities, helpers in isolation | Vitest | Fast |
| Integration | Multiple units working together (API routes + database) | Vitest, Hono test helpers | Medium |
| Component | React components render and behave correctly | Vitest, Testing Library | Medium |
| End-to-End | Full user flows through a real browser | Playwright | Slow |

### Testing Philosophy

Focus on **practical, high-value tests** — not 100% coverage for its own sake:

- Unit tests for utility functions and complex business logic
- Integration tests for API endpoints (request in → response out)
- Component tests for interactive UI elements
- E2E tests for critical user journeys (register, login, search, add to list)

### Tasks

1. **Set up testing infrastructure**
   - Configure Vitest for the project
   - Set up a test database (separate from development)
   - Configure Prisma for test environment
   - Configure Playwright

2. **Backend testing**
   - Unit tests for utility functions
   - Integration tests for API routes using Hono's test client
   - Database integration tests with test database and cleanup

3. **Frontend testing**
   - Component tests with Testing Library
   - Hook tests for custom hooks
   - Form interaction tests

4. **End-to-end testing**
   - Install and configure Playwright
   - Write E2E tests for core user journeys
   - Understand the testing pyramid and why E2E tests are expensive

5. **Accessibility testing**
   - Integrate axe-core with Vitest for automated accessibility checks
   - Use Testing Library's accessible queries (`getByRole`, `getByLabelText`) — prefer these over `getByTestId`
   - Add keyboard navigation tests for interactive components
   - E2E tests that verify keyboard-only flows (Playwright)

6. **Testing patterns to learn**
   - Arrange-Act-Assert pattern
   - Mocking and stubbing dependencies
   - Test fixtures and factories
   - Testing async operations
   - Database cleanup between tests

### Deliverables

- [ ] Vitest configured and running
- [ ] API route integration tests for all CRUD operations
- [ ] Component tests for key interactive elements
- [ ] Playwright E2E tests for core user journeys
- [ ] Automated accessibility tests integrated into component and E2E tests
- [ ] Keyboard navigation tested in E2E flows
- [ ] Tests integrated into CI pipeline (Phase 9)
- [ ] Coverage reporting configured

### Key Learnings

- Why we test and why we don't test everything
- The testing pyramid / testing trophy
- Mocking and test isolation strategies
- Writing testable code
- How to test against a real database
- E2E testing patterns with Playwright
- What makes a test valuable vs. what is test theater
- How to test accessibility programmatically (axe-core)
- Testing Library's accessible queries and why they encourage accessible code

---

## Phase 9: CI/CD Pipeline

**Goal:** Set up automated testing and deployment using GitHub Actions. Understand what CI/CD is and why it matters.

### What is CI/CD?

**CI (Continuous Integration)** is the practice of automatically running checks — linting, type checking, tests — every time code is pushed to the repository. The goal is to catch problems early and ensure that new code doesn't break what already works. Instead of finding out something is broken when you deploy, you find out within minutes of pushing.

**CD (Continuous Deployment / Continuous Delivery)** extends this by automatically deploying code that passes all checks. "Continuous Delivery" means the code is always *ready* to deploy (maybe with a manual approval step). "Continuous Deployment" means it deploys automatically without human intervention. We'll use Continuous Delivery — automatic deploy to staging, manual approval for production.

**Why does this matter?**

Without CI/CD, deploying looks like: manually run tests (if you remember), manually build the app, manually copy files to a server, hope nothing breaks. With CI/CD, deploying looks like: merge a pull request, and everything else happens automatically. It's the difference between a process that depends on human memory and discipline vs. one that's automated and reliable.

### Tasks

1. **Learn GitHub Actions basics**
   - Workflow files (`.github/workflows/*.yml`) and YAML syntax
   - Triggers: when do workflows run? (push, pull_request, manual)
   - Jobs and steps: the building blocks of a workflow
   - Secrets management: how to store API keys and credentials

2. **Create CI workflow**
   - Triggers on every push and pull request
   - Install dependencies with Bun
   - Run Oxlint
   - Run TypeScript type checking
   - Run Vitest tests (with test database in CI, including accessibility tests)
   - Build the application
   - Run Lighthouse CI accessibility audit (fail below threshold score)
   - Run Playwright E2E tests

3. **Create CD workflow**
   - Triggers on push to `main` (after CI passes)
   - Build Docker image(s)
   - Push to Azure Container Registry
   - Deploy to staging environment automatically
   - Deploy to production with manual approval gate

4. **Environment management**
   - GitHub Environments (staging, production)
   - Environment-specific secrets
   - Deployment protection rules (required reviewers for production)

### Deliverables

- [ ] CI workflow running on all pushes and PRs
- [ ] All checks must pass before merging
- [ ] Automatic deployment to staging on merge to `main`
- [ ] Manual approval gate for production deployment
- [ ] Secrets properly managed (no credentials in code)
- [ ] Accessibility score enforced in CI (Lighthouse CI)

### Key Learnings

- What CI/CD is and why every professional team uses it
- GitHub Actions workflow syntax and structure
- How to run tests in a CI environment (including databases)
- Environment and secrets management
- Deployment strategies and approval workflows
- The confidence that comes from automated checks

### Example Workflow Structure

```yaml
# .github/workflows/ci.yml
name: CI
on: [push, pull_request]

jobs:
  lint-and-typecheck:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: oven-sh/setup-bun@v2
      - run: bun install
      - run: bun run lint
      - run: bun run typecheck

  test:
    runs-on: ubuntu-latest
    services:
      postgres:
        image: postgres:16
        env:
          POSTGRES_PASSWORD: test
        ports:
          - 5432:5432
    steps:
      - uses: actions/checkout@v4
      - uses: oven-sh/setup-bun@v2
      - run: bun install
      - run: bun run test

  e2e:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: oven-sh/setup-bun@v2
      - run: bun install
      - run: bunx playwright install --with-deps
      - run: bun run test:e2e
```

```yaml
# .github/workflows/deploy.yml
name: Deploy
on:
  push:
    branches: [main]

jobs:
  deploy-staging:
    runs-on: ubuntu-latest
    environment: staging
    steps:
      # Build and push Docker image, deploy to Azure

  deploy-production:
    needs: deploy-staging
    runs-on: ubuntu-latest
    environment: production  # Requires manual approval
    steps:
      # Deploy to production
```

---

## Phase 10: Cloud Deployment

**Goal:** Deploy the application to Azure with proper infrastructure, including staging and production environments.

### Why Azure?

- Strong container support (Azure Container Apps)
- Managed PostgreSQL available (Azure Database for PostgreSQL)
- Good free tier and student credits for learning
- Excellent documentation
- Valuable enterprise skill — widely used in industry
- The concepts transfer to AWS (ECS, RDS) or GCP (Cloud Run, Cloud SQL)

### Environment Architecture

```
GitHub
  ├── Feature Branch → Pull Request → Main Branch
  │                                     │
  │                                     │
  └────────── CI Tests ─────────────────┘
                                         │
                                         ▼
Azure
  ├── STAGING ENV
  │   ├── Container App (API + Client)
  │   ├── PostgreSQL Flexible Server
  │   └── URL: staging-kiromoku.azurecontainerapps.io
  │
  └── PRODUCTION ENV (Manual Approval Required)
      ├── Container App (API + Client)
      ├── PostgreSQL Flexible Server
      └── URL: kiromoku.app (custom domain, optional)
```

### Tasks

1. **Learn Azure fundamentals**
   - Resource groups and how Azure organizes resources
   - Azure Container Registry (ACR) — where Docker images live
   - Azure Container Apps — serverless container hosting
   - Azure Database for PostgreSQL Flexible Server

2. **Set up Azure infrastructure**
   - Create resource groups (staging, production)
   - Set up Azure Container Registry
   - Provision PostgreSQL databases for each environment
   - Configure networking and security rules

3. **Configure deployments**
   - Docker image builds in GitHub Actions
   - Push images to Azure Container Registry
   - Deploy to Azure Container Apps from images
   - Environment variable and secret management in Azure

4. **Set up staging environment**
   - Automatic deploy on merge to `main`
   - Separate database from production
   - Used for testing and verification

5. **Set up production environment**
   - Manual approval required for deployment
   - Custom domain configuration (optional but professional)
   - SSL/TLS certificates (auto-managed by Azure)
   - Database backups configured

6. **Infrastructure as Code (stretch goal)**
   - Azure Bicep or Terraform
   - Version control your infrastructure definitions
   - Reproducible environment creation

### Deliverables

- [ ] Azure account set up and organized
- [ ] Staging environment auto-deploying from `main`
- [ ] Production environment with manual approval gate
- [ ] PostgreSQL databases provisioned for both environments
- [ ] SSL/TLS working
- [ ] Database backups enabled
- [ ] Monitoring and logs accessible

### Key Learnings

- Cloud infrastructure concepts and vocabulary
- Container orchestration basics
- Environment management (staging vs. production)
- Secrets and configuration in the cloud
- Cost awareness and budget alerts
- DNS and custom domains (if pursued)

### Cost Considerations

| Service | Staging | Production | Notes |
|---------|---------|------------|-------|
| Container Apps | ~$0–20/mo | ~$20–50/mo | Scales to zero when idle |
| PostgreSQL Flexible | ~$15/mo | ~$30/mo | Basic tier (Burstable B1ms) |
| Container Registry | ~$5/mo | Shared | Basic tier sufficient |
| Custom Domain | — | ~$12/year | Optional but professional |

*Estimate: ~$50–100/month for both environments at low usage. Set budget alerts.*

### Staging vs. Production

| Aspect | Staging | Production |
|--------|---------|------------|
| Deploy trigger | Auto on merge to `main` | Manual approval |
| Database | Separate, can be reset freely | Protected, backed up |
| Data | Seed/test data | Real user data |
| Access | You only | Public |
| Monitoring | Basic logging | Full alerting |
| Purpose | Verify before going live | Serve real users |

---

## Learning Objectives by Phase

| Phase | Primary Skills | Secondary Skills |
|-------|---------------|------------------|
| 1 | Bun runtime, TypeScript config, project structure | Tooling (Oxlint/Oxfmt), Linear setup |
| 2 | Docker, Prisma schema design | Containerization, migrations, ERD thinking |
| 3 | Waku API routes, REST API design | Prisma queries, validation, error handling |
| 4 | AuthN vs. AuthZ, Better Auth integration | Sessions, cookies, security, middleware patterns |
| 5 | Third-party API integration | Rate limiting, proxy pattern, typing external data |
| 6 | React + Waku, Tailwind, shadcn/ui | Server/client components, design system implementation |
| 7 | Full-stack feature building, polish | UX patterns, accessibility, responsive design |
| 8 | Testing strategies and tooling | Vitest, Playwright, mocking, test architecture |
| 9 | GitHub Actions, CI/CD concepts | Automation, workflow design, environment management |
| 10 | Azure cloud deployment | Infrastructure, containers in production, cost management |

---

## Resources

### Documentation

- [Bun Documentation](https://bun.sh/docs)
- [TypeScript Handbook](https://www.typescriptlang.org/docs/handbook/)
- [Hono Documentation](https://hono.dev/docs/)
- [Prisma Documentation](https://www.prisma.io/docs)
- [Docker Getting Started](https://docs.docker.com/get-started/)
- [PostgreSQL Documentation](https://www.postgresql.org/docs/)
- [Waku Documentation](https://waku.gg/)
- [React Documentation](https://react.dev/)
- [Tailwind CSS Documentation](https://tailwindcss.com/docs)
- [shadcn/ui Documentation](https://ui.shadcn.com/)
- [OXC Documentation](https://oxc.rs/)
- [Vitest Documentation](https://vitest.dev/)
- [Playwright Documentation](https://playwright.dev/docs/intro)
- [GitHub Actions Documentation](https://docs.github.com/en/actions)
- [Azure Container Apps Documentation](https://learn.microsoft.com/en-us/azure/container-apps/)
- [Better Auth Documentation](https://www.better-auth.com/docs)
- [MAL API Reference](https://myanimelist.net/apiconfig/references/api/v2)

### Accessibility

- [WCAG 2.1 Quick Reference](https://www.w3.org/WAI/WCAG21/quickref/) — Official accessibility guidelines
- [MDN Accessibility](https://developer.mozilla.org/en-US/docs/Web/Accessibility) — Comprehensive accessibility tutorials
- [A11y Project Checklist](https://www.a11yproject.com/checklist/) — Practical accessibility checklist
- [Radix UI Accessibility](https://www.radix-ui.com/primitives/docs/overview/accessibility) — How shadcn/ui's primitives handle accessibility
- [WebAIM Articles](https://webaim.org/articles/) — In-depth accessibility articles
- [axe DevTools Browser Extension](https://www.deque.com/axe/devtools/) — Accessibility testing in browser
- [WAVE Browser Extension](https://wave.webaim.org/extension/) — Visual accessibility evaluation
- [Screen Reader Testing Guide](https://webaim.org/articles/screenreader_testing/) — How to test with screen readers

### Recommended Reading

- [The Twelve-Factor App](https://12factor.net/) — Principles for building modern applications
- [REST API Design Best Practices](https://stackoverflow.blog/2020/03/02/best-practices-for-rest-api-design/)
- [Prisma Best Practices](https://www.prisma.io/docs/orm/prisma-client/setup-and-configuration/generating-prisma-client)

---

## Notes

- This document is a living plan. Phases may adjust as we learn and grow.
- Time estimates are intentionally omitted — we go at the pace of understanding, not arbitrary deadlines.
- Each phase should end with working code and solid comprehension before moving on.
- Cloud costs are estimates — monitor usage and set budget alerts early.
- Architectural decisions are documented in the [Architecture Decisions](#architecture-decisions) section and should be updated as the project evolves.

---

*Document created: February 2026*
*Last updated: February 2026*
