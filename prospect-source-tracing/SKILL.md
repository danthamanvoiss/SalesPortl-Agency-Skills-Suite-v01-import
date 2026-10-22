---
name: prospect-source-tracing
description: Prospecting skill that defines and audits a source-to-customer trail so every prospect or lead keeps its origin, campaign touches, and outcome.
version: 0.1.0
tags: [sales, prospecting, lead-generation, lead-data, attribution, tracking, draft-only]
---

## Purpose
Make sure each lead can be traced from where it was found, through enrichment and campaign activity, to engagement and customer outcome.

## Use when
- Setting up a new lead pipeline or CRM fields.
- Results cannot be attributed to a source, list, or campaign.

## Required inputs
- Current lead fields and where leads come from.
- Tools involved (lead sources, outreach, CRM) and what each can report.
- The questions the trail must answer (for example which source produces customers).

## Safety/authority
- Design and audit only; do not alter live records without explicit approval.
- Track only what is needed for the stated purpose; respect consent and privacy rules.

## Workflow
1. List each stage a lead passes through and the system that owns it.
2. Define layered source fields: origin, tool or list, and batch or date.
3. Define touch fields: campaign, message, and engagement events.
4. Define outcome fields: stage changes, conversion, and reason lost.
5. Find gaps where the trail breaks and propose the smallest fix.

## Output format
```
- Stage map
- Field definitions (source, touch, outcome)
- Gaps found
- Fix plan in priority order
```

## Quality checks
- Every lead can answer "where did you come from" in one lookup.
- No field is defined that nothing will fill.

## Related skills
prospect-list-segmentation, pipeline-hygiene-review, salesportl-account-handoff, prospecting-router
