# Project Structure — Smart Timetable Auto-Generator

Status: Updated Day 3 to reflect the actual scaffolded project (previously a Day 2 design document; now confirmed built).

## Full Structure (as built)

```
smart-timetable-automator/
├── docs/
│   ├── PRD_Timetable_Automator.docx
│   ├── Implementation_Blueprint_Days2-10.docx
│   ├── Implementation_Blueprint_Day2_Addendum.docx
│   ├── Pitch_Deck_Timetable_Automator.pptx
│   ├── ARCHITECTURE.md
│   ├── SCHEMA.md
│   ├── API.md
│   ├── UI-WIREFRAMES.md
│   ├── PROJECT-STRUCTURE.md
│   ├── PROJECT-LOG.md
│   ├── SETUP.md                      ← new, Day 3
│   ├── ENVIRONMENT.md                ← new, Day 3
│   └── DAY3-SUMMARY.md               ← new, Day 3
│
├── supabase/
│   └── schema.sql                    ← created and run in Supabase, Day 3
│       (seed.sql — scheduled for Day 4)
│
├── src/
│   ├── engine/
│   │   ├── generator.js              (empty — Day 3 build)
│   │   ├── availability.js           (empty — Day 3 build)
│   │   ├── validate.js               (empty — Day 3 build)
│   │   ├── constants.js              (empty — Day 3 build)
│   │   └── testGenerator.js          (empty — Day 3 build)
│   │
│   ├── services/
│   │   ├── supabaseClient.js         ✅ implemented Day 3
│   │   ├── timetableService.js       (empty — Day 4 build)
│   │   ├── agentService.js           (empty — Day 6 build)
│   │   ├── substituteService.js      (empty — Day 7 build)
│   │   └── leaveService.js           (empty — Day 8 build)
│   │
│   ├── components/
│   │   ├── TimetableGrid.jsx         (empty — Day 5 build)
│   │   ├── AgentRequestBox.jsx       (empty — Day 6 build)
│   │   ├── LeaveRequestsList.jsx     (empty — Day 8 build)
│   │   ├── MarkLeaveForm.jsx         (empty — Day 8 build)
│   │   ├── RecentChangesPanel.jsx    (empty — Day 7 build)
│   │   └── Navbar.jsx                (empty — Day 8 build)
│   │
│   ├── pages/
│   │   ├── AdminLogin.jsx            ✅ placeholder, Day 3
│   │   ├── AdminDashboard.jsx        ✅ placeholder, Day 3
│   │   ├── FacultyLogin.jsx          ✅ placeholder, Day 3
│   │   ├── FacultyDashboard.jsx      ✅ placeholder, Day 3
│   │   └── StudentView.jsx           ✅ placeholder, Day 3
│   │
│   ├── auth/
│   │   └── AuthContext.jsx           (empty — Day 5 build)
│   │
│   ├── router.jsx                    ✅ implemented Day 3
│   ├── App.jsx                       ✅ "Hello World" test, retained but not wired to entry point
│   ├── main.jsx                      ✅ updated Day 3 to use RouterProvider
│   └── index.css                     ✅ Tailwind import, Day 3
│
├── .env.local                        ✅ created Day 3 (gitignored)
├── .gitignore                        ✅ confirmed covers .env*
├── vite.config.js                    ✅ configured with React + Tailwind plugins, Day 3
├── package.json
└── README.md
```

## What Changed From the Day 2 Design

One small clarification, no structural change: `App.jsx` is **retained in the project** (it was useful as a quick Tailwind/Supabase connectivity test today) but is **no longer the app's entry point** — `main.jsx` now renders `<RouterProvider>` directly, and each page in `/src/pages` is reached via its own route. This wasn't explicitly decided in Day 2's design and is noted here for clarity; it doesn't require any changes to `ARCHITECTURE.md` or `API.md`.

Every other file and folder matches the Day 2 design exactly — no redesign was needed.
