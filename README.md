# SourcetoMCP plugin

This plugin connects Claude to the marketing, SEO and analytics accounts you have linked in [SourcetoMCP](https://sourcetomcp.com). Claude can read reports and make changes in those accounts from a chat.

This repository holds the plugin configuration only. The SourcetoMCP service itself is closed source and runs at `https://sourcetomcp.com/api/mcp`.

## Supported sources

Google Ads, Google Search Console, Google Analytics, Google Sheets, Google Docs, Google Slides, TikTok Ads, Pinterest Ads, Reddit Ads, Bing Webmaster Tools, Ahrefs and PostHog.

LinkedIn Ads is built but limited to approved accounts for now.

## What is in this repo

| File | What it does |
| --- | --- |
| `.mcp.json` | Points Claude at the remote server over HTTP. It contains no keys or tokens. |
| `rules/sourcetomcp.md` | Instructions that tell the agent which tool to call for reads, writes and deletes. |

## Setup

1. Create a free trial account at [sourcetomcp.com](https://sourcetomcp.com) and connect your accounts from the dashboard.
2. Add the plugin to Claude.
3. Claude opens a browser window asking you to approve access to your SourcetoMCP account. Approve it and the tools appear.

## Tools

| Tool | Type | What it does |
| --- | --- | --- |
| `execute` | Read | Runs a read operation on a connected source. |
| `execute_write` | Write | Creates or updates data in a connected source. |
| `execute_delete` | Delete | Deletes data in a connected source. |
| `list_connections` | Read | Lists the accounts you have connected. |
| `describe_operations` | Read | Lists the operations available for a connected source. |
| `list_metrics` | Read | Lists the fields you can query across your connections. |
| `download_result` | Read | Fetches a large result saved to storage. |
| Named report tools | Read | Shortcuts for common reports, such as `get_search_performance` and `get_ads_performance`. |

The server enforces the split between reads, writes and deletes. Sending a write to `execute` is refused, and the error names the correct tool. Every tool carries MCP annotations (`readOnlyHint`, `destructiveHint`), so clients such as Claude ask you to confirm write and delete calls. The bundled rule also tells the agent to describe a change and wait for your yes first.

Ahrefs reads use API units from your own Ahrefs allowance. Those operations cap their row counts and report what each call spent.

## Security and data handling

- **Sign-in:** OAuth 2.1 authorization code flow with PKCE (S256) and refresh tokens. There is no API key to paste into Claude.
- **Stored credentials:** the refresh tokens for your connected accounts are stored encrypted in Supabase Vault. They are decrypted on the server only to make the calls you ask for.
- **Scope of access:** every call is checked against the account that owns the connection. The server does not trust a connection ID from the model alone.
- **Revoking access:** disconnect a source from your dashboard to remove the stored credentials and stop access to it.
- **Where data lives:** requests go from Claude to `sourcetomcp.com`, then on to the provider's own API. Large results are held in storage and deleted on a daily schedule.
- **Remote server changes:** the tool list is served by the remote server, so it can change without a plugin update. Changes to what the tools can do are listed in the [changelog](https://sourcetomcp.com/dashboard/changelog).

Full details are in the [privacy policy](https://sourcetomcp.com/privacy) and [terms](https://sourcetomcp.com/terms).

## Support

Email [support@sourcetomcp.com](mailto:support@sourcetomcp.com). To report a security problem, use the same address and put "security" in the subject line.

Documentation: [sourcetomcp.com/docs](https://sourcetomcp.com/docs)
