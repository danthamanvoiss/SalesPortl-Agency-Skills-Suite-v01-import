---
name: hiscore-loyalty-operator
description: Operate HiScore loyalty and membership cards through its MCP tools (card templates, customers, cards, stamps and points, pushes, promotions, fortune-wheel games, loyalty workflows, locations, white-label branding) with a tool-call reference and approval gates.
version: 0.1.0
tags: [tools-and-products, hiscore, loyalty, membership, mcp, tool-reference]
---

## Purpose
Build and run digital wallet loyalty programs in HiScore: choose the right card mechanic, set up the template, issue and adjust cards, and reach card holders with pushes, promotions, and games, without sending anything or changing balances by accident.

## Use when
- A business wants a loyalty, membership, gift, coupon, cashback, or punch-card program.
- Card holders need a push message, a promotion, a review-gated game, or an automated loyalty workflow.
- Someone asks how a loyalty program is performing (revenue, rewards, UTM sources, operations).
- Brand assets for a white-label dashboard, scanner, or promo page need to be read or updated.

## Required inputs
- The company the work is for [confirm with `whoami`; agencies may act as a client company].
- The program goal and mechanic, or permission to recommend one.
- Brand assets (logo, colors) and the unique identifier field for customers (phone or email).
- Authority level for pushes, SMS, balance changes, publishing, and deletes.

## Safety/authority
- `whoami` first: it names the company every tool acts in, and whether an impersonation is active.
- Pushes cannot be recalled and are not idempotent. Get approval for the exact text, template, and audience before `send_push`; after a timeout, check `list_pushes` before ever resending. `all_cards` can reach a whole franchise, wider than one template.
- Approval before: `send_push`, `send_sms`, `create_custom_autopush`, `activate_template`, `activate_workflow`, `run_workflow`, `update_game` with active true, and every `delete_*`.
- Balance tools (`add_*_to_card`, `subtract_*_from_card`, `redeem_coupon_card`, `receive_reward_from_card`) change a real customer's value. Use only on a named card with a stated reason; never batch them without approval.
- A game with a coupon sector of weight above 0 publishes on create. Create games with `active` false and review first.

## Workflow
1. `whoami`; `list_templates` and `list_locations` to see what exists.
2. Pick the mechanic: stamp (punch card), reward (points, spend, visits), cashback, discount (tiered), subscription (visit packs), certificate (gift card), coupon (first visit, converts to another card), membership (tiers, paid or free).
3. Draft the template: name, type, custom fields with one unique phone or email field, mechanics, settings (expiry, welcome bonuses, referral, legal), appearance. `create_template`, then add tiers or feedback links with the template tools. Keep it inactive until approved.
4. Distribution: install links and QR from `get_template`; tracked links with `create_utm_link`; a review-gated wheel with `create_game` (pass `active` false; needs Games enabled, and its coupon prize must be an active coupon template) and `create_game_utm_link`.
5. Engagement: built-in autopushes via `list_template_autopushes` and `update_template_autopush`; custom ones via `create_custom_autopush`; segments via `list_segments`; automations via `list_workflow_triggers`, `list_workflow_actions`, `list_workflow_filters`, then `create_workflow` (paused until approved).
6. Measure: `get_revenue_statistics`, `get_template_reward_stats`, `get_template_utm_stats`, `get_game_stats`, `list_operations`.
7. Report and list what awaits approval.

## Tool-call reference
| Area | Read | Write (approval where marked above) |
|---|---|---|
| Account | `whoami` | `update_profile` |
| Templates | `list_templates`, `get_template`, `list_membership_tiers`, `get_template_reward_stats`, `get_template_utm_stats` | `create_template`, `update_template`, `activate_template`, `delete_template`, `add_reward_tier_to_template`, `update_reward_tier_in_template`, `remove_reward_tier_from_template`, `add_membership_tier_to_template`, `update_membership_tier_in_template`, `remove_membership_tier_from_template`, `add_feedback_link_to_template`, `update_feedback_link_in_template`, `remove_feedback_link_from_template` |
| Customers | `list_customers`, `get_customer` | `create_customer`, `update_customer`, `delete_customer` |
| Cards | `list_cards`, `get_card`, `list_operations` | `create_card`, `update_card`, `delete_card`, `set_card_expiration`, `set_card_membership_tier`, `reset_membership_renewal_date`, `restore_membership_visits` |
| Balances | `get_card`, `list_operations` | `add_stamp_to_card`, `subtract_stamp_from_card`, `add_point_to_card`, `subtract_point_from_card`, `add_visit_to_card`, `subtract_visit_from_card`, `add_amount_to_card`, `subtract_amount_from_card`, `add_purchase_to_card`, `subtract_purchase_from_card`, `add_scores_to_card`, `subtract_scores_from_card`, `add_reward_to_card`, `subtract_reward_from_card`, `receive_reward_from_card`, `redeem_coupon_card` |
| Pushes and SMS | `list_pushes`, `get_push`, `list_template_autopushes`, `list_segments` | `send_push`, `send_sms`, `create_custom_autopush`, `update_custom_autopush`, `delete_custom_autopush`, `update_template_autopush` |
| Promotions | `list_promotions`, `get_promotion` | `create_promotion`, `update_promotion`, `delete_promotion` |
| Games | `list_games`, `get_game`, `get_game_stats`, `list_game_utm_links`, `get_game_utm_link` | `create_game`, `update_game`, `delete_game`, `create_game_utm_link`, `delete_game_utm_link` |
| Tracking links | `list_utm_links`, `get_utm_link` | `create_utm_link`, `update_utm_link`, `delete_utm_link` |
| Workflows | `list_workflows`, `get_workflow`, `list_workflow_triggers`, `list_workflow_actions`, `list_workflow_filters`, `list_workflow_logs` | `create_workflow`, `update_workflow`, `activate_workflow`, `pause_workflow`, `run_workflow`, `delete_workflow` |
| Locations and managers | `list_locations`, `get_location`, `list_managers`, `get_manager`, `list_manager_permissions` | `create_location`, `update_location`, `delete_location`, `create_manager`, `update_manager`, `delete_manager` |
| Webhooks | `list_webhooks` | `create_webhook`, `delete_webhook` |
| Revenue | `get_revenue_statistics` | none |
| White label | `list_white_label_domains`, `get_white_label_dashboard_branding`, `get_white_label_scanner_branding`, `get_white_label_promo_branding`, `get_white_label_branding_images`, `get_white_label_subaccount_menu` | `connect_white_label_domain`, `delete_white_label_domain`, `update_white_label_dashboard_branding`, `update_white_label_scanner_branding`, `update_white_label_promo_branding`, `upload_white_label_branding_images`, `delete_white_label_branding_images`, `update_white_label_subaccount_menu` |

Push text is shown exactly as written. Cards delivered through a messenger read it as Markdown, so avoid unpaired asterisks, underscores, backticks, or square brackets.

## Output format
```
- Company acted in
- Program design (mechanic, fields, rewards, expiry)
- Records created or changed (ids), all inactive unless approved
- Distribution links (install, QR, UTM, game)
- Metrics read
- Awaiting approval (exact push text, audience, activation)
```

## Quality checks
- The template has exactly one unique phone or email field.
- Nothing customer-facing went live or was sent without approval.
- No balance was changed without a named card and reason.
- After a push, `get_push` confirmed its status; no duplicate sends.

## Related skills
dgtldash-tools-router, dgtldash-capability-check, salesportl-mcp-operator, retention-and-upsell-review, local-visibility-review, campaign-plan
