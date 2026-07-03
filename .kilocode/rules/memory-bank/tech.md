# Technical Context: Next.js Starter Template

## Technology Stack

| Technology   | Version | Purpose                         |
| ------------ | ------- | ------------------------------- |
| Next.js      | 16.x    | React framework with App Router |
| React        | 19.x    | UI library                      |
| TypeScript   | 5.9.x   | Type-safe JavaScript            |
| Tailwind CSS | 4.x     | Utility-first CSS               |
| Drizzle ORM  | 0.45.x  | SQLite database layer           |
| Bun          | Latest  | Package manager & runtime       |

## Development Environment

### Prerequisites

- Bun installed (`curl -fsSL https://bun.sh/install | bash`)
- Node.js 20+ (for compatibility)

### Commands

```bash
bun install        # Install dependencies
bun dev            # Start dev server (http://localhost:3000)
bun build          # Production build
bun start          # Start production server
bun lint           # Run ESLint
bun typecheck      # Run TypeScript type checking
bun db:generate    # Generate Drizzle migrations
bun db:migrate     # Run database migrations
```

## Project Configuration

### Next.js Config (`next.config.ts`)

- App Router enabled
- Default settings for flexibility

### TypeScript Config (`tsconfig.json`)

- Strict mode enabled
- Path alias: `@/*` → `src/*`
- Target: ESNext

### Tailwind CSS 4 (`postcss.config.mjs`)

- Uses `@tailwindcss/postcss` plugin
- CSS-first configuration (v4 style)

### ESLint (`eslint.config.mjs`)

- Uses `eslint-config-next`
- Flat config format

### Drizzle ORM (`drizzle.config.ts`)

- SQLite dialect
- Schema: `./src/db/schema.ts`
- Migrations output: `./src/db/migrations`

## Key Dependencies

### Production Dependencies

```json
{
  "next": "^16.1.3", // Framework
  "react": "^19.2.3", // UI library
  "react-dom": "^19.2.3", // React DOM
  "drizzle-orm": "^0.45.2", // ORM for database
  "@kilocode/app-builder-db": "github:Kilo-Org/app-builder-db#main" // DB utilities
}
```

### Dev Dependencies

```json
{
  "typescript": "^5.9.3",
  "@types/node": "^24.10.2",
  "@types/react": "^19.2.7",
  "@types/react-dom": "^19.2.3",
  "@tailwindcss/postcss": "^4.1.17",
  "tailwindcss": "^4.1.17",
  "eslint": "^9.39.1",
  "eslint-config-next": "^16.0.0",
  "drizzle-kit": "^0.31.10"
}
```

## File Structure

```
/
├── .gitignore              # Git ignore rules
├── .env.example            # Environment variable template
├── drizzle.config.ts       # Drizzle ORM config
├── package.json            # Dependencies and scripts
├── bun.lock                # Bun lockfile
├── next.config.ts          # Next.js configuration
├── tsconfig.json           # TypeScript configuration
├── postcss.config.mjs      # PostCSS (Tailwind) config
├── eslint.config.mjs       # ESLint configuration
├── public/                 # Static assets
│   └── .gitkeep
└── src/                    # Source code
    ├── app/                # Next.js App Router
    │   ├── layout.tsx      # Root layout (includes Tracking)
    │   ├── page.tsx        # Home page
    │   ├── globals.css     # Global styles
    │   └── favicon.ico     # Site icon
    ├── components/         # Reusable components
    │   └── Tracking.tsx    # Google Analytics + Meta Pixel
    └── db/                 # Database layer (Drizzle)
        ├── schema.ts       # Table definitions
        ├── index.ts        # Database client
        ├── migrate.ts      # Migration runner
        └── migrations/     # Generated SQL migrations
```

## Technical Constraints

### Starting Point

- Minimal structure - expand as needed
- Database schema can be extended in `src/db/schema.ts`
- Tracking is opt-in via environment variables

### Browser Support

- Modern browsers (ES2020+)
- No IE11 support

### Environment Variables

- `NEXT_PUBLIC_GA_ID` - Google Analytics tracking ID
- `NEXT_PUBLIC_META_PIXEL_ID` - Meta Pixel tracking ID
- `DB_URL` / `DB_TOKEN` - Provided automatically by sandbox

## Performance Considerations

### Image Optimization

- Use Next.js `Image` component for optimization
- Place images in `public/` directory

### Bundle Size

- Tree-shaking enabled by default
- Tailwind CSS purges unused styles
- Tracking scripts load with `afterInteractive` strategy

### Core Web Vitals

- Server Components reduce client JavaScript
- Streaming and Suspense for better UX
- Tracking script loading is non-blocking

## Deployment

### Build Output

- Server-rendered pages by default
- Can be configured for static export

### Database

- SQLite database file managed via app-builder-db
- Migrations run automatically in sandbox after push
- Use `bun db:generate` to create migrations after schema changes
