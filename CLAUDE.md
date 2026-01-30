# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Commands

```bash
pnpm dev          # Start development server
pnpm build        # Build for production
pnpm check        # Run TypeScript/Svelte type checks
pnpm lint         # Run Prettier check + ESLint
pnpm format       # Format code with Prettier
pnpm test         # Run Playwright E2E tests (builds first, runs on port 4173)
```

## Architecture

**SvelteKit 2 + Svelte 5** static site using `@sveltejs/adapter-static`. Builds output to `build/` with SPA fallback (`200.html`).

### Path Aliases

- `$lib` → `src/lib`
- `$components` → `src/lib/components`
- `$schemas` → `src/lib/schemas`

### UI Components

Uses **shadcn-svelte** with Bits UI primitives. Components live in `src/lib/components/ui/`. Styling uses:

- Tailwind CSS v4
- `tailwind-variants` for component variants
- `cn()` utility from `$lib/utils.ts` for class merging

### Server Hooks (`src/hooks.server.ts`)

- Sentry error tracking initialization
- Auth middleware: populates `event.locals.user` and protects `/protected/*` routes
- Uses `sequence()` to chain Sentry handle with auth handle

### Client Hooks (`src/hooks.client.ts`)

- Sentry client-side initialization with session replay

### Forms

Uses **sveltekit-superforms** with **Zod** for validation.

### HTTP Client

Axios instance configured in `src/lib/axios.ts`.

## Testing

E2E tests in `e2e/` directory using Playwright. Tests must import from `e2e/_shared/app-fixtures.ts` (not directly from `@playwright/test`) to get V8 code coverage.

```bash
pnpm test:show-report      # View test results
pnpm coverage:show-report  # View V8 coverage report
```

## Git Hooks (Husky)

- **pre-commit**: lint-staged runs Prettier + ESLint on staged files
- **pre-push**: runs full E2E test suite
