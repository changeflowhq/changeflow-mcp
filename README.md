# Changeflow MCP Server

Monitor any website and get AI-enriched change intelligence via MCP. Manage sources, search changes, and automate web monitoring from Claude, Harvey, or any MCP client.

**Requires a [Changeflow](https://changeflow.com) API token.** Get one from your [account settings](https://changeflow.com/account/api).

## What is Changeflow?

[Changeflow](https://changeflow.com) is an enterprise web intelligence platform. It monitors web pages, PDFs, RSS feeds, and APIs for changes, then uses AI to tell you what changed, why it matters, and what to do about it. Used by compliance teams, legal researchers, competitive intelligence analysts, and procurement teams at Fortune 500 companies and Am Law 200 firms.

## Quick Start

### Claude Desktop

Add to your `claude_desktop_config.json`:

```json
{
  "mcpServers": {
    "changeflow": {
      "command": "npx",
      "args": ["@changeflow/mcp-server"],
      "env": {
        "CHANGEFLOW_API_TOKEN": "your_api_token_here"
      }
    }
  }
}
```

Then ask Claude: *"List my Changeflow sources"* or *"Create a new source to monitor fda.gov/guidance"*

### Remote (Harvey, OpenAI, Claude Code)

Connect directly to the Streamable HTTP endpoint:

```
POST https://changeflow.com/mcp
Content-Type: application/json
Accept: application/json, text/event-stream
Authorization: Bearer your_api_token_here
```

Rate limit: 100 requests per hour.

## Tools

### list_sources

List all your monitored sources with status, tags, and check frequency.

**Parameters:**
- `tag` - Filter by tag
- `limit` - Max results (default 50, max 200)

### get_source

Get full details for a specific source by its share code.

**Parameters:**
- `source_id` (required) - Share code of the source

### get_changes

Search and list recent changes across your monitored sources.

**Parameters:**
- `query` - Keyword search across change content
- `since` - ISO 8601 date
- `type` - Filter by type: link (new pages found) or change (page content changed)
- `tag` - Filter by source tag
- `source_id` - Filter to a specific source
- `limit` - Max results (default 50, max 200)

### get_change

Get full details of a single change including AI summary and structured diff.

**Parameters:**
- `change_id` (required) - Change ID (v_123 for page changes, l_456 for link discoveries)

### create_source

Create a new source to monitor.

**Parameters:**
- `url` (required) - URL to monitor
- `looking_for` - What to watch for, in plain English
- `frequency` - Check frequency (e.g. "hourly", "daily", "every 6 hours")
- `tags` - Comma-separated tags

### check_source

Trigger an immediate check on a source.

**Parameters:**
- `source_id` (required) - Share code of the source

### pause_source / resume_source

Pause or resume monitoring on a source.

**Parameters:**
- `source_id` (required) - Share code of the source

## Getting Your API Token

1. Sign up at [changeflow.com](https://changeflow.com)
2. Go to [Account > API](https://changeflow.com/account/api)
3. Copy your API token
4. Set it as `CHANGEFLOW_API_TOKEN` environment variable

## Free Regulatory Intelligence

Looking for free, public regulatory data without authentication? Use the [GovPing MCP Server](https://github.com/stevebutterworth/govping-mcp) instead. It provides open access to 27,000+ structured regulatory changes.

## Links

- [Changeflow](https://changeflow.com) - Enterprise web intelligence platform
- [GovPing](https://govping.org) - Free regulatory intelligence (powered by Changeflow)
- [Pricing](https://changeflow.com/pricing) - Plans from $99/mo
- [API Documentation](https://changeflow.com/account/api)

## Rate Limits

- 100 requests per hour per API token
- Need higher limits? Email hello@changeflow.com

## License

MIT
