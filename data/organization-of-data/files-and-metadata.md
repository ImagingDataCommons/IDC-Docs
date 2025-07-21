# Files and metadata

{% hint style="info" %}
We gratefully acknowledge [Google Public Data Program](https://console.cloud.google.com/marketplace/product/bigquery-public-data/nci-idc-data) and the [AWS Open Data Sponsorship Program](https://registry.opendata.aws/nci-imaging-data-commons/) that support public hosting of IDC-curated content, and cover out-of-cloud egress fees!
{% endhint %}

Let's start with the overall principles of how we organize data in IDC.

IDC brings you (as of v21) over 85 TB of publicly available DICOM images and image-derived content. We share those with you as DICOM files, and those DICOM files are available in cloud-based **storage buckets** - both in Google and AWS.&#x20;

Sharing just the files, however, is not particularly helpful. With that much data, it is no longer practical to just download all of those files to later sort through them to select those you need.&#x20;

{% hint style="success" %}
Think of IDC as a library, where each file is a book. With that many books, it is not feasible to read them all, or even open each one to understand what is inside. Libraries are of little use without a catalog! &#x20;
{% endhint %}

To provide you with a catalog of our data, along with the files, we maintain _metadata_ that makes it possible to understand what is contained within files, and select the files that are of interest for your project, so that you can download just the files you need.&#x20;

In the following we describe organization of both the storage buckets containing the files, and the metadata catalog that you can use to select files that meet your needs. As you go over this documentation, please consider completing our ["Getting started" tutorial](https://github.com/ImagingDataCommons/IDC-Tutorials/tree/master/notebooks/getting_started) - it will give you the opportunity to apply the knowledge you gain by reading this article while interacting with the data, and should help better understand this content.

## Storage Buckets

{% hint style="info" %}
Storage Buckets are basic containers in Google Cloud Storage and AWS S3 that provide storage for data objects (you can read more about the relevant terms in the Google Cloud Storage documentation [here](https://cloud.google.com/storage/docs/key-terms) and in S3 [here](https://aws.amazon.com/s3/)).
{% endhint %}

All IDC DICOM file data for all IDC data versions across all of the [collections hosted by IDC](https://imaging.datacommons.cancer.gov/collections/) are mirrored between Google Cloud Storage (GCS) and AWS S3 buckets.&#x20;

Currently all DICOM files are maintained in buckets that allow for free egress within or out of the cloud. This is enabled through the partnership of IDC with [Google Public Data Program](https://console.cloud.google.com/marketplace/product/gcp-public-data-idc/nci-idc-data) and the [AWS Open Data Sponsorship Program](https://registry.opendata.aws/nci-imaging-data-commons/).

<table><thead><tr><th>Data category</th><th width="424.5574951171875">Cloud provider and bucket name</th></tr></thead><tbody><tr><td>Data covered by a non-restrictive license (CC-BY or like) and not labeled as such that <strong>may</strong> contain head scans. This category contains >90% of the data in IDC.</td><td><strong>AWS</strong>: <code>idc-open-data</code><br><strong>GCS</strong>: <code>idc-open-data</code><br>(until IDC v19, we utilized GCS bucket <code>public-datasets-idc</code> before it was superseded by <code>idc-open-data</code>)</td></tr><tr><td>Collections that <strong>may</strong> contain head scans. This is done for the collections that were labeled as such by TCIA, in case there is a change in policy and we need to treat such images in any special way in the future.</td><td><strong>AWS</strong>: <code>idc-open-data-two</code><br><strong>GCS</strong>: <code>idc-open-idc1</code></td></tr><tr><td>Data that is covered by a license that restricts commercial use (CC-NC). Note that the license information is available programmatically at the granularity of the individual files, as explained in <a href="https://github.com/ImagingDataCommons/IDC-Tutorials/blob/master/notebooks/getting_started/part3_exploring_cohorts.ipynb">this tutorial</a> - you do not need to check the bucket name to get the license information!</td><td><strong>AWS</strong>: <code>idc-open-data-cr</code><br><strong>GCS</strong>: <code>idc-open-cr</code></td></tr></tbody></table>

Within each bucket files are organized in folders, each folder containing files corresponding to a single DICOM series. On ingestion, we assign each DICOM series and each DICOM instance a UUID, in order to be able to support [data versioning](../data-versioning.md) (when needed). These UUIDs are available in our metadata indices, and are used to organized the content of the buckets: for each version of a DICOM instance having instance UUID `instance_uuid` in a version of a series version having UUID `series_uuid`, the file name is:

`<series_uuid>/<instance_uuid>.dcm`

Corresponding files have the same object name in GCS and S3, though the name of the containing buckets will be different.

## Metadata Tables

IDC metadata tables are provided to help you navigate IDC content and narrow down to the specific files that meet your research interests.

As a step in data ingestion process (summarized [earlier](./)), IDC extracts all of the DICOM metadata, merges it with collection-level and some other metadata attributes not available from DICOM, ingests collection-level clinical tables and stores the result in Google BigQuery tables searchable using SQL queries.

{% hint style="info" %}
Google [BigQuery (BQ)](https://cloud.google.com/bigquery) is a massively-parallel analytics engine ideal for working with tabular data. Data stored in BQ can be accessed using [standard SQL](https://cloud.google.com/bigquery/docs/reference/standard-sql/enabling-standard-sql) queries.
{% endhint %}

A small subset of most critical metadata attributes available in IDC BigQuery tables is extracted and made available via [`idc-index` python package](https://github.com/ImagingDataCommons/idc-index).&#x20;

If you are just starting with IDC, you can skip the details covering the content of BigQuery tables, and proceed to [this tutorial](https://github.com/ImagingDataCommons/IDC-Tutorials/blob/master/notebooks/getting_started/part2_searching_basics.ipynb) that will help you learn basics of searching IDC metadata using `idc-index`.&#x20;

Otherwise you can proceed to the next section to learn about organization of IDC BigQuery tables.
