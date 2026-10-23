---
name: dgtldash-capability-check
description: Preflight for any DGTLDash product skill: confirms the agent has a browser or API tool, the user's credentials are set, and the user subscribes to the product, before any work starts.
version: 0.1.0
tags: [tools-and-products, dgtldash, preflight, setup]
---

## Purpose
Stop product skills from failing halfway or guessing. Run this first and tell the user plainly what is missing.

## Use when
- A skill for a DGTLDash product (Prospect, PlusOne, SalesPortl and similar) is about to run.
- A product workflow failed at login, at a blocked host, or with a missing-credential error.

## Required inputs
- Which product and which path the skill needs: browser-use (a browser tool signs in to the product's site) or API (HTTP calls with a key).

## Safety/authority
- Check that environment variables exist, by name only. Never print, log, or paste a credential or key.
- Do not open billing, plan, subscription, or account-settings pages to answer the subscription question. Judge it from whether sign-in and the product's own tools work, and otherwise ask the user.
- If the user has no subscription, point them to dgtldash.com and stop; do not work around it.

## Workflow
1. Tool: for browser-use, confirm a browser tool is installed and can reach the product host. For API, confirm HTTP calls to the API host are allowed. If a host is blocked, name the host so the user can allow it.
2. Credentials: confirm the variables the product skill names are set (for example a username and password for browser-use, or an API key for API use). If missing, tell the user which names to add and that a new session may be needed.
3. Subscription: attempt sign-in or one harmless read call. If it is refused or the product is not available to the account, ask whether the user subscribes and share dgtldash.com.
4. Report a pass or a short list of fixes, then hand back to the product skill.

## Output format
```
- Tool: ok / missing (what to install or allow)
- Credentials: set / missing (names only)
- Subscription: confirmed / unknown / not subscribed
- Next step
```

## Quality checks
- No secret values appear anywhere in the output.
- Each failure has one concrete fix for the user.

## Related skills
dgtldash-product-access-setup, prospect-browser-use-company-export, prospect-api-playbook
