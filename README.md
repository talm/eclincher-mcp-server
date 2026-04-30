<div align="center">

# Eclincher MCP Server

**Full-stack social media management for AI agents.**

Publish posts, manage your inbox, and pull analytics across
Facebook · Instagram · X/Twitter · LinkedIn · TikTok · Pinterest · YouTube · Google Business

[![MCP](https://img.shields.io/badge/MCP-Streamable_HTTP-blue)](https://modelcontextprotocol.io)
[![Tools](https://img.shields.io/badge/tools-12-green)](#tools)
[![Networks](https://img.shields.io/badge/networks-8-purple)](#supported-networks)
[![License](https://img.shields.io/badge/license-MIT-gray)](LICENSE)

[Docs](https://developers.eclincher.com) · [Pricing](https://eclincher.com/pricing) · [MCP Info](https://eclincher.com/mcp) · [Get API Key](#quick-start)

</div>

---

## What is this?

Eclincher MCP is a remote [Model Context Protocol](https://modelcontextprotocol.io) server that gives AI agents — Claude, ChatGPT, Cursor, Windsurf, and others — the ability to manage social media accounts.

No `npm install`. No local process. Just point your MCP client to our URL and authenticate.

```
Server URL:  https://app.eclincher.com/mcp
Transport:   Streamable HTTP
Auth:        x-eclincher-api-key header
```

### What can it do?

| Category | What AI agents can do |
|---|---|
| **Publishing** | Schedule and publish posts to any combination of 8 social networks. Text, images, video. Immediate or scheduled. |
| **Inbox** | Read DMs, comments, and mentions across all connected platforms. Paginated, time-range filtered. The only MCP server that offers social inbox access. |
| **Analytics** | Pull 5 report types: performance, profile comparison, custom reports, competitor benchmarks, and cross-channel aggregated analytics. |
| **Account management** | List brands, connected profiles, and create new brands. |

---

## Quick start

### 1. Get your API key

Sign up at [eclincher.com/pricing](https://eclincher.com/pricing) (free 14-day trial), then go to **Settings → API** to generate your API key.

### 2. Add to your MCP client

Pick your client below, paste the config, and replace `YOUR_API_KEY` with your key.

---

## Client configurations

### Claude Desktop

Edit your config file:
- **macOS**: `~/Library/Application Support/Claude/claude_desktop_config.json`
- **Linux**: `~/.config/claude/claude_desktop_config.json`
- **Windows**: `%APPDATA%\Claude\claude_desktop_config.json`

```json
{
  "mcpServers": {
    "eclincher": {
      "url": "https://app.eclincher.com/mcp",
      "headers": {
        "x-eclincher-api-key": "YOUR_API_KEY"
      }
    }
  }
}
```

Restart Claude Desktop after saving.

---

### Claude.ai (web)

1. Go to **Settings → Integrations → Add MCP Server**
2. Server URL: `https://app.eclincher.com/mcp`
3. Add custom header: `x-eclincher-api-key`

---

### Cursor IDE

Create `.cursor/mcp.json` in your project root (or add to global settings):

```json
{
  "mcpServers": {
    "eclincher": {
      "url": "https://app.eclincher.com/mcp",
      "headers": {
        "x-eclincher-api-key": "YOUR_API_KEY"
      }
    }
  }
}
```

---

### Windsurf IDE

Go to **Settings → MCP Servers → Add Server**, or add to your config:

```json
{
  "eclincher": {
    "serverUrl": "https://app.eclincher.com/mcp",
    "headers": {
      "x-eclincher-api-key": "YOUR_API_KEY"
    }
  }
}
```

---

### VS Code (GitHub Copilot MCP)

Create `.vscode/mcp.json` in your project root:

```json
{
  "servers": {
    "eclincher": {
      "type": "http",
      "url": "https://app.eclincher.com/mcp",
      "headers": {
        "x-eclincher-api-key": "YOUR_API_KEY"
      }
    }
  }
}
```

---

### Continue.dev (VS Code / JetBrains)

Edit `~/.continue/config.json`:

```json
{
  "mcpServers": [
    {
      "name": "eclincher",
      "transport": {
        "type": "streamable-http",
        "url": "https://app.eclincher.com/mcp",
        "headers": {
          "x-eclincher-api-key": "YOUR_API_KEY"
        }
      }
    }
  ]
}
```

---

### Cline (VS Code extension)

1. Open **Cline Settings → MCP Servers → Add Remote Server**
2. Transport: `Streamable HTTP`
3. URL: `https://app.eclincher.com/mcp`
4. Add header: `x-eclincher-api-key`

---

## Tools

| Tool | Description |
|---|---|
| `list_brands` | List all brands you have access to. Call this first — you need a `brandId` for other tools. |
| `create_brand` | Create a new brand. |
| `list_accounts` | List connected social media profiles for a brand. Returns profile IDs for publishing and analytics. |
| `create_post` | Create or schedule a post. Supports text, image URL, video URL, and scheduled times. |
| `list_inbox` | List inbox messages — DMs, comments, and mentions. Time-range filtered, paginated. Max 90-day range. |
| `get_builtin_report` | Get performance analytics for a single social profile. |
| `get_comparison_report` | Compare analytics across multiple profiles. |
| `list_custom_reports` | List custom analytics reports for a brand. |
| `get_custom_report` | Get data for a specific custom report. |
| `list_competitor_reports` | List competitor analysis reports. |
| `get_competitor_report` | Get competitor benchmark data. |
| `get_cross_channel_report` | Get aggregated analytics across multiple profiles and networks. |

### Key data formats

- **brandId**: `ecagencyclient@<uuid>` format
- **profileIds** (analytics): Pipe-delimited UUIDs → `uuid1|uuid2|uuid3`
- **Timestamps**: Unix seconds (not milliseconds)
- **Timeframes**: `today`, `yesterday`, `last7days`, `last30days`, `thisweek`, `lastweek`, `thismonth`, `lastmonth`, `thisyear`, `lastyear`
- **Account types**: `facebook`, `instagram`, `twitter`, `linkedin`, `pinterest`, `tiktok`, `youtube`, `google_business`

---

## Supported networks

| Network | Publish | Inbox | Analytics |
|---|---|---|---|
| Facebook | ✅ | ✅ | ✅ |
| Instagram | ✅ | ✅ | ✅ |
| X / Twitter | ✅ | ✅ | ✅ |
| LinkedIn | ✅ | ✅ | ✅ |
| TikTok | ✅ | ✅ | ✅ |
| Pinterest | ✅ | ✅ | ✅ |
| YouTube | ✅ | ✅ | ✅ |
| Google Business | ✅ | ✅ | ✅ |

---

## Example conversations

**Publishing**
> "Schedule a post about our new product to Instagram, LinkedIn, and TikTok for tomorrow at 9 AM"

**Inbox**
> "Show me all DMs and comments from the last 24 hours"

**Analytics**
> "How did our Instagram perform this month vs last month?"

> "Give me a cross-channel summary of all our social accounts for Q1"

> "How do we compare to our competitors on engagement?"

---

## Pricing

API and MCP access is included with all Eclincher plans.

| Plan | Price | Brands | Users | Social profiles |
|---|---|---|---|---|
| Standard | $149/mo | 1 | 1 (max 2) | 15 (max 20) |
| Professional | $349/mo | Unlimited | 5 (max 10) | 25 (max 40) |
| Enterprise | Custom | Unlimited | Custom | Custom |

Free 14-day trial. Cancel anytime. [eclincher.com/pricing](https://eclincher.com/pricing)

---

## Rate limits

Per API key (shared across all team members).

| Dimension | Standard | Professional | Enterprise |
|---|---|---|---|
| Requests per minute | 30 | 90 | Custom |
| Daily credits | 5,000 | 15,000 | Custom |
| Concurrent requests | 2 | 10 | Custom |
| Posts per day | 50 | 200 | Custom |

Rate limit headers are included in every response.

---

## Links

- [Developer Portal](https://developers.eclincher.com)
- [MCP Info](https://eclincher.com/mcp)
- [Pricing](https://eclincher.com/pricing)
- [Auto-discovery](https://eclincher.com/.well-known/mcp.json)
- [Support](mailto:support@eclincher.com)

---

## License

MIT — see [LICENSE](LICENSE).
