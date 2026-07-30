# Day 9 Summary — Release Readiness Review

## Scope Decision Made Today

Today's prompt requested both a full QA pass AND deployment. Confirmed with you to keep these separate as originally scheduled: full QA/hardening today, deployment tomorrow (Day 10) — so neither gets rushed. No deployed URL exists yet as of today; repo remains `https://github.com/Dhannesh/smart-timetable-automator`.

## ✅ What Was Completed Today

### Security
- **Closed a real production risk:** removed the Day-5 "public write dev" RLS policies on `timetable_slots` (previously allowed anyone with the public anon key to write/delete the entire timetable with no login). Replaced with admin-only insert/update/delete policies; public read access preserved.

### Reliability & Error Handling
- **`ErrorBoundary.jsx`** — catches uncaught React errors app-wide, shows a friendly recovery screen instead of a blank white page.
- **`NotFound.jsx` + catch-all route** — proper 404 page for unmatched URLs instead of a blank screen.
- **`supabaseClient.js`** — validates environment variables on load with a clear, actionable error message instead of a cryptic runtime failure.

### Code Quality
- **`useAsyncData.js`** — new shared hook eliminating duplicated loading/error-state boilerplate across `AdminDashboard`, `FacultyDashboard`, and `StudentView` (flagged as a "someday" cleanup back on Day 6; done today before launch).

### Accessibility
- Added `aria-live="polite"` regions on status/loading messages so screen readers announce updates.
- Added `aria-busy` on the Generate button during async operations.
- Added `aria-hidden="true"` on decorative spinner icons.
- Added proper `<label>` elements (including screen-reader-only labels via `sr-only`) for all form selects that previously relied on placeholder text alone.

### Branding & Metadata
- Proper page `<title>`, meta description, Open Graph and Twitter Card tags in `index.html`.
- Custom favicon (`favicon.svg`) matching the app's emerald/navy visual theme.
- **`LICENSE`** (MIT) added.
- **`README.md`** rewritten to reflect what's actually built (correct tech stack including the Groq switch, real setup steps, accurate feature list) rather than the Day-1 aspirational version.

## 🐞 Critical Bug Found and Fixed Today

**Leave-request resolutions occasionally targeted the wrong slot.** You reported that after applying 3 leave requests as Dr. Sharma and resolving them via the agent, one slot incorrectly showed as "not scheduled" even though it clearly was.

**Root cause:** "Resolve via Agent" was sending an auto-generated sentence through the same Groq LLM parsing path used for genuinely free-typed admin requests — even though we already had the exact `facultyId`, `dayIndex`, and `period` directly from the database. LLM parsing, even at temperature 0, has small variance; it occasionally mis-extracted the day or period from the auto-generated text, causing the agent to check the wrong slot.

**Fix:** added a new `validateAbsenceActionDirect()` function that takes known structured data directly — no LLM involved at all. `AgentRequestBox` now uses this direct path for leave-request resolutions (`runAgentDirect`), and reserves the LLM-parsing path (`runAgentFromText`) exclusively for genuinely free-typed messages. This is both a reliability fix and a small performance win (one fewer network call) for the leave-request flow specifically.

This is exactly the kind of bug a dedicated QA day is meant to catch — it would have been embarrassing to discover in front of a live demo audience.

## 🧪 Verified Results

- Re-tested the exact failure scenario: 3 leave requests from Dr. Sharma, resolved sequentially via the agent — all 3 now correctly find and reassign the right slot, confirmed by you.
- Full regression across all previously built features: generation, typed agent requests, leave-marking, resolve-via-agent, student view, Admin/Faculty/Student auth — all working correctly after today's changes.
- RLS lockdown verified: Admin can still generate/edit (authenticated), public read access unaffected.

## 🚧 What's Ready for Tomorrow

A functionally complete, hardened v1.0 with:
- No known bugs remaining
- Production-appropriate security (RLS locked down)
- Graceful error/404 handling
- Clean, de-duplicated code
- Baseline accessibility support
- Professional branding, README, and license

## 🎯 Tomorrow's Objective (Day 10 — Final Day)

Deploy to Vercel, configure production environment variables, verify the live deployed app end-to-end, and wrap up the capstone with final documentation and a closing LinkedIn post.

## Free-Tools Compliance

100% free-tier services used today: Supabase (free tier, RLS policies only — no new paid features). No new AI API calls were needed for today's hardening work.
