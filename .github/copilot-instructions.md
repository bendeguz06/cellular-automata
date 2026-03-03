# Copilot Instructions for Cellular Automata

## Project Overview

This is a Conway's Game of Life implementation built with Bun, React 19, and TypeScript. It combines a full-stack setup with a Bun server handling both static file serving and API routes, and a React frontend for the cellular automaton simulation.

## Build, Test, and Lint Commands

**Development:**
```bash
bun install          # Install dependencies
bun dev             # Start dev server with hot module reloading at http://localhost:3000
```

**Production:**
```bash
bun build ./src/index.html --outdir=dist --sourcemap --target=browser --minify --define:process.env.NODE_ENV='\"production\"' --env='BUN_PUBLIC_*'
bun start           # Run production build
```

## Architecture

### Full-Stack Structure
- **Backend:** Bun server (`src/index.ts`) handles:
  - Static file serving (React frontend)
  - API routes using Bun's route syntax (e.g., `/api/hello`, `/api/hello/:name`)
  - Hot module reloading and browser console echo in development mode

- **Frontend:** React 19 app (`src/App.tsx`) runs a Conway's Game of Life simulation:
  - 100x100 grid with 8px cells
  - Interactive cell toggling
  - Play/pause controls and adjustable FPS (1-60)
  - Implements Game of Life rules: cell birth at 3 neighbors, survival at 2-3 neighbors

- **Entry Point:** `src/frontend.tsx` bootstraps React with hot module reloading support

### Key Tech Stack
- **Runtime:** Bun (build tool, server, package manager, runtime)
- **UI Framework:** React 19 with JSX
- **Language:** TypeScript with strict mode enabled
- **Build:** Bun's native bundler with minification for production
- **Path aliases:** `@/*` resolves to `./src/*` (configured in `tsconfig.json`)

## Key Conventions

### TypeScript Configuration
- `strict: true` is enforced; use non-null assertion operators (`!`) only when necessary
- JSX is set to `react-jsx` (automatic runtime, no React import needed)
- Module resolution uses `bundler` mode for ESNext target
- Path aliases configured: use `@/` for imports from `src/`

### Development Mode Features
- Hot module reloading (HMR) enabled automatically
- Browser console logs are echoed to the server console
- Environment variables prefixed with `BUN_PUBLIC_*` are available in the browser

### React Component Patterns
- Components use functional syntax with hooks
- Inline styles are used throughout (no separate CSS files for component-specific styling)
- React.StrictMode is enabled in production
- Grid state is managed with `useState`; simulation uses `setInterval` cleanup in `useEffect`

### API Route Pattern
Routes in `src/index.ts` use Bun's route object syntax:
```typescript
routes: {
  "/api/endpoint": {
    async GET(req) { /* ... */ },
    async PUT(req) { /* ... */ },
  },
  "/api/dynamic/:param": async req => { /* req.params.name */ },
}
```

### Build Output
- Production build outputs to `dist/` directory
- Sourcemaps are generated for debugging
- Minification is enabled in production
- Browser target removes Node.js APIs
