# Organization of data

IDC approach to storage and management of DICOM data is relying on the Google Cloud Platform [Healthcare API](https://cloud.google.com/healthcare/docs/how-tos/dicom). We maintain three representations of the data, which are fully synchronized and correspond to the same dataset, but are intended to serve different use cases.

{% hint style="warning" %}
In order to access the resources listed below, it is assumed you have completed the ["getting started" steps](../introduction/getting-started-with-gcp.md) to access Google Cloud console!
{% endhint %}

All of the resources listed below are accessible under the [`canceridc-data` GCP project](https://console.cloud.google.com/home/dashboard?project=canceridc-data).

### Storage Buckets

{% hint style="info" %}
Storage Buckets are basic containers in Google Cloud that provide storage for data objects \(you can read more about the relevant terms in the Google Cloud Storage documentation [here](https://cloud.google.com/storage/docs/key-terms)\).
{% endhint %}

Storage buckets are named using the format `idc-tcia-<TCIA_COLLECTION_NAME>`, where `TCIA_COLLECTION_NAME` corresponds to the collection name in the collections table here.

Within the bucket, DICOM files are organized using the following directory naming conventions:

`dicom/<StudyInstanceUID>/<SeriesInstanceUID>/<SOPInstanceUID>.dcm`

where `*InstanceUID`s correspond to the respective value of the DICOM attributes in the stored DICOM files.

You can read about accessing GCP storage buckets from a Compute VM [here](https://cloud.google.com/compute/docs/disks/gcs-buckets).

All of the IDC buckets are [requester-pays](https://cloud.google.com/storage/docs/requester-pays), which means you will need to provide Project ID for a project that has billing set up if you want to download the data from those buckets. 

{% hint style="warning" %}
Make sure you understand the [data egress charges](https://cloud.google.com/storage/pricing#network-egress)! As a general rule of thumb, download of the data to a GCP compute VM is free, while download to your laptop or a VM that belongs to a different cloud provider is expensive!

As an example, if you were to download to your laptop ALL of the DICOM data included in the October 2020 release of IDC, which is about 1 TB, you would need to pay a total of around $120 in egress charges.
{% endhint %}

Assuming you have a list of GCS URLs in `gcs_paths.txt`, you can download the corresponding items using the command below, substituting `$PROJECT_ID` with the valid GCP Project ID \(see the complete example in [this notebook](https://github.com/ImagingDataCommons/IDC-Examples/blob/master/notebooks/Cohort_download.ipynb)\):

```bash
$ cat gcs_paths.txt | gsutil -u $PROJECT_ID -m cp -I .
```

### BigQuery Tables

{% hint style="info" %}
Google [BigQuery \(BQ\)](https://cloud.google.com/bigquery) is a massively-parallel analytics engine ideal for working with tabular data. Data stored in BQ can be accessed using [standard SQL](https://cloud.google.com/bigquery/docs/reference/standard-sql/enabling-standard-sql) queries.
{% endhint %}

IDC utilizes the standard capabilities of the Google Healthcare API to extract all of the DICOM metadata from the hosted collections into a single BQ table. Conventions of how DICOM attributes of various types are converted into BQ form are covered in the [Understanding the BigQuery DICOM schema](https://cloud.google.com/healthcare/docs/how-tos/dicom-bigquery-schema) Healthcare API documentation article.

{% hint style="warning" %}
Due to the existing limitations of Google Healthcare API, not all of the DICOM attributes are extracted and are available in BigQuery tables. Specifically:

* sequences that have more than 15 levels of nesting are not extracted \(see [https://cloud.google.com/bigquery/docs/nested-repeated](https://cloud.google.com/bigquery/docs/nested-repeated)\) - we believe this limitation does not affect the data stored in IDC
* sequences that contain around 1MiB of data are dropped from BigQuery export and RetrieveMetadata output currently. 1MiB is not an exact limit, but it can be used as a rough estimate of whether or not the API will drop the tag \(this limitation was not documented as of writing this\) - we know that some of the instances in IDC will be affected by this limitation. The fix for this limitation is targeted for sometime in 2021, according to the communication with Google Healthcare support. 
{% endhint %}

IDC users can access this table to conduct detailed exploration of the metadata content, and build cohorts using fine-grained controls not accessible from the IDC portal.

In addition to the DICOM metadata tables, we maintain several additional tables that curate metadata non-DICOM metadata \(e.g., attribution of a given item to a specific collection and DOI, collection-level metadata, etc\).

* [`canceridc-data.idc.dicom_metadata`](https://console.cloud.google.com/bigquery?project=canceridc-data&p=canceridc-data&d=idc&t=dicom_metadata&page=table): DICOM metadata for all of the data hosted by IDC
* [`canceridc-data.idc.data_collections_metadata`](https://console.cloud.google.com/bigquery?project=canceridc-data&p=canceridc-data&d=idc&t=data_collections_metadata&page=table) : collection-level metadata for the original TCIA data collections hosted by IDC, for the most part corresponding to the content available in [this table at TCIA](https://www.cancerimagingarchive.net/collections/)
* \`\`[`canceridc-data.idc.analysis_collections_metadata`](https://console.cloud.google.com/bigquery?project=canceridc-data&p=canceridc-data&d=idc&t=analysis_collections_metadata&page=table) : collection-level metadata for the TCIA analysis collections hosted by IDC, for the most part corresponding to the content available in [this table at TCIA](https://www.cancerimagingarchive.net/tcia-analysis-results/)

In addition to the tables above, we provide the following [BigQuery views](https://cloud.google.com/bigquery/docs/views-intro) \(virtual tables defined by queries\) that extract specific subsets of metadata, or combine attributes across different tables, for convenience of the users

* [`canceridc-data.idc_views.dicom_all`](https://console.cloud.google.com/bigquery?project=canceridc-data&p=canceridc-data&d=idc_views&t=dicom_all&page=table): DICOM metadata together with the collection-level metadata
* \`\`[`canceridc-data.idc_views.segmentations`](https://console.cloud.google.com/bigquery?project=canceridc-data&p=canceridc-data&d=idc_views&t=segmentations&page=table): attributes of the segments stored in DICOM Segmentation object
* [`canceridc-data.idc_views.measurement_groups`](https://console.cloud.google.com/bigquery?project=canceridc-data&p=canceridc-data&d=idc_views&t=measurement_groups&page=table): measurement group sequences extracted from the DICOM SR TID1500 objects
* [`canceridc-data.idc_views.qualitative_measurements`](https://console.cloud.google.com/bigquery?project=canceridc-data&p=canceridc-data&d=idc_views&t=qualitative_measurements&page=table): coded evaluation results extracted from the DICOM SR TID1500 objects
* [`canceridc-data.idc_views.quantitative_measurements`](https://console.cloud.google.com/bigquery?project=canceridc-data&p=canceridc-data&d=idc_views&t=quantitative_measurements&page=table): quantitative evaluation results extracted from the DICOM SR TID1500 objects

### DICOM Stores

IDC MVP utilizes a single Google Healthcare DICOM store to host all of the collections. That store, however, is primarily intended to support visualization of the data using OHIF Viewer. At this time, we do not support access of the hosted data via DICOMWeb interface by the IDC users. See more details in the [discussion here](https://discourse.canceridc.dev/t/dicomweb-access-to-hosted-collections/69), and please comment about your use case if you have a need to access data via the DICOMweb interface.

### BigQuery tables external to IDC

In addition to the DICOM data, some of the image-related data hosted by IDC is stored in additional tables. These include the following:

* BigQuery TCGA clinical data: [`isb-cgc:TCGA_bioclin_v0.clinical_v1`](https://console.cloud.google.com/bigquery?project=isb-cgc&p=isb-cgc&d=TCGA_bioclin_v0&t=clinical_v1&page=table) . Note that this table is hosted under the ISB-CGC Google project, as documented [here](https://isb-cancer-genomics-cloud.readthedocs.io/en/latest/sections/BigQuery/ISBCGC-BQ-Projects.html), and its location may change in the future!

