# Beeceptor — Cursor Plugin

Mock, intercept, and test HTTP APIs without leaving Cursor.

The Beeceptor plugin connects Cursor to your [Beeceptor](https://beeceptor.com)
account via MCP, giving AI agents direct access to create mock rules, inspect
traffic, run chaos tests, and manage stateful API simulations.

---

## What's included

| Component | File | Purpose |
|-----------|------|---------|
| MCP server | `mcp.json` | Connects Cursor to the Beeceptor MCP API |
| Rule | `rules/beeceptor-usage.mdc` | Best-practice guidance applied when working with Beeceptor tools |
| Skill | `skills/mock-api-designer/` | Design and scaffold mock APIs from a description or spec |
| Skill | `skills/request-inspector/` | Inspect and debug captured request history |
| Agent | `agents/api-mock-setup.md` | End-to-end agent that sets up a complete mock from scratch |
| Agent | `agents/chaos-tester.md` | Configures weighted error rates and latency injection for resilience testing |
| Command | `commands/create-crud-mock.md` | One-shot CRUD resource scaffolding (`/api/users`, etc.) |
| Command | `commands/inspect-recent-requests.md` | Fetch and summarize the latest requests on an endpoint |

---

## Setup

### 1. Get your Beeceptor API key

Log in at [beeceptor.com](https://beeceptor.com), go to **Account → API Keys**,
and copy your key.

### 2. Set the environment variable

Add the following to your shell profile (`.zshrc`, `.bashrc`, etc.) or to your
Cursor environment:

```bash
export BEECEPTOR_API_KEY=your_api_key_here
```

### 3. Install the plugin

Install this plugin from the
[Cursor Marketplace](https://cursor.com/marketplace) or add the MCP config
manually to your Cursor settings:

```json
{
  "mcpServers": {
    "beeceptor": {
      "url": "https://mcp.beeceptor.com/mcp",
      "headers": {
        "Authorization": "Bearer YOUR_API_KEY"
      }
    }
  }
}
```

> **Sandbox users:** replace the URL with `https://mcp.beeceptor.tech/mcp`.

---

## Available MCP tools

Once connected, Cursor's AI has access to the full Beeceptor toolset:

**Endpoints**
- `endpoint_list` — list all endpoints and current selection
- `endpoint_select` — switch active endpoint for this session
- `endpoint_create_free` — provision a new free endpoint
- `endpoint_info` — get mock base URL for the active endpoint

**Mock Rules**
- `rule_list`, `rule_get`, `rule_create`, `rule_update`, `rule_delete`
- `rule_delete_all`, `rule_bulk_replace`, `rule_reorder`
- `rule_create_weighted` — multi-outcome weighted responses
- `rule_create_callout` — proxy/webhook forwarding rules
- `rule_create_crud` — full CRUD resource on a path

**Request History**
- `request_history_list`, `request_history_get`
- `request_history_delete`, `request_history_purge`

**State Store**
- `state_list`, `state_get`, `state_upsert`, `state_delete`, `state_delete_item`

**Settings & Utilities**
- `settings_get`, `settings_update`
- `syntax_info` — Handlebars + Faker reference

---

## Usage examples

### Create a mock API from a description

> "Set up a mock for a User Service with GET /users, POST /users, and
> GET /users/:id returning realistic faker data."

The **api-mock-setup** agent (or the **mock-api-designer** skill) will:
1. Provision or select an endpoint
2. Create rules with dynamic faker responses
3. Return the live mock URL and curl examples

### Inspect what a client is sending

> "Show me the last 10 requests that hit my endpoint and flag any that didn't
> match a rule."

Use the `inspect-recent-requests` command or the **request-inspector** skill.

### Simulate failures for resilience testing

> "Add a 20% chance of 503 and a 5% chance of a 10-second timeout on
> POST /checkout."

The **chaos-tester** agent will create weighted rules and latency injection
targeting the specified routes.

---

## Links

- [Beeceptor documentation](https://beeceptor.com/docs/)
- [Beeceptor MCP server on GitHub](https://github.com/beeceptor/beeceptor-mcp)
- [Cursor Marketplace](https://cursor.com/marketplace)
