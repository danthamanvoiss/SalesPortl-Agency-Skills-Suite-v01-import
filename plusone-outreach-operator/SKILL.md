---
name: plusone-outreach-operator
description: Operate PlusOne LinkedIn and email outreach through its MCP tools (campaigns, audience sources, sequences, leads, unified inbox, AI reply agents, knowledge base, analytics) with the build order, approval gates, and a tool-call reference.
version: 0.1.0
tags: [tools-and-products, plusone, outreach, linkedin, email, mcp, tool-reference]
---

## Purpose
Build and run PlusOne outreach correctly the first time: right account, right audience source, a validated sequence, and no message sent until the user approves the launch.

## Use when
- A LinkedIn or email outreach campaign must be created, changed, paused, or reviewed in PlusOne.
- Replies in the PlusOne inbox need triage, drafts, or a reply agent.
- Someone asks how campaigns, sequences, or sending accounts are performing.

## Required inputs
- The offer and the audience criteria, or a search URL the user pasted.
- Which LinkedIn account sends, and whether it has Sales Navigator, Recruiter, both, or neither [ask; the tools cannot tell].
- Which mailboxes, if the sequence has email steps [ask].
- Schedule, daily limits, and authority level for launch.

## Safety/authority
- `whoami` first: the plan decides whether Pro steps (EMAIL, QUALIFIER, GPT, FINDER, EVENT) exist. Then `reference` before writing any sequence or status filter; the vocabulary is not guessable (campaign limits live under `campaginLimits`, spelled that way).
- Never pick the LinkedIn account or mailboxes for the user, never invent a URL, id, tag, or sender, and never copy settings from another campaign. A wrong URL sends real messages to the wrong people.
- Build sequences only with `flow_set` (or `flow_from_template`). Do not edit a draft with `sequence_update`; launch rebuilds steps from the flow.
- Approval before: `campaign_launch`, any `campaign_set_status` that starts or resumes sending, `message_send`, `campaign_send_test_email`, attaching an agent to a live campaign, `campaign_upload_leads` or `campaign_add_prospects` into a running campaign, and every `*_delete`.
- InMails need credits the tools cannot see; raise it before launch. Do not promise zero account risk.

## Workflow
1. `whoami`, `reference` (topic as needed), `linkedin_account_list`, `email_account_list`.
2. `campaign_create` (a draft).
3. Audience: build the URL with `search_url_build` from titles and keywords; geography, industry, company size, and seniority must come from a URL the user pastes. Show the URL, then `campaign_set_source`. CSV leads go through `campaign_upload_leads`.
4. `campaign_update`: sending account (`linkedAccountId`), mailboxes (`emailAccountIds`), schedule, limits.
5. Sequence: `flow_set` with ordered steps (or `flow_from_template`); read back with `flow_get`; `campaign_preview_message` to check personalisation.
6. `campaign_get` and confirm `_readiness.blockers` is empty. Ask for launch approval; then `campaign_launch`.
7. Run: `conversation_list`, `conversation_get`, `message_draft_ai_reply` (draft), `unread_counts`; sync warm leads to the CRM per `plusone-cold-outreach-sync`.
8. Measure: `analytics_campaign`, `analytics_campaign_funnel`, `sequence_step_stats`, `campaign_activity_summary`. Prefer per-campaign analytics; the account roll-up can be slow.

## Tool-call reference
| Area | Read | Write (approval where marked above) |
|---|---|---|
| Orientation | `whoami`, `reference`, `help_videos`, `subaccount_list`, `team_members_list` | `account_settings_update` |
| Campaigns | `campaign_list`, `campaign_get`, `campaign_logs`, `campaign_activity_summary`, `campaign_export_stats`, `campaign_preview_message` | `campaign_create`, `campaign_update`, `campaign_set_source`, `campaign_set_status`, `campaign_launch`, `campaign_duplicate` (only when asked by name), `campaign_create_ab_variant`, `campaign_add_prospects`, `campaign_upload_leads`, `campaign_send_test_email`, `campaign_delete` |
| Audience | `search_url_build` | `campaign_set_source` |
| Sequences | `flow_get`, `flow_history`, `flow_audit`, `sequence_list`, `sequence_step_stats`, `sequence_leads_at_step` | `flow_set`, `flow_from_template`, `campaign_generate_ai_sequence`, `sequence_update` (running campaigns only) |
| Leads | `lead_list`, `lead_get`, `lead_flow_state`, `lead_notes_list`, `lead_export` | `lead_add`, `lead_update`, `lead_move`, `lead_note_add`, `lead_note_update`, `lead_blacklist`, `lead_delete` |
| Inbox | `conversation_list`, `conversation_get`, `unread_counts`, `notification_list`, `saved_reply_list` | `message_draft_ai_reply`, `message_send`, `message_delete`, `conversation_update`, `conversation_mark_all_read`, `saved_reply_upsert`, `notification_mark_read` |
| Agents and knowledge | `agent_list`, `agent_get`, `agent_logs`, `agent_stats`, `agent_query` (dry run), `kb_list` | `agent_upsert`, `agent_train`, `agent_duplicate`, `agent_delete`, `kb_upsert`, `kb_attach`, `kb_delete` |
| Personas, templates, placeholders | `persona_list`, `persona_get`, `template_list`, `template_get`, `placeholder_list` | `persona_create`, `persona_update`, `persona_delete`, `template_upsert`, `placeholder_add`, `placeholder_delete` |
| Tags | `tag_list` | `tag_create`, `tag_update`, `tag_delete` |
| Blacklist | `blacklist_list`, `blacklist_export` | `blacklist_add`, `blacklist_add_bulk`, `blacklist_copy`, `blacklist_delete` |
| Sending accounts | `linkedin_account_list`, `linkedin_account_get`, `linkedin_account_stats`, `linkedin_account_daily_stats`, `linkedin_account_ssi`, `email_account_list`, `email_account_get`, `email_account_stats` | `linkedin_account_update`, `linkedin_account_refresh`, `email_account_add_imap`, `email_account_oauth_url`, `email_account_update` |
| Automations | `automation_list`, `automation_get` | `automation_upsert`, `automation_duplicate` |
| Content posts | `post_list`, `post_schedule_get`, `post_schedule_settings` | `post_generate`, `post_upsert`, `post_schedule_set` |
| Tasks and reminders | `task_list`, `reminder_list`, `activity_list`, `appointment_list`, `appointment_get` | `task_upsert`, `reminder_create`, `reminder_delete`, `activity_skip` |
| Schedule | `holiday_list` | `holiday_add`, `holiday_delete` |
| Analytics | `analytics_campaign`, `analytics_campaign_funnel`, `analytics_trend`, `analytics_reporting`, `analytics_account` | none |

Reply agent order: voice and instructions (`agent_upsert`), knowledge (`kb_upsert`, `kb_attach`), `agent_train`, dry run with `agent_query`, and only then attach it to a campaign with approval.

## Output format
```
- Account, plan, and Pro steps available
- Sending account and mailboxes (as the user chose)
- Audience source URL (as shown to the user)
- Sequence summary (steps, waits, branches)
- Readiness blockers: none / list
- Awaiting approval (launch, sends, agent attach)
```

## Quality checks
- No account, mailbox, URL, or tag was chosen or invented by the agent.
- The sequence was written with `flow_set` and read back with `flow_get`.
- `_readiness.blockers` was empty before launch was proposed.
- Suppression (blacklist, opt-outs) was respected and synced to the CRM.

## Related skills
dgtldash-tools-router, dgtldash-capability-check, plusone-cold-outreach-sync, outreach-sequence-draft, email-deliverability-checklist, prospect-icebreaker-writer, salesportl-mcp-operator
