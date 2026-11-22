# Security Policy

## Reporting a Vulnerability

If you discover a security vulnerability in this plugin or in Zynta Flow's remote MCP server (`https://app.zyntaflow.com/mcp`), please report it privately instead of opening a public GitHub issue.

- **Contact:** [contato@zyntaflow.com](mailto:contato@zyntaflow.com)
- **Subject line:** `[Security] Zynta Flow MCP / Plugin`

Please include:
- A description of the vulnerability and its potential impact.
- Steps to reproduce or a proof of concept.
- Any relevant tool names, endpoints, or timestamps (avoid including live OAuth tokens or personal access tokens).

## Authentication & Data Handling

- This plugin contains no credentials, no local executable code, and no embedded API keys.
- All tool calls connect over HTTPS to `https://app.zyntaflow.com/mcp` using **OAuth 2.1 with PKCE (S256)** and Dynamic Client Registration (RFC 7591), or a user-managed Personal Access Token created at `https://app.zyntaflow.com/settings/api`.
- Every call is scoped to the authenticated account's brand permissions, rate-limited, and audited. Connections can be revoked immediately in **Settings > MCP & API**.
- Privacy Policy: [https://www.zyntaflow.com/privacy](https://www.zyntaflow.com/privacy)
