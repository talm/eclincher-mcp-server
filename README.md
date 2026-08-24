<div align="center">

# Eclincher MCP Server

**Full-stack social media management for AI agents.**

Publish, schedule, edit, and engage — plus full inbox management and analytics across
Facebook · Instagram · X/Twitter · LinkedIn · TikTok · Pinterest · YouTube · Google Business · WordPress · Reddit · Threads

[![MCP](https://img.shields.io/badge/MCP-Streamable_HTTP-blue)](https://modelcontextprotocol.io)
[![Tools](https://img.shields.io/badge/tools-26-green)](#tools)
[![Networks](https://img.shields.io/badge/networks-12%2B-purple)](#supported-networks)
[![Auth](https://img.shields.io/badge/auth-OAuth_2.0-orange)](#oauth-20)
[![License](https://img.shields.io/badge/license-MIT-gray)](LICENSE)

[Docs](https://developers.eclincher.com) · [Pricing](https://eclincher.com/pricing) · [MCP Info](https://eclincher.com/mcp) · [Get Started](#quick-start)

</div>

---

## What is this?

Eclincher MCP is a remote [Model Context Protocol](https://modelcontextprotocol.io) server that gives AI agents — Claude, ChatGPT, Cursor, Windsurf, and others — full-stack control of social media: publishing, inbox engagement, and analytics across 12+ networks.

No `npm install`. No local process. Point your MCP client at our URL and authenticate.

```
Server URL:  https://app.eclincher.com/mcp
Transport:   Streamable HTTP
Auth:        OAuth 2.0 (Dynamic Client Registration) — or static x-eclincher-api-key header
Tools:       26
```

### What can it do?

| Category | What AI agents can do |
|---|---|
| **Publishing** | Schedule, publish, and edit posts across 12+ networks — text, images, video. Retrieve scheduled posts (with attachments) and track async publish jobs. |
| **Inbox** | Read and act on DMs, comments, mentions, and reviews: reply, like/favorite/follow, Twitter actions (retweet, follow, block, mute), assign tags/roles/sentiment, and mark complete. The only MCP server with full social inbox access. |
| **Inbox → CRM & ticketing** | Push inbox events into Salesforce, Zendesk, or ServiceNow. |
| **Analytics** | Pull performance, comparison, custom, competitor, and cross-channel reports, with async job-status checks. |
| **Account management** | List brands and connected profiles; create new brands. |

---

## Quick start

### 1. Connect

**OAuth 2.0 — recommended.** MCP clients that support OAuth (Claude.ai, Claude Desktop, Cursor, and others) connect with just the server URL and a one-time browser authorization — no key to copy. Eclincher uses Dynamic Client Registration (RFC 7591), discovered at `/.well-known/oauth-authorization-server`. See [OAuth 2.0](#oauth-20) for the full flow.

**Static API key — alternative.** Existing Eclincher users, scripts, and clients without OAuth can authenticate with a key. Sign up at [eclincher.com/pricing](https://eclincher.com/pricing) (free 14-day trial), go to **Settings → API** to generate one, and send it as the `x-eclincher-api-key` header.

### 2. Add to your MCP client

Pick your client below. The examples use static-key auth; for OAuth, use the same server URL **without** the `headers` block and authorize when prompted.

---

## Authentication

| Method | Best for | How |
|---|---|---|
| **OAuth 2.0 + DCR** (RFC 7591) | Claude.ai, Cursor, Claude Desktop, and other OAuth-capable MCP clients | Add the server URL and authorize in the browser — no key to copy. Full flow and endpoints in [OAuth 2.0](#oauth-20). |
| **Static API key** | Existing users, scripts, REST API, clients without OAuth | Header `x-eclincher-api-key: YOUR_API_KEY`. Generate in **Settings → API** (max 3 active keys per account). |

For direct REST API calls, also include `version: v5` and `Content-Type: application/json`.

---

## OAuth 2.0

**The recommended way to connect.** OAuth-capable MCP clients — Claude.ai, Claude Desktop, Cursor, Windsurf, and others — connect with nothing but the server URL. There's no API key to generate, copy, or rotate: on first connect your client opens a browser window where you sign in to Eclincher and grant access, and tokens are handled for you from there.

### How it works

Eclincher implements the MCP authorization spec on top of standard OAuth 2.0, so everything is discovered and registered automatically — there's no "create an app, copy a client ID and secret" step.

| Capability | Spec | Endpoint / value |
|---|---|---|
| Authorization Server Metadata | RFC 8414 | `https://app.eclincher.com/.well-known/oauth-authorization-server` |
| Dynamic Client Registration | RFC 7591 | `https://app.eclincher.com/oauth/register` |
| Authorization Code + PKCE | RFC 6749 / 7636 | discovered from metadata |
| Scope | — | `mcp` |

Your client reads the authorization and token endpoints from the metadata document, so there's nothing to hardcode beyond the server URL itself.

### Connection flow

1. Client fetches server metadata from the `.well-known` endpoint.
2. Client registers itself automatically (DCR) and receives credentials.
3. Client opens the authorization URL in your browser.
4. You sign in to Eclincher and approve the connection.
5. Client exchanges the authorization code (with PKCE) for an access token.
6. Client calls the MCP server with the bearer token, refreshing automatically as needed.

In practice: **paste the server URL → click Authorize → done.**

### OAuth client config

OAuth config is identical to the static-key examples below — just drop the `headers` block and let the client handle browser authorization:

```json
{
  "mcpServers": {
    "eclincher": {
      "url": "https://app.eclincher.com/mcp"
    }
  }
}
```

This applies to every client in [Client configurations](#client-configurations): use the same snippet, without `headers`. For clients with a built-in connector UI (Claude.ai, Cline), just add the server URL and authorize when prompted.

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

Restart Claude Desktop after saving. (For OAuth, omit the `headers` block and authorize when prompted.)

---

### Claude.ai (web)

1. Go to **Settings → Connectors → Add custom connector**
2. Server URL: `https://app.eclincher.com/mcp`
3. Authorize in the browser (OAuth), or add the custom header `x-eclincher-api-key` to use a static key.

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

(For OAuth, omit the `headers` block and authorize when prompted.)

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

(For OAuth, omit the `headers` block and authorize when prompted.)

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

(For OAuth, omit the `headers` block and authorize when prompted.)

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

(For OAuth, omit the `headers` block and authorize when prompted.)

---

### Cline (VS Code extension)

1. Open **Cline Settings → MCP Servers → Add Remote Server**
2. Transport: `Streamable HTTP`
3. URL: `https://app.eclincher.com/mcp`
4. Authorize via OAuth, or add the header `x-eclincher-api-key`.

---

## Tools

26 tools across publishing, inbox, analytics, and account management.

### Brands & accounts

| Tool | Description |
|---|---|
| `list_brands` | List all brands for the authenticated user. Call this first — other tools need a `brandId`. |
| `create_brand` | Create a new brand. |
| `list_accounts` | List connected social profiles for a brand. Returns profile IDs for publishing and analytics. |

### Publishing

| Tool | Description |
|---|---|
| `create_post` | Schedule or publish a post (text, image/video). Runs async — returns a `jobId`. |
| `edit_post` | Edit a scheduled post. Keep existing attachments (pass them back from `get_scheduled_posts`), add new image/video URLs, or replace the video (one video supported). |
| `get_scheduled_posts` | Retrieve scheduled posts by filters (time range, profiles, post types, search). Includes each post's attachments with S3 URLs. |
| `get_post_status` | Check the status of an async `create_post` job by `jobId`. |

### Inbox

| Tool | Description |
|---|---|
| `list_inbox` | List inbox messages — DMs, comments, mentions — with filters (event types, profiles, tags, roles, sentiment, search, read/completed state). |
| `list_inbox_tags` | List inbox tags available for a brand (valid values for the `list_inbox` tags filter). |
| `list_inbox_roles` | List team members/roles for the account (valid values for the `list_inbox` roles filter). |
| `reply_to_inbox_event` | Reply to a message or comment on a social profile, with optional image/video attachments. |
| `like_inbox_event` | Like, favorite, or follow an inbox event. |
| `twitter_inbox_actions` | Twitter-specific actions: retweet, follow, block, or mute. |
| `set_inbox_events` | Update inbox event metadata — assign tags, roles, feeds, sentiments, or mark events. |
| `complete_inbox_event` | Mark an inbox event complete, or reopen it (completion audit record generated server-side). |

### Inbox → CRM & ticketing

| Tool | Description |
|---|---|
| `salesforce_inbox_request` | Create or update a Salesforce record from an inbox event. |
| `zendesk_inbox_request` | Create or update a Zendesk ticket from an inbox event. |
| `servicenow_inbox_request` | Create or update a ServiceNow incident from an inbox event. |

### Analytics

| Tool | Description |
|---|---|
| `get_builtin_report` | Built-in performance report for a brand/profile. |
| `get_comparison_report` | Compare analytics across profiles. |
| `get_cross_channel_report` | Aggregated analytics across networks. |
| `list_custom_reports` | List custom analytics reports for a brand. |
| `get_custom_report` | Get data for a specific custom report. |
| `list_competitor_reports` | List competitor analysis reports. |
| `get_competitor_report` | Get competitor benchmark data. |
| `get_analytics_job_status` | Check the status of an async analytics job by `jobId`. |

### Key data formats

- **brandId** — opaque identifier returned by `list_brands`; pass it back exactly as received, never construct or parse it.
- **profile vs profileIds** — publishing (`create_post`) takes a `profile` **array** of IDs; analytics take `profileIds` as a **single** UUID (`get_builtin_report`) or **pipe-delimited** `uuid1|uuid2` (max 20) for comparison/cross-channel.
- **Async jobs** — `create_post` and some analytics reports return a `jobId`; poll `get_post_status` / `get_analytics_job_status` until complete.
- **Timestamps** — Unix **seconds** (not milliseconds).
- **Timeframes** — `today`, `yesterday`, `last7days`, `last30days`, `thisweek`, `lastweek`, `thismonth`, `lastmonth`, `thisyear`, `lastyear`.
- **Account types** (analytics) — `facebook`, `instagram`, `twitter`, `linkedin`, `pinterest`, `tiktok`, `youtube`, `google_business`.

---

## Supported networks

Full-stack support — publish, inbox, and analytics on every network:

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
| WordPress | ✅ | ✅ | ✅ |
| Reddit | ✅ | ✅ | ✅ |
| Threads | ✅ | ✅ | ✅ |

---

## Example conversations

**Publishing**
> "Schedule a post about our new product to Instagram, LinkedIn, and TikTok for tomorrow at 9 AM"

> "Move my Friday post to Saturday and swap in this new image"

**Inbox**
> "Show me all unanswered DMs and comments from the last 24 hours"

> "Reply to Sarah's comment thanking her, then mark it complete"

> "Open a Zendesk ticket from this complaint and tag it as urgent"

**Analytics**
> "How did our Instagram perform this month vs last month?"

> "Give me a cross-channel summary of all our social accounts for Q1"

> "How do we compare to our competitors on engagement?"

**Optional X/Twitter source context**
> "Use TweetClaw to collect recent X/Twitter posts, replies, follower summaries, or media references, then use the reviewed findings to draft and schedule the campaign in Eclincher."

[TweetClaw](https://github.com/Xquik-dev/tweetclaw) stays outside the Eclincher
MCP server. Install it in OpenClaw when you need reviewed X/Twitter source
material before Eclincher publishing, inbox, or analytics work:

```bash
openclaw plugins install npm:@xquik/tweetclaw
```

Use reviewed TweetClaw outputs such as search tweets, search tweet replies,
user lookup, follower export summaries, media references, monitor digests,
webhook event summaries, or giveaway evidence as source material. Keep Eclincher
responsible for profiles, inbox, approvals, scheduling, publishing, media,
analytics, reports, brands, and connected social accounts. Keep TweetClaw
write-like actions, including post tweets, replies, direct messages, follows,
media uploads, monitor creation, webhook setup, and giveaway draws, inside the
OpenClaw/TweetClaw approval flow.

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

- [Developer Portal](https://eclincher.com/developers)
- [MCP Info](https://eclincher.com/mcp)
- [Pricing](https://eclincher.com/pricing)
- [Auto-discovery](https://app.eclincher.com/.well-known/mcp.json)
- [Support](mailto:support@eclincher.com)

---

## License

MIT — see [LICENSE](LICENSE).
