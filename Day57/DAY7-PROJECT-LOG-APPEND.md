
## Day 7 — AI Reassignment Agent + UX Polish
- Provider decision: switched from Anthropic Claude (PRD default) to Groq (Llama 3.3 70B) for the agent's NLP parsing, after Gemini's free tier proved unavailable on our project (limit: 0, confirmed via ListModels the model names were valid - it was an account/project-level quota restriction). Groq confirmed working on first real test.
- Built agentService.js (parse + validate), substituteService.js (deterministic substitute-finding + confirm), AgentRequestBox.jsx (full agent UI).
- Wired agent into AdminDashboard.jsx with live grid refresh + highlight-on-change.
- UX polish pass: transposed TimetableGrid (days as rows, periods as columns per request), added real class times per period, focus states, loading spinners, empty-state styling, consistent Student header, highlight animation on reassigned cells.
- Verified end-to-end: real absence request correctly parsed, validated, substitute proposed and applied, grid updated live. No regressions Days 3-6.
- Deliverables: DAY7-SUMMARY.md, ARCHITECTURE-UPDATE-NOTE.md, API-UPDATE-NOTE.md (documenting the Groq switch).
