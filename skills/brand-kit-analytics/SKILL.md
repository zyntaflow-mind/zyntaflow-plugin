---
name: brand-kit-analytics
description: >
  Manages Zynta Flow brand kits (identity, tone, colors, fonts, products, personas), updates
  link-in-bio pages, and analyzes Instagram post performance to identify top posts and best
  posting times. Use when the user asks to set up or update a brand kit, edit their link in
  bio, check connected social networks, review Instagram analytics, or ask how-to questions
  about their Zynta Flow account. Not for generating carousels (use create-carousel) or
  building DM flows (use instagram-automations).
---

# Brand Kits, Link in Bio & Post Analytics

Keep brand configuration accurate across every generated post, maintain each brand's link-in-bio page, and turn Instagram performance metrics into concrete next-post ideas.

## Workflow A: Set up or update a Brand Kit

1. **Create a new brand kit (`create_brand_guide`):**
   - Gather facts from what the user supplies (website, brand book, PDF, or brief). If they share a brand-book URL or attached file, import it with `import_media_file` so it is stored in Zynta.
   - Populate only verified fields (`name`, `primary_language`, `nicho`, `publico_alvo`, `tom_de_voz`, hex colors, fonts, `profile_name`, `profile_handle`, `products`, `personas`). Leave unknown fields blank rather than guessing.
2. **Edit an existing brand kit (`update_brand_guide`):**
   - Always call `get_brand_guide` first.
   - Scalar fields omitted from `update_brand_guide` stay unchanged, **but `products` (max 25) and `personas` (max 6) replace the whole list**. Preserve existing entries and persona `id` values unless the user explicitly asked to remove them.
   - Summarize what changed after saving.

## Workflow B: Update a brand's Link in Bio

1. Call `list_brand_guides` to identify the `brand_guide_id`.
2. Call `get_link_in_bio` to inspect the current `zyntaflow.com/bio/<handle>` address and existing pages/links.
3. If the brand does not yet have a handle (or the user wants to change it), call `set_link_in_bio_address`.
4. Call `update_link_in_bio_page` to save title, intro, and links.
   - **`links` replaces the entire list in order.** Keep all existing links the user did not ask to remove.
   - Changes go live immediately — return the public URL and remind the user to place it in their Instagram bio if not already set.

## Workflow C: Analyze Instagram performance

1. Call `list_brand_guides` and `list_social_connections` to confirm the brand and connected accounts.
2. Call `get_analytics` for the requested period (default: last 30 days for a performance summary, last 90 days for pattern analysis).
3. Report real numbers clearly:
   - Reach, engagement, follower movement, top-performing posts, and best posting times.
   - Note what the top posts have in common (format, topic, hook, posting window).
   - If the user asks for next steps, propose 3 follow-up post ideas matching the winning pattern and ask before adding anything to the calendar.

## Workflow D: Account, billing, or setup questions

Whenever the user asks how or where to buy credits, switch plans, view invoices, connect a social network, invite team members, or manage MCP/API tokens, call `get_zynta_help` with the matching `topic` or raw `question` (and `brand_guide_id` if relevant). Return the exact in-app links and account status instead of guessing menu names.
