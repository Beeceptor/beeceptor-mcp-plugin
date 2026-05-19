---
name: mock-api-designer
description: >-
  Design and build a complete mock API on Beeceptor. Use when you want to
  scaffold REST endpoints, create dynamic JSON responses, set up CRUD resources,
  or configure weighted/chaos responses from a description or list of routes.
---

# Mock API Designer

Use this skill to turn a description or table of routes into a live
Beeceptor mock — ready to hit immediately.

## When to use

- You have a list of routes or an API description and want a running mock
  before the backend is built.
- You need to add or replace rules on an existing endpoint.
- You want realistic dynamic data (faker names, UUIDs, timestamps) instead of
  static strings.

## Workflow

1. **Select endpoint** — call `endpoint_list` to see available endpoints and
   confirm which one to use. If none exist, call `endpoint_create_free`.
2. **Inspect existing rules** — call `rule_list` so you know what is already
   there and avoid conflicts.
3. **Check syntax** — if you need Handlebars or Faker references, call
   `syntax_info` first; do not guess helper names.
4. **Create rules** in this order of preference:
   - `rule_create_crud` for any resource path that needs full CRUD
     (`GET /users`, `POST /users`, `DELETE /users/:id`, etc.)
   - `rule_create_weighted` when the user needs chaos/error-rate simulation
   - `rule_create_callout` when the user needs to proxy or forward requests
   - `rule_create` for all other cases (single response per path/method)
5. **Verify** — call `rule_list` again and return `endpoint_info` (with
   `mockBaseUrl`) so the user can hit the mock immediately.

## Rules for mock responses

- Set `templated: true` (the default) and use `{{faker 'namespace.attribute'}}`
  in the body for realistic data. Examples:
  - `{{faker 'person.fullName'}}`, `{{faker 'internet.email'}}`
  - `{{faker 'string.uuid'}}`, `{{faker 'number.int' (object max=1000)}}`
  - `{{faker 'date.recent' (object days=30)}}`
- Mirror request fields back with `{{body 'fieldName'}}`.
- Keep response bodies pretty-printed (2-space indented JSON).
- Response headers must be static strings — no templating.
- Set `Content-Type: application/json; charset=utf-8` on JSON responses.
- Use `delay` (ms) to simulate realistic latency or timeouts.

## Output

After completing setup, reply with:
- The mock base URL from `endpoint_info`
- A summary table of routes created (method, path, rule type, status)
- Any caveats (rule priority, missing auth headers, etc.)
