# SETUP.md — Smart Timetable Auto-Generator

Complete installation and setup guide, as built on Day 3. Follow this to get the project running from a fresh clone.

## Prerequisites

| Tool | Version used | Why it's needed |
|---|---|---|
| Node.js | v24.12.0 (18+ required) | JavaScript runtime that powers Vite, npm, and the dev server |
| npm | bundled with Node | Package manager — installs and manages all project dependencies |
| VS Code | any recent version | Code editor used throughout this project |
| Git | any recent version | Version control, connects local project to GitHub |
| A Supabase account | free tier | Hosts our Postgres database, Auth, and REST API |

## 1. Clone the Repository

```
git clone https://github.com/your-username/smart-timetable-automator.git
cd smart-timetable-automator
```

## 2. Install Dependencies

```
npm install
```

This installs everything listed in `package.json`, including:
- `react`, `react-dom` — the core UI framework
- `react-router-dom` — page navigation
- `@supabase/supabase-js` — database/auth client
- `tailwindcss`, `@tailwindcss/vite` — styling
- `vite`, `@vitejs/plugin-react` — build tool and dev server

## 3. Configure Environment Variables

1. Create a file named `.env.local` in the project root (same level as `package.json`).
2. Add the following, using your own Supabase project's values (found in Supabase dashboard → **Settings → Data API**):
   ```
   VITE_SUPABASE_URL=https://your-project-ref.supabase.co
   VITE_SUPABASE_ANON_KEY=your-anon-public-key
   ```
3. This file is already listed in `.gitignore` (`.env*`) — it will never be committed.

See `ENVIRONMENT.md` for the full list of variables and what each one does.

## 4. Set Up the Database

1. Open your Supabase project → **SQL Editor** → **New query**.
2. Run the schema script found in `supabase/schema.sql` (matches `SCHEMA.md`).
3. Confirm you see "Success. No rows returned."

*(Seed data — populating the 20 faculty, 8 subjects, 6 sections — is scheduled for Day 4, not required for today's foundation check.)*

## 5. Run the Project Locally

```
npm run dev
```

Visit the printed local URL (typically `http://localhost:5173`). You should see the Student View placeholder page load, confirming the app, Tailwind styling, routing, and Supabase connection are all working together.

## 6. Verify a Production Build Works

```
npm run build
```

Should complete with no errors and a "built in ___ms" success message.

## Available Routes (placeholders as of Day 3)

| Route | Page |
|---|---|
| `/` | Student View (home) |
| `/admin/login` | Admin Login |
| `/admin/dashboard` | Admin Dashboard |
| `/faculty/login` | Faculty Login |
| `/faculty/dashboard` | Faculty Dashboard |
| `/student` | Student View |

## Troubleshooting

- **Blank white page, no styling:** check that `vite.config.js` includes the `tailwindcss()` plugin and `src/index.css` contains `@import "tailwindcss";`.
- **"Could not find the table" error:** the database schema hasn't been run yet in Supabase — see Step 4 above.
- **Environment variables undefined:** confirm variable names start with `VITE_` exactly, and that you restarted `npm run dev` after creating/editing `.env.local` (Vite only reads env files at startup).
