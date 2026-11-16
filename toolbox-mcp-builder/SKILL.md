---
name: toolbox-mcp-builder
description: Plan and build branded MCP servers in ToolBox from an API spec (OpenAPI, Swagger, Postman, cURL) for our software or a client's, one server per client, with the right auth, a reviewed tool list, and gateway publishing.
version: 0.1.0
tags: [tools-and-products, toolbox, mcp, api, integrations, tool-reference]
---

## Purpose
Turn an API into an MCP server that an AI client can authorise and use: pick the endpoints worth exposing, import them, shape each tool, secure it, test it, and publish it under the right client's server, without leaking credentials or exposing write actions by accident.

## Use when
- A product API (SalesPortl, HiScore, PlusOne, or a client's platform) should be reachable from AI harnesses as an MCP server.
- A client needs a branded server with only the tools they are allowed to use.
- An existing ToolBox server needs new tools, tighter auth, or a review of its call logs.

## Required inputs
- The API's spec or docs (OpenAPI 3.x, Swagger 2.0, Postman v2.1 collection, or cURL examples) and its base URL.
- Who the server is for (internal, one client, a product) and which actions they may take.
- The outbound auth the API uses (API key, bearer, basic, OAuth 2.0) and where that secret is stored [name only].
- The inbound auth the AI client will use (OAuth 2.1 preferred for clients; API key or bearer for internal).

## Safety/authority
- Secrets are entered in ToolBox's credential settings only. Never paste a key into a skill, chat, spec file, or repo; record the secret's name and where it lives.
- Start read-only. Add write tools one at a time with explicit approval, and never expose bulk delete, billing, or send tools to a client server unless the user asks for that exact tool.
- Every imported tool is reviewed before it goes live; nothing is published unreviewed.
- One server per client, product, or environment. Do not mix two clients' tools in one server; access is per server, not per user.
- Prefer token passthrough when the upstream API should enforce the end user's own permissions.
- Client-facing material calls it ToolBox and our product names only.

## Workflow
1. Scope: list the jobs the AI client must do; map each to one endpoint. Each ToolBox tool is exactly one HTTP request with no custom code, so multi-step logic belongs in an API endpoint we build behind it.
2. Source: get the spec. For our products, use the product's API reference tool first (for SalesPortl, `get_api_reference`) and build or export a spec from it. For a Postman collection, folders become tool groups.
3. Create the server (one per client or product) and set outbound auth with the stored credential.
4. Import, then shape each tool: clear name and description, argument mapping (path, query, header, body), required fields, redacted fields, and response pruning so results stay small.
5. Group related tools into prompts where a repeated multi-tool job helps; add resources for static reference data.
6. Set inbound auth and per-client rate limits.
7. Test every tool in the built-in playground or inspector with harmless reads; check the call log and latency.
8. Publish: give the client the server URL, or add the server to the gateway when one URL for several servers is wanted (tools are then prefixed by server).
9. Record the server, its tools, auth type, and secret names in the internal tool ledger.

## Tool-call reference
ToolBox runs on our own host and exposes: per-server endpoints (`/mcp/<server-slug>`), one gateway endpoint (`/mcp`) for a chosen set of servers, an admin REST API (bearer key; servers, tools, resources, prompts, analytics), and a built-in management MCP server so an AI client can build and edit servers for us.
- Import sources: OpenAPI 3.0 and 3.1, Swagger 2.0, Postman v2.1 (with environments and variables), cURL, or the visual tool builder.
- Built-in connectors: database (MySQL, MariaDB, Postgres; read-only by default), mailbox (IMAP), FTP.
- Operations: replayable call log, latency per tool, per-client rate limits, retries with backoff, PII redaction, response pruning, versioned tool specs, server clone, export, import, and pause.
- Management server tool names: [confirm once the ToolBox management MCP is connected; do not guess them].

## Output format
```
- Server name, audience, and URL
- Tools (name -> method and path, read or write)
- Outbound auth (type, secret name) and inbound auth
- Test results per tool
- Gateway: yes / no
- Awaiting approval (write tools, publish)
```

## Quality checks
- No secret value appears anywhere outside ToolBox's credential store.
- Each tool maps to one endpoint, has a description an AI can route on, and was tested.
- Write tools were approved individually; the client server holds only that client's tools.
- The ledger records the server, auth, and secret names.

## Related skills
dgtldash-tools-router, dgtldash-capability-check, dgtldash-product-access-setup, salesportl-mcp-operator, hiscore-loyalty-operator, plusone-outreach-operator
