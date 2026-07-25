# Day 4 Summary — Core Rule-Based Timetable Generation Engine

## ✅ What Was Completed Today

- **Master data seeded** via CSV import (per your preference, not raw SQL): 1 department, 6 sections (2nd Year A-F), 8 subjects, 6 rooms, 20 faculty, and 48 subject offerings — all correctly linked by ID.
- **`src/engine/constants.js`** — named constants for days/periods, replacing any future magic numbers in the codebase.
- **`src/engine/availability.js`** — builds and manages faculty/room availability matrices (who's free, who's booked, per day/period), plus a daily-load counter for the max-periods-per-day rule.
- **`src/engine/generator.js`** — the core algorithm: orders subject offerings hardest-to-place-first (lab-requiring subjects, then highest lecture count), then places every lecture while enforcing all five hard rules from the PRD.
- **`src/engine/validate.js`** — an independent validation pass that re-checks the generated output from scratch (not reusing the generator's own bookkeeping), acting as a safety net against bugs.
- **`src/engine/testGenerator.js`** — a standalone console test harness (run via `node`, not the browser) that fetches live data from Supabase, runs the engine, and prints a full summary.
- **`dotenv` installed** and configured to read `.env.local`, enabling standalone Node scripts to access Supabase credentials outside the Vite dev server.

## 🧪 Test Results

```
Loaded: 48 subject offerings, 20 faculty, 6 rooms
Total lectures expected: 168
Total lectures placed:   168
Unplaced lectures:       0
✅ VALIDATION PASSED — zero rule violations found.
```

**168/168 lectures placed on the first run, with zero clashes.** This is a strong result — no backtracking or fallback logic was even needed for this dataset.

## 🐞 Issues Debugged Today

1. **`ERR_MODULE_NOT_FOUND`** — Node's ES module loader requires explicit `.js` extensions in relative imports (unlike Vite/the browser, which are more lenient). Fixed by adding `.js` to all internal imports in `generator.js`, `availability.js`, and `validate.js`.
2. **Missing Supabase env vars in the test script** — `dotenv`'s default behavior looks for a file named `.env`, but our project correctly uses `.env.local` (the Vite convention). Fixed by explicitly pointing `dotenv.config()` at `.env.local`.

## 🚧 What's Ready to Build Tomorrow

- A fully proven generation engine, validated against real seeded data with zero errors.
- Clear next step: persist the generated `slots` array into the `timetable_slots` table, and build the derived query functions for section and faculty timetable views (`timetableService.js`).

## 🎯 Tomorrow's Objective

Persist the generated timetable to Supabase (`timetable_slots` table), add a light backtracking/swap step for resilience on tighter datasets, build `getSectionTimetable()` and `getFacultyTimetable()` query functions, and wire up a bare "Generate Timetable" button in the Admin area so the engine can be triggered from the UI for the first time — not just the command line.

## Notes on Scope/Timeline

Because Day 2 became a dedicated system-design day, our day numbering runs one day "behind" the original Day 1 blueprint's Day-by-day labels. Today's work corresponds to the original blueprint's "Day 3: Core Generation Engine (Part 1)" section. No functionality was skipped — the split between "build the engine" (today) and "persist + derive views" (tomorrow) is a clean, natural boundary and keeps each day's scope honest rather than rushed.

Master data was seeded via CSV import through the Supabase Table Editor rather than raw SQL insert statements, per your request — functionally identical, just a different manual process. This is noted here for the record; no code or schema changes were required.
