# Day 6 Summary — Authentication + Faculty Dashboard

## Scope Decision Made Today

Today's session prompt requested "Complete the MVP & Deliver a Working Demo" with deployment — this conflicted with the original Implementation Blueprint, which scheduled Auth + Faculty Dashboard for today, with the AI Agent (Day 7), Leave-Requests (Day 8), and full deployment (Day 10) still ahead. Given the choice, we stuck to the original plan: build Auth + Faculty Dashboard properly today, no deployment yet. This avoided rushing three major features into one day.

## ✅ What Was Completed Today

- **Created real login accounts** in Supabase Auth: 1 Admin (`admin@timetable.test`), 2 Faculty (`sharma@timetable.test`, `verma@timetable.test`), linked to their respective `admin_users`/`faculty` rows via `auth_user_id`.
- **`AuthContext.jsx`** — centralized session + role state (admin/faculty), resolved via database lookups against `admin_users` and `faculty` tables.
- **`ProtectedRoute.jsx`** — new file, redirects unauthenticated or wrong-role users to the appropriate login page.
- **`AdminLogin.jsx` and `FacultyLogin.jsx`** — real login forms replacing Day 3's placeholders, using Supabase Auth's `signInWithPassword`.
- **`Navbar.jsx`** — shared, role-aware top bar showing who's logged in + logout button.
- **`Footer.jsx`** — new file, displays "Built with Claude as part of the AB Talks 60-Day Claude AI Challenge." on Admin, Faculty, and Student pages.
- **`FacultyDashboard.jsx`** — fully implemented: shows own timetable by default, dropdown to browse any other faculty member's timetable, "Back to my timetable" shortcut.
- **`router.jsx` and `main.jsx`** updated to wrap Admin/Faculty dashboards in `ProtectedRoute` and provide `AuthProvider` app-wide.
- **`AdminDashboard.jsx` and `StudentView.jsx`** updated to include the new `Navbar`/`Footer` components (Student keeps its own header, no login).

## 🐞 Issues Debugged Today

1. **Redirect race condition (session vs. role resolution):** Initially, login redirected immediately after `signInWithPassword` resolved, but role-lookup (a separate async database call) hadn't finished — `ProtectedRoute` saw a session with no resolved role yet and bounced back to login. Fixed by moving navigation into a `useEffect` that watches for `role` to actually match the required value before redirecting, instead of navigating immediately after the login call.
2. **Silent RLS block on `admin_users`:** Even after fixing the race condition, Admin login still didn't redirect. Root cause: `admin_users` had RLS enabled with no read policy at all, so Supabase silently returned zero rows instead of an error — `resolveRole()` correctly ran, just never found the admin row. Fixed by adding a policy allowing a logged-in user to read *their own* `admin_users` row (`auth.uid() = auth_user_id`) — stricter than a blanket public policy, and correct for the long term (not just a Day 9 workaround).

## 🧪 Verified Results

- Admin login → dashboard redirect: working.
- Faculty login → own timetable displayed by default: working.
- Faculty "browse other faculty" dropdown: working, confirmed switching between Dr. Sharma and Dr. Verma's timetables.
- Protected routes correctly redirect unauthenticated direct URL access (tested in incognito).
- Logout correctly returns to the student/home view.
- Footer text confirmed visible on Admin, Faculty, and Student pages.
- No regressions in Day 3 (routing), Day 4 (engine), or Day 5 (persistence/views) functionality.

## 🚧 What's Ready to Build Tomorrow

- A fully authenticated app with role-based access control working correctly end-to-end.
- No AI agent yet — absences still require manual admin edits (not built yet).
- No leave-request flow yet for faculty.

## 🎯 Tomorrow's Objective (Day 7 per Blueprint)

Build the AI Reassignment Agent's natural-language parsing stage: a free-tier LLM call (per today's "free tools only" requirement — to be confirmed which free API we use, e.g. Gemini's free tier, when we get there) that turns a plain-English absence description into a structured, validated action, shown back to the Admin for confirmation before any substitute-finding logic runs.

## Notes on Free-Tools Requirement

Today's work used zero paid services — 100% Supabase Auth (free tier) and plain React/JS. No AI API was needed today; that begins tomorrow, and we'll explicitly choose a free-tier option (e.g. Google's Gemini API free tier) rather than a paid Anthropic key, consistent with your instruction.
