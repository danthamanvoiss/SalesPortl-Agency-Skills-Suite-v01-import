---
name: prospect-browser-use-company-export
description: Build and download a company list from Prospect's Company Explorer through the browser: pick country, industry and size, create the report, wait, and save the CSV.
version: 0.1.0
tags: [tools-and-products, prospecting, lead-generation, prospect, browser-use, export]
---

## Purpose
Pull a targeted company list (name, domain, industry, location, LinkedIn page, size, phone) out of Prospect's built-in Company Explorer using its own export flow. This is the browser-use path; the API path is `prospect-api-playbook`. Keep the two separate.

## Use when
- The user wants a list of companies for a country, industry and size band to feed lead generation.
- A list of domains is needed before looking up contacts or emails.

## Required inputs
- Country, industry and company size band (the explorer's own lists), and where to save the file.
- Sign-in details held in environment variables the user has set, and a browser tool.

## Safety/authority
- Run `dgtldash-capability-check` first (browser tool, credentials, subscription).
- Only use the explorer, its report history and its download links. Do not open billing, plan, subscription, or settings pages.
- Create one report at a time and wait for it to finish before the next, so the platform is not overloaded.
- Exports contain business contact data: save them where the user said, keep them out of public repos, and keep each export as its own raw file rather than merging into a master without approval.

## Workflow
1. Sign in at the product's login page and open Enrichment, then Find B2B companies (Company Explorer).
2. Pick a country, then an industry, then a size band. The page then shows an estimated count; note it and confirm it is a sensible size.
3. Choose "Create a new report". The task appears in the export history (Access to my exports) as in progress.
4. Poll the history every 20 to 30 seconds. Large lists (about 21,000 rows) finished in about two minutes.
5. When done, take the CSV link from the history row and download it using the signed-in session. XLSX is also offered.
6. Check the file: row count against the estimate, how many rows have a domain and a phone.
7. Report counts and next steps (contacts lookup, email finding, verification).

## What testing showed (2026-10-23)
- US, Insurance, 1 to 10 employees: estimate 21,326; the file had 21,326 rows, about 85% with a domain and about 40% with a phone.
- The file holds companies only: no people and no emails.
- Missing values are written as the literal text `nan`; treat it as empty when loading.
- Names are lowercased in places and some rows look misfiled (for example a car dealer under insurance), so apply your own category checks.

## Output format
```
- Selection: country / industry / size
- Estimate vs rows downloaded
- Rows with domain / phone
- File location and issues found
- Suggested next step
```

## Quality checks
- Row count matches the estimate closely.
- `nan` values are cleaned before loading.
- No credentials or personal data appear in the report.

## Related skills
dgtldash-capability-check, prospect-api-playbook, prospect-list-segmentation, prospect-source-tracing, prospecting-router
