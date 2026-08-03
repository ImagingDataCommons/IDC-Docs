# Your first API calls

The IDC REST API is available at the base URL `https://api.imaging.datacommons.cancer.gov`, with all endpoints under the `/v3` prefix. No authentication is required — you can try every example on this page from any terminal with `curl`.

{% hint style="info" %}
The interactive [Swagger UI](https://api.imaging.datacommons.cancer.gov/v3/docs) documents every endpoint with a filled-in request/response example, and lets you execute requests directly from the browser. The machine-readable OpenAPI specification is at [/v3/openapi.json](https://api.imaging.datacommons.cancer.gov/v3/openapi.json).
{% endhint %}

## Your first calls

Check which IDC data release the API is serving, and the headline totals:

```bash
curl -s https://api.imaging.datacommons.cancer.gov/v3/version
curl -s https://api.imaging.datacommons.cancer.gov/v3/stats
```

List the collections (datasets) available in IDC, or look at one in detail:

```bash
curl -s https://api.imaging.datacommons.cancer.gov/v3/collections
curl -s https://api.imaging.datacommons.cancer.gov/v3/collections/nlst
```

## Build your first cohort

Before filtering, discover the valid values of the attribute you want to filter on — don't guess:

```bash
curl -s 'https://api.imaging.datacommons.cancer.gov/v3/attributes/Modality/values?limit=10'
```

Then check how big your selection is (cheap), and request a page of matching series together with a ready-to-use download payload:

```bash
# distinct patient/study/series counts for the filter
curl -s https://api.imaging.datacommons.cancer.gov/v3/cohort/counts \
  -H 'content-type: application/json' \
  -d '{"terms": {"Modality": ["MR"], "BodyPartExamined": ["BREAST"]}}'

# counts + a page of series + download payload
curl -s https://api.imaging.datacommons.cancer.gov/v3/cohort/manifest \
  -H 'content-type: application/json' \
  -d '{"filters": {"terms": {"Modality": ["MR"], "BodyPartExamined": ["BREAST"]}}, "page_size": 3}'
```

The `manifest` response includes `idc` CLI commands you can run as-is to download the matching files directly from public cloud buckets — see [Getting the data](getting-data.md).

## Where to go next

* [Core concepts](idc-api-concepts.md) explains the data model, the query surfaces, and the recommended workflow — worth reading before you go beyond simple filters.
* [Endpoint reference](endpoint-details.md) lists every endpoint with worked examples.
* [Querying with SQL](querying-with-sql.md) covers questions that attribute filters can't express — joins, aggregations, and clinical data.
* Prefer to have an LLM agent do the querying? See [Using IDC with an AI assistant](../agents/README.md) — the same capabilities, exposed as agent tools.

{% hint style="info" %}
If you have feedback about the desired features of the IDC API, please let us know via the IDC [support forum](https://discourse.canceridc.dev).
{% endhint %}
