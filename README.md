# kiromoku

> A modern, minimal anime and manga cataloguing application.

**kiromoku** is a personal cataloguing application for anime and manga enthusiasts. Users can browse content via the MyAnimeList API, add items to their personal lists, track their progress, and maintain a curated archive of what they've watched, read, or want to experience.

## ✨ Project Status

🚧 **Currently in development** — This project is being built as a learning opportunity and portfolio piece, following professional software engineering practices.

## 📖 About

### Etymology

- **Kiro** — From "kiroku" (記録), meaning "record" or "archive"
- **Moku** — From "mokuroku" (目録), meaning "catalogue" or "list"

### Core Features

- 🔐 User registration and authentication
- 🔍 Browse anime and manga via MyAnimeList API
- 📝 Add items to personal "Watch List" or "Read List"
- 📊 Track status: Planning, In Progress, Completed, Dropped
- 💭 Personal notes and ratings
- 🎨 Minimal, intentional user interface

### Design Philosophy

The application embodies a **minimal, calm, and intentional** aesthetic:

- **Minimal** — No clutter, no unnecessary elements
- **Clean** — Generous whitespace, clear typography
- **Down to earth** — Organic, natural feeling
- **Calm** — No aggressive colors or animations
- **Cozy** — Warm, inviting, like a personal reading nook
- **Intentional** — Every element serves a purpose

## 🛠️ Tech Stack

### Frontend

- **TypeScript** — Type safety across the full stack
- **React** — UI component library
- **Waku** — Minimal React framework with React Server Components
- **Tailwind CSS** — Utility-first styling
- **shadcn/ui** — Accessible, customizable component primitives

### Backend

- **Hono** — Lightweight, fast web framework for API routes
- **Better Auth** — Modern authentication library
- **Prisma** — Type-safe ORM for PostgreSQL
- **PostgreSQL** — Relational database

### Development Tools

- **Bun** — JavaScript/TypeScript runtime and package manager
- **OXC (Oxlint)** — Fast linting written in Rust
- **OXC (Oxfmt)** — Fast formatting written in Rust
- **Vitest** — Unit and integration testing
- **Playwright** — End-to-end browser testing
- **Testing Library** — React component testing

### DevOps

- **Docker / Docker Compose** — Containerization
- **GitHub Actions** — CI/CD pipeline
- **Azure** — Cloud hosting platform

## 📋 Prerequisites

Before you begin, ensure you have the following installed:

- **[Bun](https://bun.sh/)** (latest version recommended) — JavaScript/TypeScript runtime
- **[Docker](https://www.docker.com/)** — For running PostgreSQL
- **[Git](https://git-scm.com/)** — Version control

## 🚀 Getting Started

### Installation

1. **Clone the repository**

   ```bash
   git clone https://github.com/Furrer-Tech/kiromoku.git
   cd kiromoku
   ```

2. **Install dependencies**

   ```bash
   bun install
   ```

3. **Set up environment variables**

   Create a `.env` file in the root directory:

   ```env
   DATABASE_URL="postgresql://YOUR_USERNAME:YOUR_PASSWORD@localhost:5432/kiromoku"
   # Add other environment variables as needed
   ```

4. **Start the database** (using Docker Compose, when available)

   ```bash
   docker-compose up -d
   ```

5. **Run database migrations** (when Prisma is set up)

   ```bash
   bun prisma migrate dev
   ```

### Development

Start the development server:

```bash
bun dev
```

The application will be available at `http://localhost:3000` (or the configured port).

### Building for Production

```bash
bun build
```

### Running the Production Build

```bash
bun start
```

## 📁 Project Structure

```
kiromoku/
├── .github/              # GitHub configuration
├── docs/                 # Project documentation
│   └── KIROMOKU_PROJECT_PLAN.md
├── public/               # Static assets
├── src/
│   ├── components/       # React components
│   ├── pages/            # Waku file-based routing
│   └── styles.css        # Global styles
├── CLAUDE.md             # AI assistant context
├── package.json          # Project dependencies and scripts
├── tsconfig.json         # TypeScript configuration
└── waku.config.ts        # Waku framework configuration
```

## 🧪 Testing

Run unit and integration tests:

```bash
bun test
```

Run end-to-end tests with Playwright (when configured):

```bash
bun playwright test
```

## 📚 Documentation

For detailed information about the project:

- **[Project Plan](docs/KIROMOKU_PROJECT_PLAN.md)** — Complete project plan, architecture decisions, and phase breakdown
- **[CLAUDE.md](CLAUDE.md)** — Project context for AI assistants

## 🤝 Contributing

This is a personal learning project, but feedback and suggestions are welcome! Please feel free to:

- Open an issue for bugs or feature suggestions
- Submit a pull request with improvements

### Code Standards

- TypeScript strict mode enabled
- All code linted with Oxlint, formatted with Oxfmt
- Meaningful variable and function names — no abbreviations unless universally understood
- Error handling is not optional — every API route, every async operation
- Accessibility is not optional — semantic HTML, ARIA where needed, keyboard navigation
- Comments explain _why_, not _what_ — the code should explain itself

## 📝 License

This project is private and intended as a portfolio piece.

## 🔗 Links

- **Project Management** — [Linear](https://linear.app/)
- **MyAnimeList API** — [API Documentation](https://myanimelist.net/apiconfig/references/api/v2)

---

**Built with ❤️ as a learning journey in modern web development.**
