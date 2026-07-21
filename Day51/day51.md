# Smart Timetable Auto-Generator with AI Reassignment Agent

A rule-based timetable auto-generation engine for a college department, paired with an AI agent that resolves faculty-absence disruptions through natural-language requests.

Built as a 10-day capstone project for the **AB Talks 60-Day Claude AI Mastery Challenge**.

---

## 🎯 Problem

Creating a department timetable manually — assigning faculty, subjects, and rooms across many sections while avoiding clashes — is slow and error-prone. Once published, handling real-world disruptions (like a faculty member's sudden absence) requires manually checking every colleague's schedule and workload to find a valid substitute.

## ✅ Solution

- **One-click auto-generation** — instantly produces a clash-free weekly timetable for all sections at once, using a deterministic, rule-based engine (no ML uncertainty in the core schedule).
- **AI Reassignment Agent** — type a plain-English request like _"Ms. Sharma is absent Monday period 3"_, and the agent checks faculty load and clashes, proposes a valid substitute, and updates the timetable once you confirm.
- **Human-in-the-loop by design** — the AI never applies a change automatically; every suggestion requires admin confirmation.

---

## 👥 Roles & Access

| Role                    | Access                             | What they can do                                                              |
| ----------------------- | ---------------------------------- | ----------------------------------------------------------------------------- |
| **Admin / Coordinator** | Full secure login                  | Generate the timetable, run the AI agent, manage leave requests               |
| **Faculty**             | Login required                     | View own timetable by default, browse other faculty, mark themselves on leave |
| **Student**             | No login (Year + Section dropdown) | View their section's timetable                                                |

---

## 📦 v1.0 Scope

- 1 department, **2nd Year**, **6 sections**
- **20 faculty members**, **8 subjects per section**
- Monday–Friday, **8 periods/day**

**Hard scheduling rules enforced:**

- No faculty double-booked across any section
- Each subject's fixed weekly lecture count is met exactly
- No subject repeated on the same day for a section
- Subjects requiring a specific lab/room only scheduled in that lab/room
- No faculty exceeds their configured max periods/day

**Explicitly out of scope for v1.0:**

- Mobile app (web-only)
- Multi-department / multi-year support
- Peer-to-peer leave negotiation between faculty
- Email/SMS notifications
- Auto-applying AI suggestions without admin confirmation

See the full **Product Requirements Document (PRD)** for complete details.

---

## 🛠 Tech Stack

- **Frontend:** React (JS, via Vite) + Tailwind CSS
- **Backend & Data:** Supabase (Postgres + Auth)
- **AI Agent Layer:** Anthropic Claude API (`claude-sonnet-4-6`) — parses natural-language requests into structured actions; substitute-finding logic itself stays rule-based for reliability

---

## 🗂 Project Structure (high-level)

```
/src
  /engine        → rule-based generation engine, availability matrices, validation
  /services      → Supabase data access, agent service, substitute-finding
  /components    → TimetableGrid, AgentRequestBox, LeaveRequestsList, etc.
  /pages         → AdminDashboard, FacultyDashboard, StudentView, login pages
  /auth          → Supabase Auth context
/supabase
  schema.sql     → full database schema
  seed.sql       → seed data (20 faculty, 8 subjects, 6 sections)
```

---

## 🚀 Getting Started (local development)

1. Clone the repository and install dependencies:
   ```
   npm install
   ```
2. Create a Supabase project and run `supabase/schema.sql`, then `supabase/seed.sql`.
3. Create a `.env.local` file with your Supabase credentials:
   ```
   VITE_SUPABASE_URL=your-project-url
   VITE_SUPABASE_ANON_KEY=your-anon-key
   ```
4. Run the dev server:
   ```
   npm run dev
   ```

---

## 🧪 Core Flows to Try

1. **Generate:** Log in as Admin → click "Generate Timetable" → see all 6 sections populate instantly.
2. **Reassign:** Type an absence request (e.g. _"Mr. Verma is absent Tuesday period 5"_) → confirm the proposed substitute → see the timetable update live.
3. **Leave:** Log in as Faculty → mark yourself on leave for a slot → check it appears on the Admin's Leave Requests list.
4. **Student view:** Open the app with no login → select Year 2 + a section → view the timetable.

---

## 🔭 Future Scope

- Multi-department, multi-year rollout
- Peer-to-peer leave negotiation workflow (faculty-to-faculty requests with accept/reject)
- Email/SMS notifications
- Native mobile app
- Analytics dashboard (workload trends, room utilization, substitution frequency)
- Voice-driven agent interaction

---

## 👤 Author

**Dhaneshwar Kumar** — ABTalks (@aabtalks)
Built as part of the 60-Day Claude AI Mastery Challenge.

## Prompt

```
Product Discovery & Sprint Planning

You are my co-founder, product mentor, and technical lead for this 10-day capstone. Your goal is to help me go from no idea to a deployed v1.0 product. Help me discover the right problem, shape the best solution, and guide me through the entire journey over the next 10 days (including today).

I'm participating in the AB Talks 60-Day Claude AI Challenge. This capstone follows a real software development lifecycle:

Requirements → Design → Setup → Implementation → Testing → Deployment → Maintenance

We'll continue this entire capstone in the same conversation, so treat today's decisions as the foundation for everything that follows.

Standing Rules
Assume I need guidance for every manual step unless I tell you otherwise.
Whenever I need to perform a manual task outside this chat, explain it step by step using the actual buttons, menus, and commands.
Wait for my confirmation and a screenshot before continuing.
Never assume I've completed a step.
Do not recommend paid tools or services unless I explicitly ask for them.

Today's Goal

Interview me one question at a time.

Keep every question simple, and briefly explain why you're asking it.

If I don't already have a project idea, interview me to discover one. Understand my interests, goals, skills, strengths, and constraints, then suggest, compare, refine, combine, and challenge ideas until we've chosen the strongest project I can realistically build in 10 days.

Don't optimize for the most ambitious project. Optimize for the most impressive project that can be fully completed within the available time. Continuously protect me from scope creep.

Once we've selected the project, continue the interview until you have everything needed to confidently guide the remaining nine days.

Clearly define:

What the v1.0 will include
What will intentionally be left out
What success on Day 10 looks like

Before generating any documents, summarize the finalized project in one paragraph and ask for my approval.

Only generate the deliverables after I confirm.

Deliverables

Generate downloadable versions of:

1. Product Requirements Document (PRD)

A complete, professional PRD for the finalized project.

2. Implementation Blueprint (Days 2-10)

Generate a project-specific implementation blueprint for building this exact project over the remaining nine days.

This must not be a generic template.

Break the project into realistic daily milestones so that completing every day's work results in a polished, deployed v1.0 by Day 10.

For each day, include:

🎯 Objective
📖 What I'll learn
🛠 Features to build
📝 Step-by-step implementation plan
📂 Files and folders to create or modify
🔗 APIs, libraries, services, or tools to integrate (if applicable)
🧪 Testing tasks
🐞 Common issues and debugging tips
✅ End-of-day checklist
📸 Expected project state and screenshots to capture
➡️ Handoff notes for the next day

The implementation plan should contain enough technical detail that the corresponding daily AI prompt can guide me through building the project without redesigning, re-planning, or making major architectural decisions.

Assume each remaining day begins with a fresh AI conversation. Therefore, each day's section must contain enough context that another AI assistant could immediately continue building from where the previous day ended.

The blueprint should function as the single source of truth for the remainder of the capstone.

3. Project Pitch Deck

Create a presentation-ready pitch deck covering:

Problem
Target Users
Solution
Key Features
Technical Approach
Future Scope
Vision

Important

Do not choose the tech stack or write code today.

Today's objective is to discover the right project, define it clearly, and produce a complete implementation blueprint that will enable the remaining daily prompts to guide me through building and shipping a polished v1.0 product by Day 10.
```
