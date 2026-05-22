# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project overview

A full-stack broadband plan comparison site for China's three major ISPs (中国移动/中国联通/中国电信). Apple-style design language with glassmorphism effects, dark gradient hero, and scroll-triggered animations.

## Commands

```bash
# Install all dependencies (root + client + server)
npm install && cd client && npm install && cd ../server && npm install

# Start both frontend and backend concurrently (preferred for dev)
npm run dev

# Start frontend only (Vite dev server on :5173)
cd client && npm run dev

# Start backend only (Express on :3001)
cd server && node index.js

# Lint frontend
cd client && npm run lint

# Build frontend for production
cd client && npm run build
```

## Architecture

**Frontend** (`client/`): React 19 + Vite 8 + Tailwind CSS 4 + Framer Motion.
- `App.jsx` is a static page assembly — it hardcodes the three operator sections with their IDs, colors, and display names rather than fetching the operator list from the API.
- Each `OperatorSection` fetches its own data from `/api/plans?operator=...` on mount. The "中国联通" section uses a dark background variant; the others use light.
- `PlanCard` uses Framer Motion `whileInView` for staggered scroll animations. `ScrollReveal` wraps section headings with the same pattern.
- `ComparisonTable` fetches all plans once and renders both a desktop table and mobile card layout (toggled via Tailwind `hidden md:block` / `md:hidden`).
- Vite proxies `/api` to `localhost:3001` in dev — no CORS issues during development.

**Backend** (`server/`): Express 5 (CommonJS) on port 3001.
- `index.js` mounts two route groups: `/api/plans` (delegated to `routes/plans.js`) and `/api/operators` (inline handler that deduplicates plans by operator name).
- `routes/plans.js` supports `GET /` (with optional `?operator=` filter) and `GET /:id`.
- Data lives in `data/plans.json` — 9 plans (3 operators × 3 speed tiers: 200M/500M/1000M). IDs follow `cm-`/`cu-`/`ct-` prefixes.

## Key patterns

- **No router**: The frontend is a single-page site with anchor-based navigation (`#mobile`, `#unicom`, `#telecom`, `#compare`). No React Router.
- **Tailwind CSS 4**: Uses the new `@import "tailwindcss"` syntax and `@theme` block for custom design tokens (Apple-style color palette and font stack). No `tailwind.config.js`.
- **Server is CommonJS** (`type: "commonjs"`), **client is ESM** (`type: "module"`). Server uses `require()`, client uses `import`.
- **Data is read-only**: No POST/PUT/DELETE endpoints. The JSON file acts as a static database.

## Deployment (Netlify)

`netlify.toml` is configured at the project root. The Express backend is replaced by a single Netlify Function (`netlify/functions/api.js`) that serves the same three API endpoints from a local copy of `plans.json`.

Build settings (set automatically via `netlify.toml`):
- **Build command**: `cd client && npm install && npm run build`
- **Publish directory**: `client/dist`
- **Functions directory**: `netlify/functions`

The `/api/*` path is rewritten to `/.netlify/functions/api/*` via a 200-status redirect, so the client-side fetch calls work without modification in production.

If you add new plans or change the data, both `server/data/plans.json` and `netlify/functions/data/plans.json` need to be updated (they are independent copies).
