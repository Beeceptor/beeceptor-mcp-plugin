---
name: inspect-recent-requests
description: Fetch and summarize the last N requests received by the active Beeceptor endpoint to debug integration issues.
---

# Inspect Recent Requests

1. Call `endpoint_list` to confirm the active endpoint.
2. Call `request_history_list` with limit 20 (or the number the user specifies).
3. For each request, note: timestamp, method, path, matched rule (if any),
   and response status.
4. Flag any requests where no rule matched (fell through to default response).
5. If the user points to a specific request ID, call `request_history_get`
   to show full headers, body, and rule match details.
6. Return a concise summary table and actionable next steps
   (e.g. missing rule, wrong path pattern, header mismatch).
