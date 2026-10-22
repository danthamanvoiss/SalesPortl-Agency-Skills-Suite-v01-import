---
name: email-deliverability-checklist
description: Pre-send checklist for cold email covering domain setup, list hygiene, volume ramp, content, and opt-out compliance.
version: 0.1.0
tags: [sales, prospecting, outreach, email, deliverability, compliance, read-only]
---

## Purpose
Catch the common causes of spam placement and compliance problems before a campaign goes out.

## Use when
- Before launching any cold or bulk email campaign.
- Replies or opens drop suddenly and the cause is unclear.

## Required inputs
- Sending domain and mailbox setup details.
- List source and how emails were verified.
- Planned daily volume, sequence copy, and target regions.

## Safety/authority
- Read-only review; do not send, change DNS, or edit mailboxes.
- Compliance notes are general guidance, not legal advice; recommend counsel for unclear regions.
- Never advise tactics meant to evade spam filters or hide the sender.

## Workflow
1. Check authentication records (SPF, DKIM, DMARC) are reported as set up.
2. Check list hygiene: verified addresses, duplicates removed, suppression and unsubscribe lists applied.
3. Check volume: new mailboxes warmed up, daily limit ramped gradually.
4. Check content: plain, honest subject and body, limited links, no misleading claims.
5. Check compliance: sender identity, physical address where required, working opt-out, regional rules noted.
6. Return a go / fix-first verdict with the list of fixes.

## Output format
```
- Verdict: go / fix first
- Findings by area (setup, list, volume, content, compliance)
- Required fixes
- Suggested monitoring after launch (bounces, complaints, replies)
```

## Quality checks
- Each finding names the evidence checked or marks it as unverified.
- Fixes are specific and ordered by impact.

## Related skills
outreach-sequence-draft, campaign-plan, pipeline-hygiene-review, prospecting-router
