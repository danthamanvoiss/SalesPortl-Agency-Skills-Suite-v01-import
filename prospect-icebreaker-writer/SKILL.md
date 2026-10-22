---
name: prospect-icebreaker-writer
description: Prospecting skill that writes short, relevant, person-to-person icebreaker opening lines for each prospect or lead, saying why you are reaching out without feeling invasive.
version: 0.1.0
tags: [sales, prospecting, outreach, personalization, draft-only]
---

## Purpose
Produce a one-to-two sentence opener per lead that is casual, honest about why you are reaching out, and based only on facts you can point to.

## Use when
- A prepared lead list needs a personalized first line before a campaign.
- An existing sequence opens generically and replies are weak.

## Required inputs
- Lead fields available (name, role, company, website, public profile notes).
- The offer in one sentence and the single question or ask for the first touch.
- Tone preference and any phrases to avoid.

## Safety/authority
- Draft-only; no sending actions.
- Use only information the lead has made public for business purposes; never reference private, family, health, or location-tracking details.
- No invented observations, fake familiarity, or false claims of prior contact.
- If a lead has too little data for a real observation, use a neutral opener instead of fabricating one.

## Workflow
1. Group leads by how much usable data they have (rich, thin, none).
2. For each lead, pick one verifiable, work-related observation.
3. Write the opener in this shape: what you noticed, why it made you reach out, a low-pressure ask (for example "can I send something over?").
4. Flag any line that feels intrusive and rewrite it.
5. Output one column ready to merge into the campaign list.

## Output format
```
- Lead identifier
- Observation used (and where it came from)
- Icebreaker line
- Confidence: specific / generic fallback
```

## Quality checks
- Every line is traceable to a stated source.
- Sounds like one person writing to another, not a template.
- Under 40 words and ends with an easy, optional ask.
- Nothing creepy: no over-precise details.

## Related skills
outreach-sequence-draft, prospect-list-segmentation, anti-slop-content-review, prospecting-router
