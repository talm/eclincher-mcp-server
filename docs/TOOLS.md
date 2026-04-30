
# Tools Reference

## Typical workflow

1. Call `list_brands` to get your brand IDs
2. Call `list_accounts` with a brand ID to get profile IDs
3. Use profile IDs for publishing, inbox, or analytics

---

## list_brands

List all brands you have access to. Always call this first.

**Parameters**: None

**Returns**: Array of `{ brandName, brandId }`.
Example brandId: `Eclient$@5RtY?076-Ag&2^38-4(a05-9ZZz!~6d%3-5fs@G^^&^44556aT#6`

---

## create_brand

| Parameter | Type | Required | Description |
|---|---|---|---|
| brandName | string | Yes | Name for the new brand (1–200 chars) |

---

## list_accounts

| Parameter | Type | Required | Description |
|---|---|---|---|
| brandId | string | Yes | Brand ID from `list_brands` |

---

## create_post

| Parameter | Type | Required | Description |
|---|---|---|---|
| brandId | string | Yes | Brand ID |
| text | string | Yes | Post text (max 10,000 chars) |
| profile | string[] | Yes | Array of profile IDs to publish to |
| scheduleTimes | number[] | No | Unix timestamps in seconds. Omit to post now. |
| image | string | No | Image URL (https) |
| video | string | No | Video URL (https) |

---

## list_inbox

| Parameter | Type | Required | Description |
|---|---|---|---|
| brandId | string | Yes | Brand ID |
| startTime | number | Yes | Range start — unix seconds |
| endTime | number | Yes | Range end — unix seconds |
| nextId | string | No | Pagination cursor from previous response |

Constraints: `endTime` must be after `startTime`. Max 90-day range.

---

## get_builtin_report

| Parameter | Type | Required | Description |
|---|---|---|---|
| brandId | string | Yes | Brand ID |
| profileIds | string | Yes | Single profile UUID |
| account_type | string | Yes | facebook, instagram, twitter, linkedin, pinterest, tiktok, youtube, google_business |
| timeframe | string | Yes | today, yesterday, last7days, last30days, thisweek, lastweek, thismonth, lastmonth, thisyear, lastyear |
| reportType | string | No | Default: `performance` |

---

## get_comparison_report

| Parameter | Type | Required | Description |
|---|---|---|---|
| brandId | string | Yes | Brand ID |
| profileIds | string | Yes | Pipe-delimited: `uuid1\|uuid2` (max 20) |
| timeframe | string | Yes | Timeframe |

---

## list_custom_reports

| Parameter | Type | Required | Description |
|---|---|---|---|
| brandId | string | Yes | Brand ID |

---

## get_custom_report

| Parameter | Type | Required | Description |
|---|---|---|---|
| brandId | string | Yes | Brand ID |
| reportId | string | Yes | Report UUID from `list_custom_reports` |

---

## list_competitor_reports

| Parameter | Type | Required | Description |
|---|---|---|---|
| brandId | string | Yes | Brand ID |

---

## get_competitor_report

| Parameter | Type | Required | Description |
|---|---|---|---|
| brandId | string | Yes | Brand ID |
| reportId | string | Yes | Report UUID from `list_competitor_reports` |
| timeframe | string | Yes | Timeframe |

---

## get_cross_channel_report

| Parameter | Type | Required | Description |
|---|---|---|---|
| brandId | string | Yes | Brand ID |
| profileIds | string | Yes | Pipe-delimited UUIDs (max 20) |
| timeframe | string | Yes | Timeframe |

