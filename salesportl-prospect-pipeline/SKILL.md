---
name: salesportl-prospect-pipeline
description: Set up and run the prospect pipeline in SalesPortl: import or push leads, tag source and ICP, move prospects from cold to engaged to sales, and trigger actions from stage, tag, field and score changes.
version: 0.1.0
tags: [tools-and-products, prospecting, salesportl, crm, pipeline, lead-generation]
---

## Purpose
Use SalesPortl as the home of the prospect pipeline: every prospect keeps its source, ICP tags, and engagement history, and moves from cold prospect to warm, opted-in, or engaged lead to active sales with automations firing at the right moments.

## Use when
- Leads from prospecting need to be loaded, organized, and worked in SalesPortl.
- Cold prospects need clear rules for when they become warm or sales-ready.
- A workflow should fire when a prospect changes stage, gets a tag, or reaches a score.

## Required inputs
- The lead list or source and its fields (see `prospect-list-segmentation`).
- The offer(s) and the definition of cold, warm, engaged, and sales-ready for this business.
- Access to the SalesPortl workspace (UI, API key, or connected tool) [if not available, produce the setup plan only and point to `dgtldash-product-access-setup`].
- Where cold outreach runs (for example PlusOne) and how events come back (see `plusone-cold-outreach-sync`).

## Safety/authority
- Draft the plan and field mapping first; imports, bulk actions, workflow activation, and sends need explicit approval.
- Never overwrite existing contacts blindly; rely on deduplication and review matches first.
- Respect opt-outs: unsubscribed contacts stay out of lists and campaigns, and only the contact can opt back in.
- Do not store credentials in contact fields, workflow text, or notes.

## Workflow
1. Design the pipeline stages with clear exit rules, for example: Cold Prospect, Contacted, Engaged, Qualified, Sales Active, Won, Lost (adjust names to the business; stage names are labels, rules matter).
2. Create custom fields for what prospecting needs: lead source layers (origin, tool, list), ICP match per offer, business category, enrichment status, and next-action date.
3. Load leads: import CSV or Excel with field mapping, or create them through the API in bulk. Both deduplicate on email or phone; review skipped or merged records.
4. Tag leads on arrival (offer, segment, source) so lists and segments can be built from tags and fields.
5. Turn on lead scoring so engagement (opens, clicks, replies, bookings, form submissions) moves the score; set heavier weight on replies and bookings and negative weight on unsubscribes.
6. Add automations: stage change, tag applied, field condition, or webhook received can each start a workflow (for example notify an owner, create a task, enroll in a warm sequence, or call out to another tool).
7. Define the cold-to-warm rule in plain terms (for example a reply, a booked call, or a score threshold) and apply it the same way every time.
8. Report a weekly snapshot: counts by stage, source performance, and stalled prospects.

## Output format
```
- Pipeline stages and exit rules
- Custom fields and tags to create
- Load method and field mapping
- Scoring weights
- Automations (trigger -> action)
- Cold-to-warm rule
- Items awaiting approval
```

## Quality checks
- Every prospect can show its source and current stage reason.
- Duplicates and opt-outs are handled before any outreach.
- Automations match the stage rules and do not double-contact.

## Related skills
prospecting-router, prospect-list-segmentation, prospect-source-tracing, plusone-cold-outreach-sync, crm-workflow-blueprint, pipeline-hygiene-review, dgtldash-product-access-setup
