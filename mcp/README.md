# MCP server

The IDC **MCP server** lets LLM agents query IDC directly. It exposes the same capabilities as the [REST API](../api/README.md) — discovery, cohort building, retrieval, SQL, clinical data, licenses, and citations — as [Model Context Protocol](https://modelcontextprotocol.io/) tools, so an agent such as Claude can find, describe, and download IDC data on your behalf, in plain conversation.

The [Model Context Protocol](https://modelcontextprotocol.io/) is an open standard for connecting LLM applications to external tools and data sources. Any spec-conformant MCP client can use the IDC server.

The server is live in production at:

```
https://api.imaging.datacommons.cancer.gov/mcp
```

It is public and **unauthenticated** — no API key, account, or config file needed. Point a client at the URL and ask it to find an imaging cohort.

{% hint style="info" %}
The MCP server and the REST API are two surfaces over one backend, released together as version 3, currently in **beta** (`3.0.0b2`). The contract may still change in response to feedback before the final `3.0.0` release. Feedback is welcome on the [IDC support forum](https://discourse.canceridc.dev).
{% endhint %}

## One backend, two surfaces

The MCP server and the REST API share a single core, so a capability is implemented once and exposed in both. The difference is *who is calling*:

* Use the **[REST API](../api/README.md)** when *you* write the code — scripts, apps, notebooks (plain HTTP/JSON, with a Swagger UI).
* Use **MCP** when an *LLM agent* does the querying — the same capabilities as tools, with prescriptive descriptions and an `idc://guide` resource so the agent follows the recommended "ground-first" workflow on its own.

Because the two surfaces share a backend, the [data model](../api/idc-api-concepts.md#the-data-model) and [query surfaces](../api/idc-api-concepts.md#the-query-surfaces-and-how-they-relate) documented for the REST API apply unchanged to MCP.

## Relation to the Imaging Data Commons Skill

The [Imaging Data Commons Skill](https://github.com/ImagingDataCommons/imaging-data-commons-skill) is a *different access path* to the same data. An [Agent Skill](https://agentskills.io/) that works with Claude and any other agent that supports the format, it has the agent write and run Python directly against [`idc-index`](https://github.com/ImagingDataCommons/idc-index) inside a code-execution sandbox (e.g. Claude Code, or Claude Desktop / claude.ai with code execution enabled) — no server involved, and no network round trip for metadata, but it only works where the client can execute Python locally.

The MCP server (and the REST API) instead expose the same `idc-index` data as callable tools over the network, for clients that can't or don't want to run code: remote-MCP connectors, non-Python agent frameworks, or a curated tool surface instead of hand-written SQL. Both share the same data model and the same ground-first workflow.

* Pick the **skill** when local Python execution is available.
* Pick **MCP** for network-only clients or the hosted, zero-setup path.

## In this section

* [Getting started](getting-started.md) — connect a client to the IDC MCP server.
* [Tools and resources](tools.md) — the full list of capabilities the server exposes.

The server is developed in the open at [ImagingDataCommons/IDC-REST-MCP](https://github.com/ImagingDataCommons/IDC-REST-MCP).
