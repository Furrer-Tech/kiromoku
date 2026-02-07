# Copilot Instructions — kiromoku

## Project Overview

kiromoku is a modern anime and manga cataloguing application built as a learning project and portfolio piece.

## Tech Stack

- **Runtime:** Bun (not Node.js)
- **Language:** TypeScript (strict mode)
- **Frontend:** React with Waku framework, Tailwind CSS, shadcn/ui
- **Backend:** Hono (not Express)
- **Auth:** Better Auth (not Lucia, Auth.js, or hand-rolled auth)
- **ORM:** Prisma with PostgreSQL
- **Testing:** Vitest, Playwright, Testing Library
- **Linting/Formatting:** OXC (Oxlint, Oxfmt) — not ESLint or Prettier
- **CI/CD:** GitHub Actions
- **Cloud:** Azure

## Code Conventions

- Use TypeScript strict mode. Avoid `any` types.
- Use Bun APIs where applicable (e.g., `Bun.serve`, `Bun.file`), not Node.js-specific APIs.
- Use Hono patterns for API routes and middleware, not Express patterns.
- Use Better Auth for all authentication and session management. Do not implement auth from scratch.
- Use Prisma for all database interactions — no raw SQL unless explicitly needed.
- Use Tailwind CSS utility classes for styling. Follow the project's design tokens.
- Use shadcn/ui components as the base for UI elements.
- Prefer named exports over default exports.
- Use descriptive variable and function names. No abbreviations.
- Handle errors explicitly — no silent catches, no unhandled promises.
- Write semantic HTML. Include ARIA attributes where needed.

## File Structure Hints

- Frontend pages live in Waku's routing directory
- API routes use Hono's router
- Database models are defined in `prisma/schema.prisma`
- Shared types should live in a shared location accessible to both frontend and backend
- Tests live alongside or mirror the source structure

## Important Notes

- This is a learning project. The developer is a junior engineer building professional-quality code.
- Prioritize readability and maintainability over cleverness.
- When suggesting code, prefer explicit patterns over implicit/magical ones.
- Always suggest proper error handling and loading states.
- Accessibility matters — suggest ARIA labels, keyboard handlers, and semantic elements.
