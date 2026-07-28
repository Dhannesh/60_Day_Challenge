# ARCHITECTURE.md — Day 7 Update Note

**Change:** All references to "Anthropic Claude API" for the AI Reassignment Agent are superseded by **Groq API (Llama 3.3 70B Versatile model)**, decided on Day 7 due to Google Gemini's free-tier quota being unavailable on the tested project, and per your explicit preference for a genuinely free-tier provider over a paid one.

**Where this affects the original Day 2 ARCHITECTURE.md:**

- **Component Diagram (Section 2):** the "External Services" box labeled "Anthropic Claude API" should now read "Groq API (Llama 3.3)".
- **Section 4 (AI Reassignment Agent data flow):** every mention of "Claude API" in the sequence diagram becomes "Groq API". The flow itself (parse → validate → find substitute → confirm → update) is unchanged — only the LLM provider changed, not the pipeline design.
- **Section 6 (External Services table):** replace the Anthropic Claude API row with:

| Service | Purpose | Auth method | Notes |
|---|---|---|---|
| Groq API | Natural-language parsing for the reassignment agent (Llama 3.3 70B Versatile model) | API key via environment variable (`VITE_GROQ_API_KEY`) | Free tier, no cost risk. Called via plain `fetch` to `https://api.groq.com/openai/v1/chat/completions` (OpenAI-compatible format) |

**No other architectural decisions changed.** The safety design principle — the LLM only parses/phrases, and all matching/writing logic stays in deterministic JS (`substituteService.js`) — is fully preserved and was validated working correctly today.
