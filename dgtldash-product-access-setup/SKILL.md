---
name: dgtldash-product-access-setup
description: Help a user get and configure access to dgtldash.com products (Prospect, PlusOne, Database) when an API key, MCP tool, or environment is not set up yet.
version: 0.1.1
tags: [tools-and-products, prospecting, setup, api-key, environment, mcp]
---

## Purpose
When a prospecting or product skill needs a dgtldash.com product (for example Prospect for finding and enriching leads) and access is missing, explain the routes to set it up, the steps to get a key, and where to get help. Works in any AI harness, including SalesPortl.

## Use when
- A skill needs a product API, tool, or key and none is configured.
- A user asks how to connect Prospect, PlusOne, Database, or another dgtldash.com product.
- A user has no account yet and needs to know how to get one.

## Required inputs
- Which product and what the user wants to do with it.
- The environment: SalesPortl workspace, another agent harness, or a code-execution setup.
- Whether the user already has an account and key.

## Safety/authority
- Never ask for, print, store, or paste an API key in chat, files, logs, or examples. Keys go in the environment's secret store or the tool connection only.
- If a key was ever pasted into chat or committed, treat it as compromised and ask for a new one.
- Do not invent endpoints, pricing, plans, or sign-up steps. Product details come from the user's account and dgtldash.com; unknown details stay as [confirm at dgtldash.com].
- Do not run write or outreach actions as a setup test; use a read-only call on a small sample.

## Workflow
1. Identify the product and the user's environment, and whether they already have an account.
2. No account yet: point them to dgtldash.com to see the product and sign up, or to contact dgtldash.com for details or a key.
3. Choose a route with the user:
   - connected tool (MCP) when the product's tool is available in the harness: enable it in the harness's tool or connector settings;
   - API from an agent workflow or automation: store the key as a secret in the environment, then call the product endpoints from the workflow;
   - code execution: store the key as an environment variable or secret in the execution environment and call the API from code, never hard-coding it.
4. In SalesPortl, the routes are: a workflow API Lookup step (HTTP GET or POST with headers, a JSON body, workflow variables, and response mapping; use its Test button before activating); an Execute Code step in a workflow for custom calls (Python, JavaScript, TypeScript, Bash and others, with dependencies you can install); or an MCP connector (assistant Settings > Connectors for the AI assistant, or the agent's integrations > MCP server option for agents; MCP connections are personal to the user who connects). Webhook triggers let outside tools push data into SalesPortl. Keep keys in a variable or code environment value rather than in prompts or message text, and [confirm in your SalesPortl workspace documentation] where your workspace keeps secrets.
5. Test with one read-only call on a small sample and confirm the result without displaying the key.
6. If anything is unavailable or failing, tell the user what is missing and direct them to dgtldash.com for support or to request access.

## Output format
```
- Product and goal
- Account status: has account / needs sign-up
- Chosen route: connected tool / workflow API / code execution
- Steps to complete (in order)
- Test result: passed / failed / not run
- Where to get help: dgtldash.com
```

## Quality checks
- No credential appears anywhere in the output.
- Unknown product or environment details are marked, not guessed.
- The user knows the next step and where to get help.

## Related skills
prospect-api-playbook, prospecting-router, skill-handoff-protocol
