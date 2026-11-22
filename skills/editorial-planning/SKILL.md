---
name: editorial-planning
description: >
  Plans social media content end-to-end in Zynta Flow — generates or searches brand content
  ideas, writes and refines Reel scripts, builds multi-week editorial plans, inspects calendar
  entries with cursor pagination, and diagnoses or retries failed publications. Use when the
  user asks to plan a week or month of posts, brainstorm ideas, write a Reel script, review
  their content calendar, or troubleshoot a failed post. Not for designing individual
  carousels (use create-carousel).
---

# Editorial Planning, Ideas & Calendar Diagnostics

Manage a brand's full content pipeline in Zynta Flow: idea board, Reel scripts, multi-week editorial plans, calendar inspection, and publication troubleshooting.

## Step 1: Resolve brand and quote costs first

1. Call `list_brand_guides`. If multiple brands exist and none was specified, ask which brand to plan for.
2. Before any paid batch action (`generate_content_ideas`, `generate_idea_script`, `generate_content_plan`, or `publish_calendar_event`), call `get_content_action_costs` for that `brand_guide_id`.
3. Present the unit price and payer balance to the user and wait for explicit approval before calling a paid tool.

## Step 2: Choose the right workflow

### A. Idea board and Reel scripts
- **Browse existing ideas:** call `list_content_ideas` (filter by `status`, `persona_id`, or `search`). Continue paginating with `next_offset` until `null` when the user wants a complete view.
- **Inspect one idea:** `get_content_idea` (returns brief, persona, and saved Reel script if any).
- **Generate new ideas:** after cost approval, call `generate_content_ideas` with a fresh UUID in `request_id`.
- **Edit or triage ideas:** `update_content_idea` (change title, angle, hook, or mark `new` / `used` / `dismissed`).
- **Write or revise a Reel script:**
  - `generate_idea_script` returns an existing script for free unless `regenerate: true` (regenerating is a new paid operation).
  - `revise_idea_script` swaps to hook variant `0` or `1` for free.
- **Promote an idea to the calendar:** `plan_content_idea` creates a non-publishing calendar draft (`scheduled_date` alone never publishes) and marks the idea `used` for free.

### B. Multi-week editorial plan
- Gather `posts_per_week` (1–7), `weeks` (1–12), `platform` (`instagram` or `linkedin`), optional `start_date`, `timezone`, `include_reels`, `persona_ids`, and `briefing`.
- Show the estimated cost from `get_content_action_costs` and wait for OK.
- Call `generate_content_plan` with a stable `request_id` UUID. Every generated item lands on the calendar as an idea/draft — nothing is scheduled or published.

### C. Calendar inspection and publication troubleshooting
- Call `list_calendar_events` with explicit filters (`brand_guide_id`, `status` including `publishing` and `failed`, `search`, `from_date`, `to_date`, `order`).
- **Always paginate with `cursor` (`next_cursor`)** using the exact same filters and `order` until `next_cursor` is `null` — one page is not the whole calendar.
- Inspect a specific item with `get_calendar_event` to read its linked carousel, caption, media, and publication diagnostics.
- **Publish or retry only on explicit user instruction:**
  - First check media, caption, brand approval requirements, and cost (`get_content_action_costs`).
  - Call `publish_calendar_event` (new send) or `retry_calendar_publication` (failed post after fixing the cause).
  - A `processing` response is acceptance, not proof of publication: re-read with `get_calendar_event` until a terminal state (`published` or `failed`) is reached.

## Guardrails

- **Always reuse the exact same `request_id` UUID and inputs on retries** after a timeout or network error on `generate_content_ideas`, `generate_idea_script`, `revise_idea_script`, `plan_content_idea`, `generate_content_plan`, `publish_calendar_event`, and `retry_calendar_publication`. Never invent a new UUID for a retry — that risks double billing or duplicate calendar entries.
- **Distinguish drafts from scheduled posts.** Entries in `idea`, `creating`, or `ready` never publish even when they carry a date. Only `scheduled` entries (with a connected social account) publish automatically.
- **Never republish an already-confirmed post.** Inspect `get_calendar_event` first; the backend refuses to resend confirmed publications.
