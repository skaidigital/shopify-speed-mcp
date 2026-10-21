# Shopify Speed Check MCP

A remote MCP server that checks how fast a Shopify store is and what it costs to run. Ask your AI
assistant about any store, and it gets:

- Google PageSpeed Insights scores for mobile and desktop
- real-user Core Web Vitals from the Chrome UX Report
- the apps and scripts slowing the store down
- the theme, or whether the store is headless
- a Shopify Basic, Grow, Advanced and Plus fee comparison at your sales volume

No API key or account is needed. The same checks run on the
[free Shopify speed test](https://www.skaidigital.com/tools/shopify-speed-test).

**Endpoint:** `https://tools.skaidigital.com/mcp` (streamable HTTP, no auth)

The server is hosted and closed-source. This repository holds only the client configuration and
directory listings.

## Tools

| Tool | What it does |
| --- | --- |
| `check_shopify_store_speed` | PageSpeed mobile and desktop scores, Core Web Vitals, the apps and scripts slowing the store, and an estimate of what a headless build could change. Takes 10 seconds to 2.5 minutes; results are cached for 24 hours. |
| `detect_shopify_store` | Whether a site is a Shopify theme store or a headless Shopify storefront, and which theme it uses. |
| `calculate_shopify_fees` | Monthly and annual cost of each Shopify plan for a sales volume and payment mix, with break-even sales between plans. Only `monthlyGmv` is required. |

Usage is capped per client and in total to keep the service free.

## Setup

### Cursor

Add to `~/.cursor/mcp.json`, or `.cursor/mcp.json` in a project:

```json
{
  "mcpServers": {
    "shopify-speed-check": {
      "url": "https://tools.skaidigital.com/mcp"
    }
  }
}
```

### Claude Desktop

Open **Settings → Connectors → Add custom connector** and enter
`https://tools.skaidigital.com/mcp`.

On plans without custom connectors, add this to `claude_desktop_config.json` instead (requires
Node.js):

```json
{
  "mcpServers": {
    "shopify-speed-check": {
      "command": "npx",
      "args": ["-y", "mcp-remote", "https://tools.skaidigital.com/mcp"]
    }
  }
}
```

### Claude Code

```sh
claude mcp add --transport http shopify-speed-check https://tools.skaidigital.com/mcp
```

### VS Code

Add to `.vscode/mcp.json`:

```json
{
  "servers": {
    "shopify-speed-check": {
      "type": "http",
      "url": "https://tools.skaidigital.com/mcp"
    }
  }
}
```

### Gemini CLI

```sh
gemini extensions install https://github.com/skaidigital/shopify-speed-mcp
```

## Example prompts

- "How fast is gymshark.com on mobile, and what's slowing it down?"
- "Is allbirds.com headless or on a Shopify theme?"
- "At $400k a month in online sales, is Shopify Plus cheaper than Advanced?"

## About

Built by [Skai Digital](https://www.skaidigital.com), a Shopify and headless commerce agency.

The configuration files in this repository are MIT licensed.
