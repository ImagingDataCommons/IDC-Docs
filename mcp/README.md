# The IDC MCP server

The IDC **MCP server** lets LLM agents query IDC directly. It exposes IDC's capabilities as [Model Context Protocol](https://modelcontextprotocol.io/) tools, so an agent such as Claude can find, describe, and download IDC data on your behalf, in plain conversation.

The [Model Context Protocol](https://modelcontextprotocol.io/) is an open standard for connecting LLM applications to external tools and data sources. Any spec-conformant MCP client can use the IDC server.

The server is live in production at:

```
https://api.imaging.datacommons.cancer.gov/mcp
```

It is public and **unauthenticated** — no API key, account, or config file needed. Point a client at the URL and ask it to find an imaging cohort.

{% hint style="info" %}
The MCP server and the REST API are released together as version 3, currently in **beta**. The contract may still change in response to feedback before the final `3.0.0` release. To see exactly which beta build is deployed, ask your agent to call the [`get_idc_version` tool](tools.md) (or call the REST API's [`GET /v3/version`](../api/endpoint-details.md)) — the `api_version` field reports the running release. Feedback is welcome on the [IDC support forum](https://discourse.canceridc.dev).
{% endhint %}

## Shared concepts

The MCP server and the [REST API](../api/README.md) are two surfaces over a single backend: a capability is implemented once and exposed in both. The difference is *who is calling* — you write the code against REST, whereas MCP is called by an LLM agent, with prescriptive tool descriptions and an `idc://guide` resource so the agent follows the recommended "ground-first" workflow on its own.

Because the two share a backend, the [data model](../api/idc-api-concepts.md#the-data-model) and [query surfaces](../api/idc-api-concepts.md#the-query-surfaces-and-how-they-relate) documented for the REST API apply unchanged to MCP.

For how this server compares with the [IDC agent skill](../agents/skill/README.md) — the other way to give an assistant access to IDC — see [Using IDC with an AI assistant](../agents/README.md).

## In this section

* [Connecting a client](getting-started.md) — set up claude.ai, Claude Desktop, or any other MCP client.
* [Tools and resources](tools.md) — the full list of capabilities the server exposes.

The server is developed in the open at [ImagingDataCommons/IDC-REST-MCP](https://github.com/ImagingDataCommons/IDC-REST-MCP).
