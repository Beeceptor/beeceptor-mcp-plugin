---
name: create-crud-mock
description: Scaffold a full CRUD mock for a REST resource on Beeceptor (GET list, GET by ID, POST, PUT, DELETE).
---

# Create CRUD Mock

1. Ask for the resource path (e.g. `/api/users`) and the ID field name
   (e.g. `id`, `userId`, `_id`).
2. Call `endpoint_list` to confirm the active endpoint and retrieve its name.
3. Call `rule_create_crud` with:
   - `conditions`: `[{ "type": "path", "operator": "starts_with", "value": "<resourcePath>" }]`
   - `idField`: the provided ID field name
   - `method`: `"*"` (all methods)
4. Call `endpoint_info` to get the mock base URL.
5. Return the mock base URL and a ready-to-use curl example for each operation:
   - `GET  <mockBaseUrl><resourcePath>` — list all
   - `GET  <mockBaseUrl><resourcePath>/<id>` — get one
   - `POST <mockBaseUrl><resourcePath>` — create
   - `PUT  <mockBaseUrl><resourcePath>/<id>` — update
   - `DELETE <mockBaseUrl><resourcePath>/<id>` — delete
