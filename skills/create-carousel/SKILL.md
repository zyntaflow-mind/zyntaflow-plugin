---
name: create-carousel
description: >
  Creates and refines on-brand social media carousels in Zynta Flow — picks a brand kit and
  design preset, generates slide copy, optionally attaches photos and adjusts slide layout,
  sets a caption, and saves a non-publishing draft to the content calendar. Use when the user
  asks to create, write, design, edit, or add images to a carousel for Instagram, LinkedIn,
  TikTok, or Bluesky in Zynta Flow. Not for multi-week editorial calendars (use
  editorial-planning) or Instagram DM flows (use instagram-automations).
---

# Create & Refine a Carousel in Zynta Flow

Turn a topic, brief, or existing calendar idea into a finished, on-brand carousel saved directly in the user's Zynta Flow account.

## Step 1: Identify the brand and check context

1. Call `list_brand_guides` to inspect the brands available on the account.
   - If the user has multiple brands and did not name one, list the brands and ask which one to use before generating anything.
2. Call `get_brand_guide` on the chosen brand when you need its products, personas, tone of voice, or palette details.
   - If the user mentions a target audience or persona, note its `persona_id` for `generate_carousel`.
3. If the carousel belongs to an existing calendar item, call `list_calendar_events` (or `get_calendar_event`) to pull the `calendar_event_id` so the brief and link stay connected.

## Step 2: Pick a template preset

Call `list_presets` (with `detailed: true` when comparing slide structures, palettes, or character limits) and choose a preset that matches the user's format and slide count. If the user did not specify a preset, pick a clean default and state which preset you chose.

## Step 3: Check cost and generate the draft

`generate_carousel` spends account credits (same rate as the web app).

- If the user has not already confirmed generating a paid carousel, mention that it uses account credits (or check balance via `get_content_action_costs` if they ask about pricing).
- Call `generate_carousel` with `brand_guide_id`, `preset_id`, topic/brief, and optional `persona_id`, `calendar_event_id`, or `use_brand_palette`.
- After generation, return:
  - The slide-by-slide summary of the copy.
  - The editor link (`edit_url`) so the user can review and export in Zynta Flow.

## Step 4: Polish slides, images, and caption (when requested)

- **Edit slide text, colors, fonts, or display flags:** inspect first with `get_carousel`, then apply changes with `update_carousel` (omitted fields stay intact).
- **Add contextual stock photos:** call `attach_unsplash_images` (spends credits; existing photos are kept unless `regenerate: true`).
- **Attach user photos:**
  - From a public URL: `attach_slide_image`.
  - From a local or attached file: import with `import_media_file` or obtain an upload slot with `create_media_upload`, then attach the resulting public URL.
- **Adjust framing or positions:** use `adjust_slide_layout`.
- **Save on the calendar as a safe draft:** use `create_calendar_event` or `update_calendar_event` with status `ready` (or `creating`) and set the caption with `set_caption`.

## Guardrails

- **Never schedule or publish silently.** Setting a `scheduled_date` on a draft (`idea`, `creating`, or `ready`) never publishes; only set status `scheduled` or call `publish_calendar_event` when the user explicitly asks to schedule or publish.
- **Find exported or published carousels with `list_carousels`.** `list_drafts` returns recent drafts only; use `list_carousels` when searching across drafts, exports, and published carousels.
- **Credit or plan errors:** if a tool returns a 402 or 403 with a remedy link (buy credits or subscribe), surface that exact link to the user — payments happen in the Zynta Flow web app.
