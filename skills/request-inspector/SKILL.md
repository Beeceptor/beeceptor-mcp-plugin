---
name: request-inspector
description: >-
  Inspect and analyze incoming request history on a Beeceptor endpoint. Use
  when debugging integration issues, verifying payloads, auditing traffic, or
  checking whether a client is calling the mock correctly.
---

# Request Inspector

Use this skill to fetch, read, and summarize real traffic captured by your
Beeceptor mock endpoint.

## When to use

- A client hit the mock but you are not sure what payload or headers were sent.
- You want to verify that a rule matched (or didn't match) an incoming request.
- You need to audit traffic for debugging, QA, or demo preparation.
- You are building a stateful flow and want to confirm request sequencing.

## Workflow

1. **Confirm endpoint** — call `endpoint_list` and verify the correct endpoint
   is selected for this session.
2. **Fetch history** — call `request_history_list` with a reasonable limit
   (default 20; increase if needed).
3. **Drill into requests** — for any suspicious or failed request, call
   `request_history_get` with the request ID to see full headers, body, matched
   rule, and response details.
4. **Summarize findings** — return a structured summary including:
   - Total requests in the window
   - Methods and paths called
   - Any requests that did not match a rule (fell through to default response)
   - Header or body anomalies relevant to the user's question
5. **Clean up if asked** — use `request_history_delete` (single) or
   `request_history_purge` (all) only when the user explicitly requests it.

## Output format

Return a concise analysis:
- What was called (method + path + timestamp)
- What rule matched (or "no rule matched")
- Key request headers / body fields relevant to the issue
- Recommended next steps (fix rule condition, add missing header, etc.)

Never delete history unless the user explicitly asks.
