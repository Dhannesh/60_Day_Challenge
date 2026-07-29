
## Day 8 — Faculty Leave-Marking + Admin Leave Requests
- Scope conflict flagged and resolved: stuck to blueprint (Leave-Request flow today, full QA pass deferred to Day 9) rather than today's prompt's QA/hardening template.
- Added RLS policies on leave_requests (faculty insert own, authenticated read, admin update).
- Built leaveService.js (create, bulk create, get pending, mark handled), MarkLeaveForm.jsx (upgraded to multi-select checkboxes per request), LeaveRequestsList.jsx.
- Extended AgentRequestBox.jsx to accept pre-filled leave-request context and auto-mark requests as handled on confirmation.
- Debugged: second+ leave request wouldn't trigger the agent (boolean ref only allowed one auto-run ever); fixed by tracking the specific leave request ID instead.
- Verified: multi-select leave marking, admin list, resolve-via-agent working for multiple sequential requests, status updates correctly. No regressions Days 3-7.
- Deliverable: DAY8-SUMMARY.md.
