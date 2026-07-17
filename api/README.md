# API

The IDC API provides programmatic access to IDC metadata and data: discover what is in IDC, build cohorts with attribute filters, retrieve download manifests, run read-only SQL queries, explore clinical data, and generate citations and license summaries for your selections.

The API is live in production at:

```
https://api.imaging.datacommons.cancer.gov
```

All IDC data is public and open — **no authentication, account, or credentials are required** to use the API.

{% hint style="info" %}
The current API release is version 3 (v3), in **beta**. The `/v3` contract may still change in response to feedback before the final `3.0.0` release. To see exactly which beta build is deployed, call [`GET /v3/version`](endpoint-details.md) — the `api_version` field reports the running release. If you have feedback, please let us know on the [IDC support forum](https://discourse.canceridc.dev)!
{% endhint %}

The same capabilities are available through two surfaces, which share a single backend:

* **REST API** (documented in this section) — plain HTTP/JSON for scripts, applications, and notebooks. Interactive Swagger UI at [https://api.imaging.datacommons.cancer.gov/v3/docs](https://api.imaging.datacommons.cancer.gov/v3/docs), OpenAPI specification at [/v3/openapi.json](https://api.imaging.datacommons.cancer.gov/v3/openapi.json).
* **MCP server** — the same capabilities exposed as [Model Context Protocol](https://modelcontextprotocol.io/) tools, so LLM agents (Claude and others) can query IDC directly. See the [MCP section](../mcp/README.md) of this documentation.

Because both surfaces are implemented over one core, anything you can do over REST you can also do over MCP, and vice versa.

## What you can do

* **Discover** what's in IDC — collections, derived analysis results (segmentations, annotations), filterable attributes and their valid values, headline statistics.
* **Build cohorts** — turn attribute filters into distinct patient/study/series counts, a page of matching series, and a ready-to-use download payload.
* **Retrieve** — public `s3://` URLs, a full `manifest.txt`, and `idc` CLI commands; files transfer directly from public S3/GCS buckets, never through the API server.
* **Run SQL** — guarded read-only queries against the series-level `index` table, specialized per-modality indices, and per-collection clinical tables — joins, aggregations, anything the structured filters can't express.
* **Explore clinical data** — discover and read the per-collection clinical tables (demographics, staging, therapies, outcomes) and join them to imaging.
* **Publish responsibly** — viewer URLs for visual inspection, per-cohort license breakdowns (CC BY vs CC BY-NC), and ready-to-use citations.

## In this section

* [Getting Started](getting-started.md) — your first API calls.
* [IDC API Concepts](idc-api-concepts.md) — the data model, the query surfaces, and the recommended workflow.
* [Endpoint Details](endpoint-details.md) — the endpoint reference and worked examples.
* [Querying with SQL](querying-with-sql.md) — the guarded SQL surface and the tables available to it.
* [Getting the data](getting-data.md) — manifests, download commands, licenses, and citations.

The API is developed in the open at [ImagingDataCommons/IDC-REST-MCP](https://github.com/ImagingDataCommons/IDC-REST-MCP), which also hosts the developer-facing documentation.

{% hint style="warning" %}
Looking for documentation of the earlier API versions? The V1 and V2 APIs are superseded by v3; their documentation is preserved in the [Archive](../archive.md).
{% endhint %}
