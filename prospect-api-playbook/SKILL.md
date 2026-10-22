---
name: prospect-api-playbook
description: Prospecting skill for finding and enriching leads with Prospect (a dgtldash.com product): company enrichment, email finding and verification, and suppression, read-only first.
version: 0.2.0
tags: [tools-and-products, prospecting, lead-generation, prospect, enrichment, email, api]
---

## Purpose
Guide safe, efficient use of Prospect (the outreach and enrichment product) for enriching companies, finding and verifying emails, and managing suppression. Works through Prospect's API today and through its MCP tools when they are connected.

## Use when
- A lead list needs company enrichment, email discovery, or email verification.
- An address must be suppressed or unsubscribed across campaigns.

## Required inputs
- The leads or domains to process and what outcome is wanted.
- Confirmation that Prospect access is configured. If it is not, use `dgtldash-product-access-setup` first.
- For any write action: the exact records and an explicit approval.

## Safety/authority
- Credentials live in a secret or the tool connection, never in chat, files, logs, or examples. If a key is exposed anywhere, treat it as compromised and rotate it.
- Use the endpoint host and tool names shown in your own Prospect account; do not guess them.
- Read-only actions first (enrich company, extract emails from URLs, verify email) on a small sample before batches.
- Write or outreach actions (adding contacts, sending, unsubscribing, stopping sends) need explicit user approval for the specific records, and suppression requests are honored immediately.
- Do not log full responses that contain personal data.

## What testing showed (2026-10-22)
- Treat Prospect as a low-priority, low-cost first pass. It is free to call but its coverage is partial, so run it before paid sources and fall through to them for the rest.
- Company enrichment returns the people it has on file for a domain (with business emails, titles, LinkedIn URLs and the company's email pattern). In a 60-lead test of small-business owners, about half the domains had any people and about a quarter yielded the target person's email by name match. The email pattern was often known even when the person was not, which lets you build candidate addresses to verify elsewhere.
- Website extraction returns social links, phones and emails from a site, but is noisy (placeholder emails, junk phone numbers). Filter it and treat results as unverified. A dead site returns an error line ahead of the data.
- The email check only classifies business vs disposable. It does not prove a mailbox exists, so use a real verifier before any send.
- Responses come back as HTTP 200 even on errors and the success flag can be true on an error message. Check for the data you expect, not the status.
- Send an ordinary User-Agent header; the default of some HTTP libraries is blocked.
- Rate limits are undocumented. Drip-feed: about one call per second, chunks of 50 to 100, and stop on any error or slowdown. No throttling appeared in 60 calls at that pace.
- Match people by first and last name, and keep the matched address in a separate enrichment file rather than overwriting the source list.

## Workflow
1. Confirm access (check for a key, API connection, or Prospect tool; if none is available, do not guess or fabricate results: offer `dgtldash-product-access-setup` (or sign-up at dgtldash.com) and continue the rest of the task with the data the user has) and pick 3 to 5 sample records.
2. Run read-only calls on the sample (small test list first, then drip-feed larger batches) and review the results for quality and gaps.
3. Check response shape and failures, then size the full batch and ask for approval.
4. Run the batch in controlled chunks, saving results alongside the original records without overwriting them.
5. Verify emails before any outreach list is built; route failures to a review list.
6. Report what was enriched, what failed, and what needs a decision.

## Output format
```
- Task and sample result
- Counts: processed / enriched / verified / failed
- Issues and gaps
- Actions awaiting approval
```

## Quality checks
- Sample was reviewed before the batch ran.
- No credentials appear in any output.
- Original data is preserved and writes were approved.

## Related skills
dgtldash-product-access-setup, prospecting-router, prospect-list-segmentation, prospect-source-tracing, email-deliverability-checklist
