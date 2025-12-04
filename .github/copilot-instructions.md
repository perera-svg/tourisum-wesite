# Copilot Instructions

## Project Overview

TanStack Start full-stack React app with SSR, using Nitro as the server runtime. Tourism website demo showcasing TanStack ecosystem integration.

## Tech Stack & Key Patterns

### Core Framework

- **TanStack Start** with file-based routing (`src/routes/`)
- **Vite** build system with React Compiler (`babel-plugin-react-compiler`)
- **Nitro** server runtime (see `vite.config.ts`)

### Data Layer

- **Prisma** with PostgreSQL (Prisma Postgres adapter) - schema in `prisma/schema.prisma`
- **tRPC** for type-safe API routes (`src/integrations/trpc/`)
- **TanStack Query** for client-side data fetching
- Database commands use `dotenv -e .env.local` prefix: `pnpm db:migrate`, `pnpm db:push`

### State Management

- **TanStack Store** for global state (see `src/lib/demo-store.ts`)
- **TanStack DB** for local-first reactive collections (`src/db-collections/`)

## File Structure Conventions

### Routes

Routes live in `src/routes/`. File naming:

- `index.tsx` - Index route for directory
- `$paramName.tsx` - Dynamic route segment
- `__root.tsx` - Root layout with providers
- `api.*.ts` - API routes (e.g., `api.trpc.$.tsx` for tRPC handler)

### Components

- `src/components/ui/` - shadcn components (install: `pnpx shadcn@latest add <component>`)
- `src/components/storybook/` - Custom storybook components
- Use `cn()` from `@/lib/utils` for className merging

### Server Functions

Use `createServerFn` from TanStack Start for server-side logic:

```tsx
import { createServerFn } from "@tanstack/react-start";

const getData = createServerFn({ method: "GET" }).handler(async () => {
  /* server logic */
});
```

## Sentry Integration

Error tracking configured in `instrument.server.mjs` and `src/router.tsx`.

Instrument server functions with Sentry spans:

```tsx
import * as Sentry from "@sentry/tanstackstart-react";

Sentry.startSpan({ name: "Operation description" }, async () => {
  // Long-running operation
});
```

## Code Style

- **Biome** for linting/formatting (tabs, double quotes)
- Run `pnpm check` before committing
- Path aliases: `@/` maps to `src/` (via `vite-tsconfig-paths`)
- Environment variables: Add to `src/env.ts` using T3Env, prefix client vars with `VITE_`

## Development Commands

```bash
pnpm dev          # Start dev server (port 3000, with Sentry instrumentation)
pnpm build        # Production build
pnpm test         # Run Vitest tests
pnpm storybook    # Start Storybook on port 6006
pnpm db:studio    # Open Prisma Studio
```

## Key Integration Points

- **WorkOS Auth**: Provider in `src/integrations/workos/provider.tsx`, requires `VITE_WORKOS_CLIENT_ID`
- **AI Chat**: Uses Anthropic Claude via `@ai-sdk/react`, API at `/demo/api/tanchat`
- **tRPC**: Router defined in `src/integrations/trpc/router.ts`, uses superjson transformer

## Demo Files

Files/routes prefixed with `demo` are examples and can be safely deleted or used as reference patterns.
