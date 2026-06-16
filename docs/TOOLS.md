# Tools Reference

26 tools across brands & accounts, publishing, inbox, CRM/ticketing, and analytics.

## Typical workflow

1. Call `list_brands` to get your brand IDs
2. Call `list_accounts` with a brand ID to get profile IDs
3. Use profile IDs for publishing, inbox, or analytics

> **Async tools:** `create_post` and some analytics reports run asynchronously and return a `jobId` — poll `get_post_status` / `get_analytics_job_status` until they complete.

> **Confirmed vs. derived:** brands, accounts, core publishing, inbox listing, and the analytics reports are confirmed. Parameters for the newer engagement, CRM/ticketing, scheduled-post, and job-status tools are documented from their MCP descriptions — verify exact field names against the live tool `inputSchema` before relying on them.

---

## Brands & accounts

### list_brands

List all brands you have access to. Always call this first.

**Parameters**: None

**Returns**: Array of `{ brandName, brandId }`.
Example brandId: `Eclient$@5RtY?076-Ag&2^38-4(a05-9ZZz!~6d%3-5fs@G^^&^44556aT#6`

### create_brand

| Parameter | Type | Required | Description |
|---|---|---|---|
| brandName | string | Yes | Name for the new brand (1–200 chars) |

### list_accounts

| Parameter | Type | Required | Description |
|---|---|---|---|
| brandId | string | Yes | Brand ID from `list_brands` |

---

## Publishing

### create_post

| Parameter | Type | Required | Description |
|---|---|---|---|
| brandId | string | Yes | Brand ID |
| text | string | Yes | Post text (max 10,000 chars) |
| profile | string[] | Yes | Array of profile IDs to publish to |
| scheduleTimes | number[] | No | Unix timestamps in seconds. Omit to post now. |
| image | string | No | Image URL (https) |
| video | string | No | Video URL (https) |

**Returns**: `{ jobId }` — runs asynchronously; poll `get_post_status` with the `jobId` to confirm the post published.

### get_scheduled_posts

| Parameter | Type | Required | Description |
|---|---|---|---|
| brandId | string | Yes | Brand ID |
| startTime | number | No | Range start — unix seconds |
| endTime | number | No | Range end — unix seconds |
| profile | string[] | No | Filter by profile IDs |
| postTypes | string[] | No | Filter by post type |
| search | string | No | Text search across post content |
| nextId | string | No | Pagination cursor |

**Returns**: posts with their attachments (S3 URLs). Pass attachments back into `edit_post` to keep them.

### edit_post

| Parameter | Type | Required | Description |
|---|---|---|---|
| brandId | string | Yes | Brand ID |
| postId | string | Yes | ID of the scheduled post (from `get_scheduled_posts`) |
| text | string | No | Updated post text |
| profile | string[] | No | Updated target profile IDs |
| scheduleTimes | number[] | No | Unix seconds |
| image | string | No | New image URL (https) |
| video | string | No | New video URL (https) — replaces existing (one video supported) |
| attachments | object[] | No | Existing attachments to keep — pass back from `get_scheduled_posts` |

### get_post_status

| Parameter | Type | Required | Description |
|---|---|---|---|
| brandId | string | Yes | Brand ID |
| jobId | string | Yes | Job ID returned by `create_post` |

---

## Inbox

### list_inbox

| Parameter | Type | Required | Description |
|---|---|---|---|
| brandId | string | Yes | Brand ID |
| startTime | number | Yes | Range start — unix seconds |
| endTime | number | Yes | Range end — unix seconds |
| nextId | string | No | Pagination cursor from previous response |
| eventTypes | string[] | No | Filter by event type (DMs, comments, mentions) |
| profile | string[] | No | Filter by profile IDs |
| tags | string[] | No | Filter by tag (values from `list_inbox_tags`) |
| roles | string[] | No | Filter by assigned role (values from `list_inbox_roles`) |
| sentiment | string | No | Filter by sentiment |
| search | string | No | Text search |
| completed | boolean | No | Filter by completed state |
| read | boolean | No | Filter by read state |

Constraints: `endTime` must be after `startTime`. Max 90-day range. (Filters beyond the first four come from the latest tool description — verify names.)

### list_inbox_tags

| Parameter | Type | Required | Description |
|---|---|---|---|
| brandId | string | Yes | Brand ID |

**Returns**: tags valid for the `list_inbox` and `set_inbox_events` `tags` field.

### list_inbox_roles

| Parameter | Type | Required | Description |
|---|---|---|---|
| brandId | string | Yes | Brand ID |

**Returns**: team members/roles valid for the `list_inbox` and `set_inbox_events` `roles` field.

### reply_to_inbox_event

| Parameter | Type | Required | Description |
|---|---|---|---|
| brandId | string | Yes | Brand ID |
| eventId | string | Yes | Inbox event to reply to |
| text | string | Yes | Reply text |
| image | string | No | Image URL (https) |
| video | string | No | Video URL (https) |

### like_inbox_event

| Parameter | Type | Required | Description |
|---|---|---|---|
| brandId | string | Yes | Brand ID |
| eventId | string | Yes | Inbox event to like, favorite, or follow |

### twitter_inbox_actions

| Parameter | Type | Required | Description |
|---|---|---|---|
| brandId | string | Yes | Brand ID |
| eventId | string | Yes | Inbox event |
| action | string | Yes | `retweet`, `follow`, `block`, or `mute` |

### set_inbox_events

| Parameter | Type | Required | Description |
|---|---|---|---|
| brandId | string | Yes | Brand ID |
| eventIds | string[] | Yes | Inbox events to update |
| tags | string[] | No | Tag values (from `list_inbox_tags`) |
| roles | string[] | No | Role values (from `list_inbox_roles`) |
| feeds | string[] | No | Feed IDs |
| sentiment | string | No | e.g. `positive`, `negative`, `neutral` |

### complete_inbox_event

| Parameter | Type | Required | Description |
|---|---|---|---|
| brandId | string | Yes | Brand ID |
| eventId | string | Yes | Inbox event |
| completed | boolean | Yes | `true` to complete, `false` to reopen |

The completion audit record (who/when) is generated server-side.

---

## Inbox → CRM & ticketing

*Beyond `brandId` and `eventId`, the record/ticket/incident fields depend on your connected integration's configuration — confirm against your server.*

### salesforce_inbox_request

| Parameter | Type | Required | Description |
|---|---|---|---|
| brandId | string | Yes | Brand ID |
| eventId | string | Yes | Inbox event to create/update a Salesforce record from |

### zendesk_inbox_request

| Parameter | Type | Required | Description |
|---|---|---|---|
| brandId | string | Yes | Brand ID |
| eventId | string | Yes | Inbox event to create/update a Zendesk ticket from |

### servicenow_inbox_request

| Parameter | Type | Required | Description |
|---|---|---|---|
| brandId | string | Yes | Brand ID |
| eventId | string | Yes | Inbox event to create/update a ServiceNow incident from |

---

## Analytics

### get_builtin_report

| Parameter | Type | Required | Description |
|---|---|---|---|
| brandId | string | Yes | Brand ID |
| profileIds | string | Yes | Single profile UUID |
| account_type | string | Yes | facebook, instagram, twitter, linkedin, pinterest, tiktok, youtube, google_business |
| timeframe | string | Yes | today, yesterday, last7days, last30days, thisweek, lastweek, thismonth, lastmonth, thisyear, lastyear |
| reportType | string | No | Default: `performance` |

*Note: `account_type` lists the 8 confirmed networks. If WordPress/Reddit/Threads analytics are live (per the README grid), add their values here.*

### get_comparison_report

| Parameter | Type | Required | Description |
|---|---|---|---|
| brandId | string | Yes | Brand ID |
| profileIds | string | Yes | Pipe-delimited: `uuid1\|uuid2` (max 20) |
| timeframe | string | Yes | Timeframe |

### get_cross_channel_report

| Parameter | Type | Required | Description |
|---|---|---|---|
| brandId | string | Yes | Brand ID |
| profileIds | string | Yes | Pipe-delimited UUIDs (max 20) |
| timeframe | string | Yes | Timeframe |

### list_custom_reports

| Parameter | Type | Required | Description |
|---|---|---|---|
| brandId | string | Yes | Brand ID |

### get_custom_report

| Parameter | Type | Required | Description |
|---|---|---|---|
| brandId | string | Yes | Brand ID |
| reportId | string | Yes | Report UUID from `list_custom_reports` |

### list_competitor_reports

| Parameter | Type | Required | Description |
|---|---|---|---|
| brandId | string | Yes | Brand ID |

### get_competitor_report

| Parameter | Type | Required | Description |
|---|---|---|---|
| brandId | string | Yes | Brand ID |
| reportId | string | Yes | Report UUID from `list_competitor_reports` |
| timeframe | string | Yes | Timeframe |

### get_analytics_job_status

| Parameter | Type | Required | Description |
|---|---|---|---|
| brandId | string | Yes | Brand ID |
| jobId | string | Yes | Job ID returned by an async analytics report |
