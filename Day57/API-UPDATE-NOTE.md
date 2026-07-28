# API.md — Day 7 Update Note

**Change:** The `/agent/parse-request` endpoint description (originally specifying Anthropic Claude) now uses **Groq API**. Endpoint contracts (request/response shapes) are unchanged — only the underlying provider changed.

**Updated endpoint reference:**

### `parseAbsenceRequest(message)` — implemented in `src/services/agentService.js`
- **Purpose:** Send the admin's plain-English absence description to Groq (Llama 3.3 70B) and get back a structured action.
- **Request:** `message: string` (e.g. `"Dr. Sharma is absent Monday period 1"`)
- **Response:** `{ facultyNameRaw: string, dayIndex: number (0-4), period: number (1-8) }`
- **Provider:** Groq API, model `llama-3.3-70b-versatile`, called via `POST https://api.groq.com/openai/v1/chat/completions`
- **Auth:** `VITE_GROQ_API_KEY` (environment variable, free tier)
- **Error cases:** unparseable response (throws with rephrase suggestion), missing API key (throws immediately with setup instructions)

### `validateAbsenceAction(parsed)` — implemented in `src/services/agentService.js`
- **Purpose:** Confirms the parsed faculty name matches a real faculty member (fuzzy match) and that they have an actual scheduled slot at the given day/period.
- **Response:** `{ slotId, facultyId, facultyName, dayIndex, period, sectionLabel, subjectName }`
- **Error cases:** no matching faculty, multiple ambiguous matches, no slot found at that day/period

### `findSubstituteCandidates({ dayIndex, period, excludeFacultyId })` — implemented in `src/services/substituteService.js`
- **Purpose:** Deterministically (no AI) finds and ranks valid substitute candidates by lowest current weekly load, checking availability and daily max-periods limits.
- **Response:** `{ proposedFacultyId, proposedFacultyName, reason }` or `{ proposedFacultyId: null, reason: 'No available substitute found...' }`

### `confirmReassignment({ slotId, newFacultyId })` — implemented in `src/services/substituteService.js`
- **Purpose:** Applies the admin-confirmed substitute to `timetable_slots`.
- **Response:** `{ success: true }`

All four functions match the originally-designed API contract from Day 2's `API.md` in shape and behavior — only the LLM provider (Groq instead of Claude) and the fact these are called as direct client-side service functions (not separate REST routes, consistent with our Supabase-centric architecture) are implementation details worth noting for the record.
