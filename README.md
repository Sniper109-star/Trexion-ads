# Trexion Ads

Next.js 16 starter template with built-in database support, Google Analytics/Meta Pixel tracking, and opinionated developer experience for AI-assisted shipping.

## What's Included

- **App Router** on Next.js 16 with TypeScript and Tailwind CSS 4
- **Database layer** with Drizzle ORM + SQLite and generated migrations
- **Ads / tracking** component wiring in the root layout
- **CI workflow** for typecheck, lint, and build
- **Memory bank** for preserving project context across sessions

## Prerequisites

- [Bun](https://bun.sh) >= 1.3
- Node.js >= 20

## Getting Started

```bash
bun install
```

## Scripts

```bash
bun dev           # Start development server
bun build         # Production build
bun start         # Start production server
bun lint          # Run ESLint
bun typecheck     # Run TypeScript check
bun db:generate   # Generate Drizzle migrations from src/db/schema.ts
bun db:migrate    # Apply migrations
```

## Environment Variables

Create a `.env.local` file in the project root with the values needed for your environment:

```bash
NEXT_PUBLIC_GA_ID=
NEXT_PUBLIC_META_PIXEL_ID=
```

If your environment supplies `DB_URL` or `DB_TOKEN`, the database layer uses them automatically. For local SQLite, no extra env vars are required.

## Database

Schema: `src/db/schema.ts`
Client: `src/db/index.ts`
Migrations: `src/db/migrations/`

Example usage in Server Components / Server Actions:

```ts
import { db } from "@/db";
import { users } from "@/db/schema";
import { eq } from "drizzle-orm";

const all = await db.select().from(users);
const one = await db.select().from(users).where(eq(users.id, 1));
await db.insert(users).values({ name: "John", email: "john@example.com" });
```

## Tracking

- Component: `src/components/Tracking.tsx`
- Wired into `src/app/layout.tsx`
- Loads conditionally based on `NEXT_PUBLIC_GA_ID` and `NEXT_PUBLIC_META_PIXEL_ID`

## Project Structure

```
├── src/app/                  # App Router pages
│   ├── layout.tsx
│   ├── page.tsx
│   ├── globals.css
│   └── favicon.ico
├── src/components/           # Reusable components
│   └── Tracking.tsx
├── src/db/                   # Database layer
│   ├── schema.ts
│   ├── index.ts
│   ├── migrate.ts
│   └── migrations/
├── .github/workflows/ci.yml  # CI pipeline
├── drizzle.config.ts
├── next.config.ts
├── package.json
├── bun.lock
├── tsconfig.json
├── postcss.config.mjs
├── eslint.config.mjs
├── .env.example
└── README.md
```

## Portability

All app-scoped configuration and package artifacts are committed:
- `package.json`, `bun.lock`, `tsconfig.json`
- `.github/workflows/ci.yml`
- `.env.example` rather than `.env` files

Cloning this repo plus `bun install` is sufficient to run.

## Development Notes

- Use Server Components by default. Add `"use client"` only when necessary.
- Do not run `bun db:migrate` manually in this environment. Local SQLite works, but platform migration behavior is sandbox-specific.
- After schema changes, run `bun db:generate` to regenerate migrations.

## License

MIT
