# IDC data model and query surfaces

This page explains the data model behind IDC's programmatic interfaces — the [REST API](README.md), the [MCP server](../mcp/README.md), and the [agent skill](../agents/skill/README.md) — along with the distinct "surfaces" they expose and the recommended workflow for getting from a question to the data. It is written in terms of the REST API's endpoints, but the concepts apply unchanged to the other two.

## The data model

IDC stores public cancer imaging as **DICOM**, organized as a hierarchy:

```
Patient → Study → Series        (the DICOM hierarchy; the `index` table has one row per Series)
   labelled by two independent grouping axes:
     collection_id              the source dataset (e.g. `nlst`, `tcga_luad`); a patient
                                belongs to exactly one collection
     analysis_result_id         a derived dataset (segmentations / annotations / radiomics);
                                a single analysis result can span multiple collections
```

The main queryable table is **`index`** — **one row per series**, the unit you filter, count, and download.

`collection_id` and `analysis_result_id` are **orthogonal**: an analysis result is not nested under one collection, so filtering by a `collection_id` will not necessarily capture all of an analysis result's series, and vice versa. Filter on whichever axis you actually mean. (`analysis_results_index` lists each result's source collections in its plural `collections` field.)

IDC is large (100+ TB total), so **always check counts and size before downloading.**

## The query surfaces, and how they relate

The API exposes a few distinct surfaces that build on each other:

```
 DISCOVERY ───▶ COHORT ───▶ RETRIEVAL            ⟍
 what exists?   how big is    download links       ⟍   SQL  (escape hatch:
 what can I     my filtered   (URLs, manifest,      ⟋   anything the structured
 filter on?     selection?    idc commands)        ⟋    surfaces can't express)
       │            ▲
       └─ provides the vocabulary (attributes + valid values) ─┘
```

| Surface | Answers | Key endpoints |
|---|---|---|
| **Discovery** | "What exists? What can I filter on?" | `GET /v3/version`, `/v3/stats`, `/v3/collections`, `/v3/collections/{id}`, `/v3/analysis_results`, `/v3/attributes`, `/v3/attributes/{attr}/values` |
| **Cohort** | "How big is my selection, and what's in it?" | `POST /v3/cohort/counts`, `POST /v3/cohort/manifest` |
| **Retrieval** | "Give me the download links" | `POST /v3/cohort/manifest.txt` |
| **SQL** | "Run my custom query" (+ schema) | `GET /v3/tables`, `/v3/tables/{table}`, `POST /v3/sql` |
| **Side tools** | View / cite / license-check a cohort | `GET /v3/viewer-url`, `POST /v3/citations`, `POST /v3/licenses` |

In one paragraph: **Discovery** hands you the lay of the land *and the vocabulary* — the attribute names and valid values you'll filter on. **Cohort** turns a chosen combination of that vocabulary into distinct counts, a page of matching series, and a ready-to-use download payload. **Retrieval** is the download half on its own — public `s3://` URLs, a full `manifest.txt`, and `idc` CLI commands. **SQL** is the bypass: when your selection needs a `GROUP BY`, a join, or an aggregation that structured cohort filters can't express, you write a read-only `SELECT` (see [Querying with SQL](querying-with-sql.md)). The side tools (viewer / citations / licenses) all operate on the *same* cohort filters.

## Filter syntax

Cohort filters (used by `cohort/counts`, `cohort/manifest`, `cohort/manifest.txt`, `licenses`, and `citations`) have two parts:

* **`terms`** — `{attribute: [values]}` for equality / membership. Values are **OR**'d *within* an attribute and **AND**'d *across* attributes. For example, `{"Modality": ["CT", "MR"], "collection_id": ["nlst"]}` means "(CT OR MR) AND in the nlst collection."
* **`ranges`** — `{attribute: {"gte": x, "lte": y}}` for the numeric and date attributes (e.g. `instanceCount`, `series_size_MB`, `StudyDate`). Either bound may be omitted for an open-ended range.

```json
{
  "terms": {"Modality": ["CT"], "collection_id": ["nlst"]},
  "ranges": {"instanceCount": {"gte": 100, "lte": 200}}
}
```

{% hint style="info" %}
This is a different, simpler filter syntax than the earlier V2 API (which used per-attribute suffixes like `_btw`). If you are migrating from V2, note that ranges are now expressed with `gte`/`lte` inside a `ranges` object.
{% endhint %}

These structured filters are not SQL — they operate only on the `index` table's filterable attributes. For a range on a property that isn't a filterable attribute (for example a clinical value such as patient age, which lives in the [clinical tables](querying-with-sql.md#clinical-non-imaging-data)), use the [SQL surface](querying-with-sql.md) instead.

Always **ground your values first** with `GET /v3/attributes` (what you can filter on — including which attributes are `term` vs `range`) and `GET /v3/attributes/{attr}/values` (the real values and their correct casing). Don't guess values.

## Recommended workflow

Start from the shape of your question — there are two entry points:

**A. Simple attribute filter** (e.g. *"breast MRI from NLST"*):

1. **Ground values** — `GET /v3/attributes`, then `GET /v3/attributes/{attr}/values`. If the property you need isn't among the attributes (e.g. *what anatomy a segmentation contains*), it lives in a specialized index — switch to path B.
2. **Build and size** — `POST /v3/cohort/counts` (cheap) to sanity-check size, then `POST /v3/cohort/manifest` for the series page and download payload.

**B. Relational or aggregate question** (e.g. *"modalities present per collection"*, *"series matching a joined condition"*) → **go straight to SQL**:

1. **Ground the schema** — `GET /v3/tables`, then `GET /v3/tables/{table}`. Don't guess table/column names.
2. **Query** — `POST /v3/sql`. Select `series_aws_url` (or `SeriesInstanceUID`) if you want a manifest out of it.

**Both paths then:** get the data — prefer the returned `idc` commands / `manifest.txt` (direct from S3/GCS; see [Getting the data](getting-data.md)) — and **be a good citizen** — check `licenses` (CC BY vs CC BY-NC) and include `citations` when you publish.

{% hint style="info" %}
**Explore narrow, then widen.** While you're still figuring out a query, keep result sizes small — a low `page_size` / `max_rows` / `limit`, or a `COUNT`/`GROUP BY` instead of raw rows — and raise the limit only once you know you need the full set. Size-capped responses include a `truncated` boolean: `false` means the result is complete; `true` means raise the limit and re-check (or narrow/aggregate).
{% endhint %}
