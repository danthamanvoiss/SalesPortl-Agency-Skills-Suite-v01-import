---
name: dgtldash-tools-router
description: Entry point for any task done inside our own software (SalesPortl, HiScore, PlusOne, ToolBox) through its connected tools; picks the product skill, runs the preflight, and keeps reads before writes.
version: 0.1.0
tags: [core, routing, tools-and-products, dgtldash, salesportl, hiscore, plusone, toolbox]
---

## Purpose
Route work that has to happen inside one of our products to the one product skill that knows its tools. Each product skill carries a tool-call reference, so the agent calls real tool names instead of guessing, and reaches for the product's own context tool before it builds anything.

## Use when
- The user asks to do something in SalesPortl, HiScore, PlusOne, or ToolBox ("build a workflow", "set up a loyalty card", "launch a LinkedIn campaign", "turn this API into an MCP server").
- A workflow skill (sales, marketing, prospecting, client success) reaches the step where the work must be done in a product.
- Work spans two products, such as a lead synced from PlusOne to SalesPortl, or a HiScore loyalty member followed up from SalesPortl.

## Required inputs
- The outcome wanted and which product it lives in [if unclear, ask one question].
- Which account the work is for: our own, or a client's account or sub-account.
- Authority level: read-only, draft, or approved to change, send, publish, or spend.

## Safety/authority
- Run `dgtldash-capability-check` first when the product's tools have not been used in this session.
- Read before write: identify the account (`whoami`), read the area's context tool, then change things.
- Anything that reaches real people or money waits for explicit approval of the exact action: sending SMS, email, push or LinkedIn messages, launching campaigns, publishing sites or games, buying numbers, payouts, deletes.
- A product connection acts in one account only. Never assume a change lands in a client's sub-account; confirm the account first.
- Do not invent tool names, fields, or limits. When the reference does not cover something, use the product's own reference or docs tool and mark the gap.

## Workflow
1. Name the product and the account. If two products are involved, name the system of record for each field (SalesPortl holds the lead record; PlusOne holds the cold sequence; HiScore holds the loyalty card).
2. Pick the product skill:
   - CRM, pipelines, messaging, workflows, AI agents, sites, funnels, forms, bookings, ads, communities, SaaS plans, sub-accounts: `salesportl-mcp-operator`
   - Loyalty and membership cards, stamps and points, pushes, promotions, games, loyalty workflows: `hiscore-loyalty-operator`
   - LinkedIn and email outreach campaigns, sequences, unified inbox, reply agents: `plusone-outreach-operator`
   - Turning an API into an MCP server, branded servers per client, the MCP gateway: `toolbox-mcp-builder`
3. For lead sourcing and prospect lists, send the task to `prospecting-router` when it is installed (SalesPortl Agency Skills Suite); it calls these skills when product work starts.
4. Hand off with the account, the authority level, and the outcome. One product skill at a time unless the task is a sync.

## Output format
```
- Product and account
- Skill chosen and why
- Authority level confirmed
- Preflight result
- Next step
```

## Quality checks
- The account was confirmed before any change.
- No tool name was used that is not in the product skill's reference or the product's own tool list.
- Every action that reaches people or money has a recorded approval.

## Related skills
dgtldash-capability-check, dgtldash-product-access-setup, salesportl-mcp-operator, hiscore-loyalty-operator, plusone-outreach-operator, toolbox-mcp-builder, prospecting-router, agency-router
