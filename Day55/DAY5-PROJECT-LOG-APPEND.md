
## Day 5 — Persistence, Derived Views & First Generate Button
- Added temporary public write/update/delete RLS policies on timetable_slots (dev-only, locked down Day 9).
- Hardened generator.js with swap-based backtracking; added markFree() to availability.js.
- Built timetableService.js: generateAndPersistTimetable(), getSectionTimetable(), getFacultyTimetable(), getAllSections(), getAllFaculty().
- Built reusable TimetableGrid.jsx component.
- Implemented real AdminDashboard.jsx (Generate button + section picker + live grid) and StudentView.jsx (dropdown + live grid), replacing Day 3 placeholders.
- Verified: 168 lectures/6 sections/0 unplaced persisted correctly; grid displays real subject/faculty/room data; no regressions in routing or engine logic.
- Deployment intentionally deferred to Day 10 per blueprint.
- Deliverable: DAY5-SUMMARY.md.
