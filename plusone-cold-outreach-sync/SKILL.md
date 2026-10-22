---
name: plusone-cold-outreach-sync
description: Plan and run cold outreach in PlusOne with a two-way lead sync to SalesPortl, so leads are pushed out for cold campaigns and engagement signals flow back to decide who to action as warm.
version: 0.1.0
tags: [tools-and-products, prospecting, plusone, salesportl, cold-outreach, sync]
---

## Purpose
Use PlusOne as the cold outreach platform while SalesPortl holds the lead record. Leads go from SalesPortl to PlusOne for campaigns; replies, clicks, interest, bounces, and opt-outs come back so the team knows when to move a prospect from cold prospecting to warm sales activity.

## Use when
- A segment of prospects is ready for a cold campaign.
- Engagement events from outreach must update stages, tags, and scores in the CRM.
- Suppression and opt-outs must stay consistent across both tools.

## Required inputs
- The segment to push: verified emails only, ICP-tagged, source recorded.
- Campaign brief: offer, opening lines, sequence, sending limits.
- How the two tools connect right now: connected tools, API, or webhook [if unknown, ask; if PlusOne access is missing, point to `dgtldash-product-access-setup`].
- The cold-to-warm rule from `salesportl-prospect-pipeline`.

## Safety/authority
- Draft-only until the user approves the exact segment and campaign; no sending from this skill.
- Check `email-deliverability-checklist` before any campaign launches.
- Suppression always wins: an opt-out or bounce in either tool is applied in both, immediately.
- Use only fields the user approved for sync; do not move personal data that the purpose does not need.
- Do not invent PlusOne features, event names, or limits; confirm them in PlusOne or its tools and mark unknowns as [confirm in PlusOne].

## Workflow
1. Select the segment in SalesPortl (tag or field based) and confirm size, verification status, and exclusions.
2. Map fields: email as the unique key, plus name, company, source layers, offer, opening line, and the SalesPortl record identifier.
3. Push the segment to PlusOne in a controlled first batch; check the sample before the full push.
4. Launch the campaign with limits agreed in the brief.
5. Return events to SalesPortl by webhook or API: reply, click, positive interest, bounce, unsubscribe. Update tags and score, and move the stage per the rule.
6. Flag warm leads for action: create the task or start the warm workflow, and pause cold steps for that lead so nobody gets both.
7. Sync suppression both ways and keep a log of what moved and why.
8. Review results by segment and source, then adjust list quality, timing, or copy.

## Output format
```
- Segment and size pushed
- Field mapping
- Campaign summary and limits
- Event-to-action rules (event -> tag/score/stage/task)
- Suppression sync check
- Results by segment and source
- Items awaiting approval
```

## Quality checks
- Each lead exists once per tool, matched by email, and carries its source.
- No lead receives cold and warm outreach at the same time.
- Opt-outs and bounces are reflected in both tools before the next send.

## Related skills
prospecting-router, salesportl-prospect-pipeline, prospect-icebreaker-writer, email-deliverability-checklist, outreach-sequence-draft, dgtldash-product-access-setup
