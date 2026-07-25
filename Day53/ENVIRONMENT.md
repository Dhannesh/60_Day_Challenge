# ENVIRONMENT.md — Smart Timetable Auto-Generator

Reference for every environment variable, tool, and configuration file used in this project.

## Environment Variables

All environment variables live in `.env.local` (never committed — see `.gitignore`). Vite only exposes variables prefixed with `VITE_` to the frontend code.

| Variable | Purpose | Where to find it |
|---|---|---|
| `VITE_SUPABASE_URL` | The base URL of your Supabase project's API | Supabase dashboard → Settings → Data API → "Project URL" |
| `VITE_SUPABASE_ANON_KEY` | Public, RLS-governed key used by the browser to talk to Supabase | Supabase dashboard → Settings → Data API → "anon public" key |

**Added on a later day (not yet needed):**

| Variable | Purpose | When it's added |
|---|---|---|
| `VITE_ANTHROPIC_API_KEY` (or equivalent) | Used by the AI Reassignment Agent to call Claude | Day 6, when the agent's natural-language parsing is built |

## Development Tools

| Tool | Version (as configured) | Role |
|---|---|---|
| Node.js | v24.12.0 | JavaScript runtime |
| npm | bundled with Node | Package management |
| Vite | v8.1.5 | Dev server + build tool |
| VS Code | latest | Code editor |
| Git | latest | Version control |

## Key Configuration Files

### `vite.config.js`
Registers the React plugin (JSX support) and the Tailwind CSS v4 Vite plugin. This is what makes `@import "tailwindcss";` in `index.css` actually work.

### `src/index.css`
Single-line file: `@import "tailwindcss";` — loads all of Tailwind's utility classes project-wide. No other global CSS is used at this stage.

### `.gitignore`
Confirmed to include `.env*`, ensuring `.env.local` (and any future `.env` variants) are never pushed to GitHub. Also excludes `node_modules`, `dist`, and editor-specific files.

### `src/router.jsx`
Defines all top-level routes using `react-router-dom`'s `createBrowserRouter`. This is the single source of truth for what URL maps to what page component.

### `src/main.jsx`
The Vite entry point. Renders `<RouterProvider router={router} />` instead of a single `<App />` component, handing control of "what's on screen" to the router based on the current URL.

## Supabase Project Configuration

- **Row Level Security (RLS):** enabled on all master-data and timetable tables, with a temporary "public read" policy on each (`select using (true)`) to allow easy development. This will be tightened on Day 9 per the Implementation Blueprint (e.g. restricting `leave_requests` and write access to authenticated roles only).
- **Auth provider:** Supabase Auth (email/password) — not yet wired into the app as of Day 3; scheduled for Day 5.

## Where Secrets Live at Deployment Time (forward reference for Day 10)

When the app is deployed to Vercel, `VITE_SUPABASE_URL`, `VITE_SUPABASE_ANON_KEY`, and the Anthropic key will be set as **Environment Variables in the Vercel project dashboard** — not committed to the repository, and not hardcoded anywhere in source files.
