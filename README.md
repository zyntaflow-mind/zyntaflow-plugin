# Zynta Flow Plugin for Antigravity & Gemini CLI

Official plugin connecting **Google Antigravity** and **Gemini CLI** to [Zynta Flow](https://www.zyntaflow.com) via its remote Model Context Protocol (MCP) server (`https://app.zyntaflow.com/mcp`).

Create on-brand social media carousels, plan multi-week content calendars, write Reel scripts, build Instagram comment-to-DM automations, manage link-in-bio pages, and analyze post performance — directly from your agent conversation.

## Installation

### Google Antigravity

Install from the **Antigravity Marketplace** (`Customizations > Marketplace` → search **Zynta Flow**), or install directly from GitHub via the Antigravity CLI:

```bash
agy plugin install https://github.com/zyntaflow-mind/zyntaflow-plugin
```

### Gemini CLI

Install directly as a Gemini CLI extension:

```bash
gemini extensions install https://github.com/zyntaflow-mind/zyntaflow-plugin
```

## Authentication

No API key or manual token setup is required for interactive use:

1. Install and enable the plugin.
2. On your first Zynta Flow request, your client opens Zynta Flow's **OAuth 2.1 (PKCE S256)** consent screen.
3. Sign in with your Zynta Flow account and click **Approve**.
4. Revoke access at any time in Zynta Flow under **Settings → MCP & API** (`https://app.zyntaflow.com/settings/api`).

For headless scripts or environments without browser OAuth, you can create a Personal Access Token (`zfp_...`) in **Settings → MCP & API** and pass it as `Authorization: Bearer <token>`.

## What's Included

### Remote MCP Server
- **Endpoint:** `https://app.zyntaflow.com/mcp` (Streamable HTTP, stateless)
- **Auth:** OAuth 2.1 with PKCE (S256) and Dynamic Client Registration (RFC 7591)
- **Availability:** Included on every Zynta Flow plan, including the free trial. Reading, organizing, and simple edits are free; AI generation and publishing use the same credits as the web app.

### Bundled Skills
| Skill | What it covers |
| --- | --- |
| `create-carousel` | End-to-end on-brand carousel generation, slide editing, image attachment, layout tweaks, captions, and safe draft saving |
| `editorial-planning` | Brand idea board, Reel scripts, multi-week editorial plans, cursor-paginated calendar inspection, and publication troubleshooting |
| `instagram-automations` | Instagram comment-to-DM, story reply/mention, and multi-step visual automation flows with scope and safety guardrails |
| `brand-kit-analytics` | Brand kit setup/updates, live link-in-bio editing, Instagram performance analytics, and in-app help links |

### Tool Coverage
- **Brand Kits:** `list_brand_guides`, `get_brand_guide`, `create_brand_guide`, `update_brand_guide`
- **Carousels & Media:** `list_presets`, `generate_carousel`, `list_carousels`, `list_drafts`, `get_carousel`, `update_carousel`, `attach_unsplash_images`, `attach_slide_image`, `adjust_slide_layout`, `create_image_post`, `attach_post_image`, `create_media_upload`, `import_media_file`
- **Ideas, Scripts & Editorial Planning:** `list_content_ideas`, `get_content_idea`, `generate_content_ideas`, `update_content_idea`, `generate_idea_script`, `revise_idea_script`, `plan_content_idea`, `generate_content_plan`, `get_content_action_costs`
- **Calendar & Publishing:** `list_calendar_events`, `get_calendar_event`, `create_calendar_event`, `update_calendar_event`, `set_caption`, `publish_calendar_event`, `retry_calendar_publication`
- **Instagram Automations:** `list_automation_flows`, `get_automation_flow`, `create_automation_flow`, `update_automation_flow`, `delete_automation_flow`, `list_automations`, `create_automation_rule`, `update_automation_rule`, `delete_automation_rule`
- **Analytics, Connections & Link in Bio:** `list_social_connections`, `get_analytics`, `get_link_in_bio`, `set_link_in_bio_address`, `update_link_in_bio_page`
- **Help & Account Links:** `get_zynta_help`

## Example Prompts

- *"List my brands in Zynta Flow and show which social networks are connected to each."*
- *"Create an on-brand carousel draft about our latest product launch and save it to my calendar."*
- *"Plan next week's Instagram and LinkedIn posts for my brand after showing credit costs."*
- *"Build an Instagram automation that replies to comments with 'GUIDE' and sends my PDF via DM."*
- *"Analyze my top-performing Instagram posts from the last 90 days and suggest three follow-ups."*

## Links

- **MCP Setup Guide:** [https://www.zyntaflow.com/mcp](https://www.zyntaflow.com/mcp)
- **Website:** [https://www.zyntaflow.com](https://www.zyntaflow.com)
- **Privacy Policy:** [https://www.zyntaflow.com/privacy](https://www.zyntaflow.com/privacy)
- **Terms of Service:** [https://www.zyntaflow.com/terms](https://www.zyntaflow.com/terms)
- **Support:** [contato@zyntaflow.com](mailto:contato@zyntaflow.com)

## License

MIT — see [LICENSE](./LICENSE).
