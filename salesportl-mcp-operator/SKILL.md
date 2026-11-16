---
name: salesportl-mcp-operator
description: Operate SalesPortl through its MCP tools (CRM, pipelines, messaging, workflows, AI agents, sites, funnels, forms, bookings, ads, communities, SaaS and sub-accounts) with a context-first, verify-before-done pattern and a tool-call reference.
version: 0.1.0
tags: [tools-and-products, salesportl, mcp, crm, automation, tool-reference]
---

## Purpose
Do real work in a SalesPortl account with the right tools in the right order: identify the account, load the area's context tool, build, then verify the saved result with the platform's own checker before calling anything done.

## Use when
- A task must be done in SalesPortl: contacts, pipelines, tasks, messaging, workflows, AI agents, websites and funnels, forms, calendars and bookings, ads, communities and courses, affiliates, SaaS plans, sub-accounts.
- An integration needs the SalesPortl HTTP API schema.
- The user asks to audit what an account has and whether it works.

## Required inputs
- The outcome and the account (our agency account or a sub-account) [confirm with `whoami`].
- Authority level for changes, sends, publishes, and purchases.
- Named records the task touches (contact, pipeline, site, agent), or permission to look them up.

## Safety/authority
- `whoami` first. A connection acts in one account only; work in a sub-account needs that sub-account's own connection. Never build in the agency account what belongs to a client.
- Before any email or SMS, call `get_messaging_context` and get approval for the exact recipients and text. Sends have a short undo window; scheduled sends can be cancelled with `cancel_scheduled_message`.
- Approval before: `send_email`, `send_sms`, `make_ai_phone_call`, `buy_phone_numbers`, `release_phone_number`, `publish_site`, `publish_form`, `publish_cms_entries`, `activate_ai_agent`, `activate_agent_workflow`, `set_ad_status`, `update_ad_budget`, `create_payout_batch`, `mark_payout_paid`, `apply_snapshot`, every `delete_*`, and anything in billing or SaaS plans.
- Never report something as done, live, or fixed from your own writes; run `verify_artifact` and relay its result.
- `ask_assistant` uses account credits and acts on its own within the app's approval rules; prefer direct tools when the steps are known.

## Workflow
1. `whoami`; then `get_my_account_plan` if a feature may be plan-gated.
2. Load the context tool for the area (table below) before the first write in that area.
3. Read existing records (`list_*`, `get_*`) and reuse them; do not duplicate pipelines, tags, fields, or agents.
4. Build or change with the area's write tools. For workflows, browse `search_workflow_capabilities` and read `get_workflow_capability_docs` for every step and trigger type before `create_agent_workflow`.
5. Verify: `verify_artifact` (stage completion, then activation or publish before going live). For an inherited account, `run_account_audit` (deterministic is free and read-only).
6. Report what changed, the verification result, and what awaits approval.

## Tool-call reference
Context first (read-only): `get_messaging_context`, `get_booking_context`, `get_scheduling_context`, `get_branding_context`, `get_cms_context`, `get_community_context`, `get_custom_objects_context`, `get_dashboard_builder_context`, `get_email_design_context`, `get_global_variable_context`, `get_image_generation_context`, `get_integrations_context`, `get_phone_number_context`, `get_saas_context`, `get_social_posting_context`, `get_team_context`, `get_affiliate_context`, `get_analytics_context`, `get_clicktrack_context`, `get_activity_log_context`, `get_assistant_setup_context`, `get_funnel_design_guide`.

| Area | Read | Write (approval where marked above) |
|---|---|---|
| Account | `whoami`, `get_my_account_plan`, `list_sub_accounts`, `recent_activity`, `run_account_audit` | `create_sub_account`, `update_sub_account`, `set_sub_account_status`, `delete_sub_account`, `create_snapshot`, `apply_snapshot`, `update_snapshot_items` |
| Contacts | `list_contacts`, `get_contact`, `get_contact_activity`, `list_contact_fields`, `list_contact_views`, `get_contact_layout` | `create_contact`, `update_contact`, `update_contact_custom_fields`, `import_contacts`, `add_contact_tags`, `remove_contact_tags`, `create_tags`, `add_note`, `create_custom_field`, `opt_out_contact`, `delete_contact(s)` |
| Custom objects | `list_custom_objects`, `get_custom_object` | `create_custom_object_type`, `create_custom_objects`, `update_custom_objects`, `link_custom_object_contacts`, `move_custom_object_stage` |
| Pipelines | `list_pipelines`, `get_pipeline_leads` | `create_pipeline`, `update_pipeline`, `move_lead_stage`, `convert_contact_pipeline_to_deals` |
| Messaging and inbox | `list_conversations`, `list_channels`, `list_communication_threads`, `list_scheduled_messages`, `list_email_templates` | `send_email`, `send_sms`, `reply_to_customer`, `assign_conversation`, `mark_conversation_done`, `snooze_conversation`, `create_email_channel`, `connect_email_domain`, `record_external_message` |
| Calls and meetings | `search_calls`, `get_call`, `get_call_transcript`, `get_call_stats`, `get_meeting`, `get_meeting_transcript` | `make_ai_phone_call`, `search_available_numbers`, `buy_phone_numbers`, `update_phone_number` |
| SMS registration | `get_sms_registration_status`, `get_dlc_application`, `check_dlc_carrier_status` | `correct_dlc_application`, `resubmit_dlc_application`, `update_toll_free_verification` |
| AI agents | `list_ai_agents`, `get_ai_agent`, `audit_ai_agent_config`, `read_agent_knowledge`, `list_subagents`, `list_ai_agent_versions` | `create_ai_agent`, `update_ai_agent`, `configure_agent_knowledge`, `configure_inline_tools`, `install_agent_template`, `create_subagent`, `create_agent_widget`, `activate_ai_agent`, `restore_ai_agent_version` |
| Agent workflows | `search_workflow_capabilities`, `get_workflow_capability_docs`, `get_workflow_status` | `create_agent_workflow`, `update_workflow_step`, `test_agent_workflow`, `activate_agent_workflow` |
| Workers and scheduled tasks | `list_workers`, `get_worker`, `preview_worker_contacts`, `list_scheduled_tasks`, `get_scheduled_task_runs` | `create_worker`, `update_worker`, `test_worker_on_contact`, `create_scheduled_task`, `run_scheduled_task_now` |
| Tasks and projects | `list_tasks`, `get_task`, `list_projects`, `list_task_templates`, `list_task_statuses`, `get_task_stats` | `create_task(s)`, `update_task`, `complete_task`, `create_project`, `create_task_from_template`, `add_task_comment`, `set_task_trigger` |
| Websites and funnels | `list_websites`, `get_website`, `read_website_file`, `grep_website_files`, `glob_website_files`, `get_funnel_status`, `inspect_web_page` | `create_website`, `import_website`, `patch_website_file`, `multi_edit_website_file`, `create_funnel`, `update_funnel`, `update_page_seo`, `add_custom_domain`, `publish_site`, `purge_cache` |
| CMS | `get_cms_collection`, `list_cms_entries`, `get_cms_entry` | `create_cms_collection`, `create_cms_entries`, `update_cms_entries`, `write_cms_template`, `publish_cms_entries` |
| Forms | `list_forms`, `get_form`, `search_form_elements`, `get_form_element_docs`, `preview_form_visibility` | `create_form_from_plan`, `update_form_element`, `publish_form`, `export_form_submissions` |
| Email design | `get_email_design` | `create_email_design`, `update_email_design` |
| Calendars and bookings | `list_bookables`, `get_bookable`, `list_bookings`, `find_bookings`, `list_scheduling_staff`, `booking_counts` | `create_bookable`, `update_bookable`, `activate_bookable`, `create_booking`, `update_booking`, `cancel_booking`, `create_scheduling_staff` |
| Social | `list_social_posts`, `get_social_post`, `get_social_platform_docs`, `list_social_account_options` | `create_social_post`, `update_social_post`, `connect_social_account` |
| Ads | `list_ad_accounts`, `list_ad_campaigns`, `get_ad_campaign`, `get_ad_insights`, `get_ad_capabilities`, `search_ad_library`, `estimate_reach` | `validate_ad_spec`, `create_ad`, `duplicate_ad_campaign`, `set_ad_status`, `update_ad_budget`, `create_audience_from_contacts` |
| Tracking and analytics | `get_analytics`, `query_analytics`, `query_clicktrack_stats`, `list_tracking_campaigns`, `query_activity_logs`, `get_dashboard` | `create_tracking_campaign`, `create_traffic_source`, `add_tracking_domain`, `create_dashboard`, `update_dashboard`, `enable_conversion_forwarding` |
| Communities and courses | `list_communities`, `get_community_feed`, `get_community_course`, `list_community_members`, `get_community_insights` | `create_community`, `create_community_post`, `create_community_course`, `publish_community_course`, `invite_community_members`, `moderate_community` |
| Affiliates | `list_affiliates`, `get_affiliate`, `list_commissions`, `list_affiliate_payouts` | `create_affiliate`, `decide_affiliate_application`, `review_commission`, `create_payout_batch`, `mark_payout_paid` |
| SaaS and team | `get_saas_plan`, `list_users`, `get_team_context` | `create_saas_plan`, `update_saas_plan`, `update_saas_branding`, `invite_team_users`, `create_team_role` |
| Integrations and assistant | `list_assistant_connectors`, `list_payment_integrations`, `get_integrations_context` | `connect_integration`, `test_integration`, `create_assistant_connector`, `test_assistant_connector`, `ask_assistant`, `check_assistant_run`, `resolve_assistant_approval` |
| Files and images | `list_files`, `list_library_files`, `read_library_file`, `list_generated_images`, `search_stock_images` | `upload_file`, `save_library_file`, `generate_images`, `remove_background`, `import_stock_image` |
| Domains | `check_domain_status`, `lookup_dns` | `add_domain`, `verify_domain`, `set_primary_domain` |
| Help and API | `search_help_docs`, `read_help_article`, `get_api_reference`, `list_changelog` | none |
| Verification | `verify_artifact`, `run_account_audit`, `get_completion_requirements` | none |

For HTTP integrations (outside the MCP), read the endpoint with `get_api_reference` (for example `POST /api/leads`); never guess body fields. API keys live in the platform's API settings and in environment secrets, never in skills or chat.

## Output format
```
- Account acted in
- Context tools read
- Records read / created / changed (ids)
- verify_artifact or audit result (relayed)
- Awaiting approval (exact action)
- Next step
```

## Quality checks
- `whoami` and the area context tool ran before the first write.
- Nothing is called done without a `verify_artifact` pass.
- No duplicate pipelines, fields, tags, or agents were created where one existed.
- No message, publish, purchase, or delete ran without recorded approval.

## Related skills
dgtldash-tools-router, dgtldash-capability-check, salesportl-prospect-pipeline, plusone-outreach-operator, hiscore-loyalty-operator, crm-workflow-blueprint, toolbox-mcp-builder
