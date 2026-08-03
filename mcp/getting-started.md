# Connecting a client to the IDC MCP server

The IDC MCP server is public and unauthenticated at **`https://api.imaging.datacommons.cancer.gov/mcp`**. Point any spec-conformant MCP client at that URL — there is no API key, config file, or account to set up.

## Connect a client

### claude.ai / Claude Desktop (remote connector)

Add the URL as a custom (remote) connector in Claude's connector settings:

```
https://api.imaging.datacommons.cancer.gov/mcp
```

Then ask, for example: *"Find breast MRI in IDC, show the counts and total size, and give me a download command."* The agent will discover valid filter values, size the cohort, and return the download commands on its own.

### Claude Desktop / Claude Code (local, for developers)

If you are developing against the server, you can run it locally over stdio. Add to your MCP client config:

```json
{
  "mcpServers": {
    "idc": {
      "command": "uv",
      "args": ["run", "--directory", "/absolute/path/to/IDC-REST-MCP", "idc-mcp"]
    }
  }
}
```

See the [IDC-REST-MCP repository](https://github.com/ImagingDataCommons/IDC-REST-MCP) for install instructions.

### Other MCP clients

Any spec-conformant remote-MCP client works — point it at `https://api.imaging.datacommons.cancer.gov/mcp`. Inspect or debug the tools with the [MCP Inspector](https://github.com/modelcontextprotocol/inspector):

```bash
npx @modelcontextprotocol/inspector
```

## How the hosted transport behaves

The hosted service uses **streamable-HTTP, configured stateless with plain-JSON responses** — each request is self-contained. In practice:

* **Any spec-conformant remote-MCP client works**, and the service autoscales behind a plain load balancer with no session affinity.
* **No session handshake is needed to script it** — you can `POST` a `tools/list` or `tools/call` directly (set `Accept: application/json, text/event-stream`); you don't have to `initialize` first or carry an `Mcp-Session-Id` header.
* **Session-bound MCP features are not available** (server→client sampling, elicitation, resource subscriptions, streamed progress) — the server exposes only client-initiated tools and static resources.

Once connected, see [Tools and resources](tools.md) for what the server can do, and the [Core concepts](../api/idc-api-concepts.md) page for the shared data model and recommended workflow. To compare this server with the [IDC agent skill](../agents/skill/README.md), see [Using IDC with an AI assistant](../agents/README.md).
