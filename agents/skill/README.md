# The IDC agent skill

The [Imaging Data Commons Skill](https://github.com/ImagingDataCommons/imaging-data-commons-skill) is an [Agent Skill](https://agentskills.io/): a packaged set of instructions, reference guides, and helper scripts that an AI assistant loads on demand when you ask it about cancer imaging data.

It is not Claude-specific. It follows the open Agent Skills format, so it works with Claude (claude.ai, Claude Desktop, Claude Code, API) and with any other agent that supports the format — or you can load its instruction file into any assistant's context by hand.

Unlike the [hosted server](../../mcp/README.md), the skill has your assistant **write and run Python directly** against the [`idc-index`](https://github.com/ImagingDataCommons/idc-index) package in a code-execution sandbox. There is no IDC server in the loop, and no network round trip for metadata queries — but it only works where your assistant can execute code.

## What it gives your assistant

* A grounded understanding of the IDC data model and the `idc-index` metadata tables — which table answers which question, and how they join.
* Validated query patterns, so it writes SQL that runs instead of SQL that looks plausible.
* Download via the `idc-index` Python API and the `idc` command-line tool, including manifests and directory templates.
* Viewer URLs, so you can eyeball a series in the browser before committing to a download.
* License checks (CC BY vs CC BY-NC) and ready-to-use citations in several formats.

## Reference guides it bundles

The skill ships a set of topic guides that the assistant pulls in only when relevant, covering:

* the `idc-index` tables, their join keys, and when to use each
* end-to-end workflows, from a research question to downloaded files
* SQL patterns for cohort building and aggregation
* clinical (non-imaging) data — discovering tables, and joining them to imaging
* direct access to the public S3/GCS buckets, and reading DICOM straight from cloud storage
* DICOMweb access
* digital pathology and slide microscopy
* querying the IDC BigQuery tables
* the `idc` command-line interface
* reading the underlying Parquet index directly

**This is the main reason to add the skill.** Several of these surfaces — BigQuery, DICOMweb, direct bucket and Parquet access, the pathology and clinical-data workflows — are not exposed by the hosted server at all.

## Requirements

* Your assistant must be able to **execute Python** (3.10 or newer).
* Network access to PyPI, to install `idc-index`, and to the public IDC buckets.
* **No authentication for IDC data** — it is all public.
* The BigQuery and Google Healthcare DICOMweb paths additionally need your own GCP credentials. Everything else works without a cloud account.

## Do I need this if I already use the hosted server?

They overlap, and running both is the recommended setup.

* The [hosted server](../../mcp/README.md) covers discovery, cohort building, SQL, retrieval, licenses, and citations, with no setup beyond a URL.
* The skill adds the surfaces listed above, and does its metadata work locally rather than over the network.
* If your assistant can run Python, there is no downside to having both installed — the skill's own setup guide walks through adding the server alongside it.

If you would rather see the three access paths side by side, see [Using IDC with an AI assistant](../README.md).

## Staying current

The skill is versioned, and each release is pinned to a specific `idc-index` release and a specific IDC data version. IDC publishes new data releases several times a year, so a stale skill will describe a stale IDC.

* Watch the [releases page](https://github.com/ImagingDataCommons/imaging-data-commons-skill/releases) and update when a new version lands.
* After updating, **start a new conversation** — an assistant binds its skills at the start of a session.
* To check what your assistant is actually working from, ask it which IDC data version it sees.

## Where it lives

The skill is developed in the open at [ImagingDataCommons/imaging-data-commons-skill](https://github.com/ImagingDataCommons/imaging-data-commons-skill) and released under the MIT license (the data it reaches carries its own per-collection licenses). If the assistant gives you an incomplete or incorrect answer, please report it via the repository's issue tracker — the issue template is designed for exactly that.

Ready to install it? See [Setting up the skill](setup.md).
