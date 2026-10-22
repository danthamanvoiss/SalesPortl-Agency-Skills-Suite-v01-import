---
name: prospect-api-playbook
description: Use Prospect for company enrichment, email finding and verification, and campaign suppression, read-only first and with explicit approval for any write.
version: 0.1.0
tags: [tools-and-products, prospect, enrichment, email, api]
---

## Purpose
Guide safe, efficient use of Prospect (the outreach and enrichment product) for enriching companies, finding and verifying emails, and managing suppression. Works through Prospect's API today and through its MCP tools when they are connected.

## Use when
- A lead list needs company enrichment, email discovery, or email verification.
- An address must be suppressed or unsubscribed across campaigns.

## Required inputs
- The leads or domains to process and what outcome is wanted.
- Confirmation that Prospect access is configured (see Safety/authority).
- For any write action: the exact records and an explicit approval.

## Safety/authority
- Credentials live in a secret or the tool connection, never in chat, files, logs, or examples. If a key is exposed anywhere, treat it as compromised and rotate it.
- Use the endpoint host and tool names shown in your own Prospect account; do not guess them.
- Read-only actions first (enrich company, extract emails from URLs, verify email) on a small sample before batches.
- Write or outreach actions (adding contacts, sending, unsubscribing, stopping sends) need explicit user approval for the specific records, and suppression requests are honored immediately.
- Do not log full responses that contain personal data.

## Workflow
1. Confirm access and pick 3 to 5 sample records.
2. Run read-only calls on the sample and review the results for quality and gaps.
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
lead-list-segmentation, lead-source-tracing, email-deliverability-checklist
