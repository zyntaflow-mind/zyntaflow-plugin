---
name: instagram-automations
description: >
  Designs, reviews, creates, updates, and manages Instagram comment-to-DM and message
  automation flows in Zynta Flow. Use when the user asks to automate Instagram comment
  replies, send a link or lead magnet via DM on a keyword, respond to story replies or
  mentions, inspect existing automation flows, or pause/edit/delete an automation. Not for
  post creation (use create-carousel) or calendar planning (use editorial-planning).
---

# Instagram Comment-to-DM & Message Automations

Build and maintain Instagram automation flows in Zynta Flow so users can inspect and visually edit every step inside the web app.

## Step 1: Audit existing flows and connected Instagram accounts

1. Call `list_automation_flows` (and `list_automations` if checking legacy single-trigger rules) to see:
   - Which Instagram accounts (`social_account_id`) are connected and accessible.
   - What flows already exist, so you do not create colliding keyword triggers.
2. If no Instagram account is connected, call `get_zynta_help` with `topic: "connections"` and give the user the exact app link to connect Instagram.

## Step 2: Confirm required trigger details before creating

Before calling `create_automation_flow` (or `create_automation_rule`), confirm:
1. **Trigger type:** `comment`, `message`, `story_reply`, or `story_mention`.
2. **Keywords & match mode:** `contains` (whole word/phrase, default), `partial`, or `exact` — or empty for catch-all.
3. **For `comment` triggers, ALWAYS ask `media_scope`:**
   - `specific` (one or more `media_ids`)
   - `next` (locks onto the next post the account publishes)
   - `since` (posts published from `media_since` onward)
   - `all` (every past and future post — **never assume `all` silently**, as a keyword will fire on any later post that mentions it).
4. **Delivery assets (PDF, image, video):**
   - Instagram only allows sending a file inside a DM after the person has written or tapped a button.
   - In a `comment` flow, put `file_url` on a `send_message` step reached through a button (up to 3 quick-reply buttons, max 20 characters per label), not on the very first DM after a comment.
   - If the user has a local file, import it first via `import_media_file` or `create_media_upload` to get a Zynta public URL.
5. **Initial status:** new flows default to `draft` so the user can review them safely; only set `live` if the user explicitly asks to activate it immediately.

## Step 3: Build or update the flow

- **Prefer `create_automation_flow`** even for a simple keyword + reply + DM: every flow is visible and editable step-by-step on Zynta's visual canvas (`comment_reply`, `send_message`, `action`, `condition`, `randomizer`, `delay`, `start_flow`).
- **Editing an existing flow:** read it first with `get_automation_flow`. Because `update_automation_flow` replaces the full `steps` list when `steps` is supplied, preserve every step the user did not ask to remove.
- **Deleting:** `delete_automation_flow` and `delete_automation_rule` are permanent — confirm with the user before deleting.

## Step 4: Hand back the editor link

After creating or updating a flow, return:
- A concise summary of trigger, scope (`media_scope`), keywords, status (`draft` or `live`), and step sequence.
- The Zynta Flow URL from the tool response so the user can inspect or test the flow in the canvas.

## Guardrails

- **Plan or email verification gates:** if an account has no active plan/trial or an unverified email, automation write tools return `403` and change nothing. Surface the returned explanation and remedy link clearly.
- **Button label limit:** Instagram truncates quick-reply chips at 20 characters — keep every button `label` ≤ 20 characters.
- **Unique step IDs:** never use `"trigger"` as a step `id` (reserved). Every step must link cleanly via `next`, `yes`/`no`, branch `next`, or button `next`.
