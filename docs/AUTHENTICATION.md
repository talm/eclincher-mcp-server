
# Authentication

## Getting your credentials

1. Sign up at [eclincher.com/pricing](https://eclincher.com/pricing) — free 14-day trial
2. Connect your social media accounts in the Eclincher dashboard
3. Go to **Settings → API**
4. Click **Generate new API key**
5. Copy your API key

> **Important**: Save your API key immediately.

## Headers

All API and MCP requests use the same header:

```
x-eclincher-api-key: YOUR_API_KEY
```

For direct REST API calls, also include:

```
version: v5
Content-Type: application/json
```

### Full example (REST API)

```bash
curl -X POST https://app.eclincher.com/api/v5/brands/list \
  -H "x-eclincher-api-key: YOUR_API_KEY" \
  -H "version: v5" \
  -H "Content-Type: application/json"
```

### Full example (MCP client config)

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

## Plans with API access

| Plan | Price | Brands | Users | Profiles |
|---|---|---|---|---|
| Standard | $149/mo | 1 | 1 (max 2) | 15 (max 20) |
| Professional | $349/mo | Unlimited | 5 (max 10) | 25 (max 40) |
| Enterprise | Custom | Unlimited | Custom | Custom |

## Security

- Keys are scoped to your account — all brands and profiles
  are accessible with any key from your account
- Maximum 3 active keys per account
- Revoke a key to immediately disable all access

## Team access

All team members on your plan share API keys.

- **Standard**: Up to 2 users sharing one key
- **Professional**: Up to 10 users sharing one key
- **Enterprise**: Custom user count

All requests count against the same rate limits and credit pool.
