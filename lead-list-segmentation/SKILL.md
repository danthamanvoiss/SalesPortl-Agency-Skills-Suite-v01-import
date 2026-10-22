---
name: lead-list-segmentation
description: Turn a raw lead list into ICP-tagged segments by offer, with a clear rule for which leads get paid enrichment.
version: 0.1.0
tags: [sales, lead-data, segmentation, icp, draft-only]
---

## Purpose
Organize combined lead data into segments per product or service offering, tag ICP fit, and decide where limited enrichment credits are spent.

## Use when
- New lead lists arrive and need to be sorted before any outreach.
- A campaign needs a focused list from a larger master list.

## Required inputs
- The lead list or a representative sample with its column names.
- ICP definition per offer: who qualifies, who is excluded.
- Enrichment budget (credits or none) and which fields matter most (for example email).

## Safety/authority
- Work on copies; never delete or overwrite the master list. Archive instead of removing.
- Do not combine or export personal data beyond what the stated purpose and applicable privacy rules allow.
- Mark assumptions about ICP fit as assumptions.

## Workflow
1. Profile the list: row count, missing fields, duplicates, source breakdown.
2. Normalize obvious fields (company, domain, role, location) without discarding originals.
3. Apply the ICP rules per offer and tag each lead with matches, exclusions, and a fit score.
4. Rank matched leads and tag the best ones for paid enrichment; keep free-data matches in the master.
5. Produce the segment lists plus a short summary of counts and rules used.

## Output format
```
- List profile (rows, gaps, duplicates)
- Segment definitions and rules
- Tag columns added (offer match, fit score, enrich flag)
- Segment counts
- Recommended next step per segment
```

## Quality checks
- Every tag can be explained by a rule.
- Master list is unchanged apart from added tag columns.
- Excluded groups are listed, not silently dropped.

## Related skills
lead-qualification, lead-source-tracing, icebreaker-writer
