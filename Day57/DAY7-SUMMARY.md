# Day 7 Summary — AI Reassignment Agent + UX Polish

## ✅ What Was Completed Today

- **AI provider decision:** switched from the PRD's original Anthropic Claude API to **Groq (Llama 3.3 70B)** for the agent's natural-language parsing — genuinely free tier, no cost risk, per your explicit choice.
- **`agentService.js`** — parses plain-English absence requests into structured actions via Groq, then validates against real Supabase data (fuzzy faculty-name matching, confirms the faculty actually has a slot at that day/period).
- **`substituteService.js`** — fully deterministic (non-AI) substitute-finding logic: checks faculty availability at the exact slot, daily load limits, ranks candidates by lowest current weekly load. Also handles applying the confirmed reassignment to the database.
- **`AgentRequestBox.jsx`** — the full agent UI: text input → parsed confirmation card → substitute proposal → confirm/cancel → live update, with proper loading/error/success states.
- **Wired into `AdminDashboard.jsx`** — the agent box sits below the live timetable grid; a successful reassignment triggers a grid refresh and a temporary green highlight on the changed cell.
- **UX/Product polish pass** (post-implementation, as requested):
  - Transposed `TimetableGrid` — days as rows, periods as columns (per your request).
  - Added real class times under each period header (8:50-9:40 through 3:40-4:30).
  - Added visible focus rings on all inputs/buttons for keyboard accessibility.
  - Added subtle loading spinners (Generate button, agent "Thinking" state, grid loading state) instead of plain text.
  - Added a "recently changed" green highlight animation on the timetable cell after a successful reassignment.
  - Improved empty-state styling (dashed border + icon instead of plain text).
  - Made Student view's header visually consistent with Admin/Faculty pages.
  - Added a shared `max-w-6xl mx-auto` content wrapper on Admin/Student pages for better readability on wide screens.

## 🐞 Issues Debugged Today

1. **Gemini free-tier quota returning `limit: 0`:** tried the originally-planned Gemini API, but the specific Google Cloud project behind the API key had no free-tier allocation for any tested model (2.0-flash, 1.5-flash — the latter also turned out to be fully retired, a separate 404 issue). After ruling out a model-naming problem via `ListModels`, concluded this was an account/project-level quota restriction outside our control.
2. **Decision to switch providers:** rather than keep fighting an account-level restriction, switched to Groq — confirmed working correctly on the first real test.

## 🧪 Verified Results

- Real end-to-end test: "Dr. Sharma is absent Monday period 1" → correctly parsed → correctly matched to her actual Data Structures Lab slot in Section A → correctly proposed Prof. Kulkarni as substitute (lowest current load, available at that time) → admin confirmed → grid updated live, showing Prof. Kulkarni in that slot.
- No regressions in any previously built feature (routing, engine, persistence, auth).

## 🚧 What's Ready to Build Tomorrow

- A feature-complete core product: generate, view (Admin/Faculty/Student), authenticate, and AI-assisted reassignment, all working together with a polished UI.
- No leave-request flow yet — faculty can't yet mark themselves on leave; that's tomorrow's work.

## 🎯 Tomorrow's Objective (Day 8 per Blueprint)

Build the faculty-initiated Leave Marking flow (mark self on leave for a day/period) and the Admin's Leave Requests list (pending items, "Resolve via Agent" shortcut that pre-fills today's AgentRequestBox with the relevant context).

## Documentation Updates Required

`ARCHITECTURE.md` and `API.md` (from Day 2) need updating to reflect the Groq/Llama switch instead of Anthropic Claude — noted for the next documentation pass. No structural changes needed, just the provider name/endpoint details.

## Free-Tools Compliance

Today's work used 100% free-tier services: Groq API (free), Supabase (free tier, unchanged). No paid API was used at any point, consistent with your requirement.
