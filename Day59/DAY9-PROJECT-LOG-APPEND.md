
## Day 9 — Release Readiness Review
- Scope confirmed: full QA/hardening today, deployment deferred to Day 10 as originally planned (today's prompt asked for both).
- Security: closed a real production risk by removing open public-write RLS policies on timetable_slots (Day 5 leftover); replaced with admin-only insert/update/delete, public read preserved.
- Reliability: added ErrorBoundary.jsx, NotFound.jsx + catch-all route, env var validation with clear error messaging in supabaseClient.js.
- Code quality: extracted useAsyncData.js hook, removing duplicated loading/error boilerplate across AdminDashboard, FacultyDashboard, StudentView.
- Accessibility: aria-live regions, aria-busy, aria-hidden on spinners, proper form labels (incl. sr-only).
- Branding: page title/meta/OG/Twitter tags, custom favicon, MIT LICENSE, README rewritten to match actual build (incl. Groq switch).
- Critical bug found and fixed: leave-request resolutions occasionally targeted the wrong slot because auto-generated text was re-parsed by the LLM instead of using already-known structured data. Fixed via new validateAbsenceActionDirect() bypassing the LLM for leave-request-triggered resolutions. Re-tested and confirmed fixed on the exact scenario that failed (3 sequential Dr. Sharma leave requests).
- Full regression passed across all features. No known bugs remain.
- Deliverable: DAY9-SUMMARY.md.
