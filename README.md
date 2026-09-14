# mcp-jungle-scout

Jungle Scout MCP — Amazon sales estimates + product database + keyword data

Part of [Pipeworx](https://pipeworx.io) — an MCP gateway connecting AI agents to 1573+ live data sources.

## Tools

| Tool | Description |
|------|-------------|
| `junglescout_sales_estimate` | How many units does an Amazon ASIN sell — returns Jungle Scout estimated units sold, revenue, and price for a product ASIN. Example: junglescout_sales_estimate({ asin: "B08N5WRWNW", marketplace: "us", _apiKey: "KEY_NAME:API_KEY" }) |
| `junglescout_product_database` | Search Amazon products by keyword or category — returns matching ASINs with title, brand, price, rank, estimated units sold, reviews, and rating from the Jungle Scout product database. Example: junglescout_product_database({ keyword: "yoga mat", marketplace: "us", _apiKey: "KEY_NAME:API_KEY" }) |
| `junglescout_keyword_data` | Amazon keyword search volume — returns monthly search volume and keyword metrics for a keyword, or the top keywords an ASIN ranks for. Pass `keyword` OR `asin`. Example: junglescout_keyword_data({ keyword: "yoga mat", marketplace: "us", _apiKey: "KEY_NAME:API_KEY" }) |

## Quick Start

Add to your MCP client (Claude Desktop, Cursor, Windsurf, etc.):

```json
{
  "mcpServers": {
    "jungle-scout": {
      "url": "https://gateway.pipeworx.io/jungle-scout/mcp"
    }
  }
}
```

### What this endpoint actually serves

`tools/list` at `https://gateway.pipeworx.io/jungle-scout/mcp` returns the tools in the table
above **plus the shared Pipeworx meta-tools** — `ask_pipeworx`,
`discover_tools`, `search_within`, `remember`/`recall` and the rest of the
gateway-wide set. So the tool count you see is larger than this table: a
single-pack endpoint currently lists roughly 30 shared tools alongside the
pack's own. The connection's `initialize` response states its exact scope, and
is the authoritative answer for a given day.

This is deliberate, not multiplexing by accident. The meta-tools are what let a
scoped connection answer a question this pack does not cover — via
`ask_pipeworx`, which routes across the whole catalog — without you adding a
second MCP server. There is currently no way to mount a pack endpoint without
them; if the extra schemas cost you more context than the routing is worth,
connect to the full gateway once rather than to several pack endpoints.

Or connect to the full Pipeworx gateway to get every pack's tools listed
directly, instead of just this one's:

```json
{
  "mcpServers": {
    "pipeworx": {
      "url": "https://gateway.pipeworx.io/mcp"
    }
  }
}
```

Both URLs reach the same gateway and the same 1573+ data sources. The
only difference is which pack's tools are listed **directly**; `ask_pipeworx`
reaches all of them from either one.

## Standalone (no gateway account)

This package also runs as a local stdio MCP server — no Pipeworx account, no
gateway round-trip:

```json
{
  "mcpServers": {
    "jungle-scout": {
      "command": "npx",
      "args": ["-y", "@pipeworx/mcp-jungle-scout"]
    }
  }
}
```

Or run it directly to confirm it starts:

```bash
npx -y @pipeworx/mcp-jungle-scout
```

It speaks MCP over stdin/stdout and answers `initialize`/`tools/list`/`tools/call`
for **only** this pack's tools — none of the shared meta-tools the gateway
connection above adds. Same source, same tools, no ask_pipeworx routing.

## Using with ask_pipeworx

Instead of calling tools directly, you can ask questions in plain English —
this works on the pack endpoint above as well as on the full gateway:

```
ask_pipeworx({ question: "your question about Jungle Scout data" })
```

The gateway picks the right tool and fills the arguments automatically.

## More

- [Docs and guides](https://pipeworx.io/docs)
- [pipeworx.io](https://pipeworx.io)

## License

MIT
