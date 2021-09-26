# Files and metadata

## Storage Buckets

{% hint style="info" %}
Storage Buckets are basic containers in Google Cloud that provide storage for data objects \(you can read more about the relevant terms in the Google Cloud Storage documentation [here](https://cloud.google.com/storage/docs/key-terms)\).
{% endhint %}

All IDC DICOM file data for all IDC data versions and all of the [collections hosted by IDC](https://imaging.datacommons.cancer.gov/collections/) are maintained in Google Cloud Storage \(GCS\), from which it is available to the user on a [Requester Pays](https://cloud.google.com/storage/docs/requester-pays) basis. Currently all DICOM files are in the `idc-open` bucket.

The object namespace is flat, where every object name is composed of a standard format CRDC UUIDs and with the ".dcm" file extension, e.g. `905c82fd-b1b7-4610-8808-b0c8466b4dee.dcm`. For example, that instance can be accessed using [gsutil](https://cloud.google.com/storage/docs/gsutil) as `gs://idc-open/905c82fd-b1b7-4610-8808-b0c8466b4dee.dcm`

You can read about accessing GCP storage buckets from a Compute VM [here](https://cloud.google.com/compute/docs/disks/gcs-buckets). Because the `idc-open` bucket has a Requester Pays policy, you will need to provide a Project ID for a project for which billing has been configured in order to be able to download data from that bucket.

{% hint style="warning" %}
Make sure you understand the [data egress charges](https://cloud.google.com/storage/pricing#network-egress)! As a general rule of thumb, downloading of the data to a GCP compute VM within the same GCS location as the bucket is free, while downloading to your laptop, or another Google VM in a different GCS location, or a VM that belongs to a different cloud provider is expensive!

As an example, if you were to download to your laptop ALL of the DICOM data included in the V2 release of IDC, which is about 6 TB, you would need to pay a total of around $120\) in egress charges.
{% endhint %}

Typically, the user would not interact with the storage buckets to select and copy files \(unless the intent is to copy the entire content hosted by IDC\). Instead, one should use either IDC Portal, or IDC BigQuery tables containing metadata corresponding to the files to identify items of interest and define a cohort. The manifest of the cohort will include both the Google Storage URLs for the corresponding files in the bucket, and the [CRDC UUIDs](guids-and-uuids.md), which can be resolved to the Google Storage URLs to access the files.

Assuming you have a list of GCS URLs in `gcs_paths.txt`, you can download the corresponding items using the command below, substituting `$PROJECT_ID` with the valid GCP Project ID \(see the complete example in [this notebook](https://github.com/ImagingDataCommons/IDC-Examples/blob/master/notebooks/Cohort_download.ipynb)\):

```bash
$ cat gcs_paths.txt | gsutil -u $PROJECT_ID -m cp -I .
```

## BigQuery Tables

{% hint style="info" %}
Google [BigQuery \(BQ\)](https://cloud.google.com/bigquery) is a massively-parallel analytics engine ideal for working with tabular data. Data stored in BQ can be accessed using [standard SQL](https://cloud.google.com/bigquery/docs/reference/standard-sql/enabling-standard-sql) queries.
{% endhint %}

The flat address space of IDC DICOM objects in GCS storage is accompanied by BigQuery tables that allow the researcher to reconstruct the DICOM hierarchy as it exists for any given version.

There are several important BQ tables and views in which we keep copies of the metadata exposed via the TCIA interface at the time this version was captured and other pertinent information.

There is an instance of each of the following tables and views per IDC version. The set of tables and views corresponding to an IDC version are collected in a single BQ dataset per IDC version, `idc_<idc_version_number>`. As an example, the BQ tables for IDC version 2 are in the `canceridc-data.idc_v2`dataset.

Several Google BigQuery \(BQ\) tables support searches against metadata extracted from the data files. Additional BQ tables define the composition of each IDC data version.

We maintain several additional tables that curate metadata non-DICOM metadata \(e.g., attribution of a given item to a specific collection and DOI, collection-level metadata, etc\).

* `canceridc-data.idc_v<idc_version_number>.auxiliary_metadata` \(also available via [`canceridc-data.idc_current.auxiliary_metadata`](https://console.cloud.google.com/bigquery?p=canceridc-data&d=idc_current&t=auxiliary_metadata&page=table) view for the current version of IDC data\) This table defines the contents of the corresponding IDC version. There is a row for each instance in the version. Version attributes:
  * `idc_version_number:` The IDC version number.
  * `version_hash`: the md5 hash of the sorted `collection_hashes` of all collections in this version

    Collection attributes:

  * `tcia_api_collection_id:` The ID, as accepted by the TCIA API, of the original data collection containing this instance.
  * `idc_webapp_collection_id:`The ID, as accepted by the IDC web app, of the original data collection containing this instance.
  * `source_doi:`A DOI of the TCIA wiki page corresponding to the original data collection or analysis results that is the source of this instance.
  * `collection_hash`: The md5 hash of the sorted `patient_hashes` of all patients in the collection containing this instance.

    Patient attributes:

  * `submitter_case_id:`The submitter’s \(of data to TCIA\) ID of the patient containing this instance. This is the DICOM PatientID.
  * `idc_case_id:`IDC generated UUID that uniquely identifies the patient containing this instance.

    This is needed because DICOM PatientIDs are not required to be globally unique.

  * `patient_hash`: the md5 hash of the sorted `study_hashes` of all studies in the patient containing this instance.

    Study attributes:

  * `StudyInstanceUID:` DICOM UID of the study containing this instance.
  * `study_uuid:`IDC assigned UUID that identifies a version of the study containing this instance.
  * `study_hash`: the md5 hash of the sorted `series_hashes` of all series in study containing this instance.

    Series attributes:

  * `SeriesInstanceUID:` DICOM UID of the series containing this instance.
  * `series_uuid:`IDC assigned UUID that identifies a version of the series containing this instance.
  * `series_hash`: the md5 hash of the sorted `instance_hashes` of all instance in the series containing this instance.

    Instance attributes:

  * `SOPInstanceUID:` DICOM UID of this instance.
  * `instance_uuid:`IDC assigned UUID that identifies a version of this instance.
  * `gcs_url:` The GCS URL of a file containing the version of this instance that is identified by the `instance_uuid`
  * `instance_hash`: the md5 hash of the version of this instance that is identified by the `instance_uuid`
  * `instance_size:` the size, in bytes, of this version of the instance that is identified by the `instance_uuid` 
* `canceridc-data.idc_v<idc_version_number>.dicom_metadata` \(also available via [`canceridc-data.idc_current.dicom_metadata`](https://console.cloud.google.com/bigquery?p=canceridc-data&d=idc_current&t=dicom_metadata&page=table)  view for the current version of IDC data\) DICOM metadata for each instance in the corresponding IDC version. IDC utilizes the standard capabilities of the Google Healthcare API to extract all of the DICOM metadata from the hosted collections into a single BQ table. Conventions of how DICOM attributes of various types are converted into BQ form are covered in the [Understanding the BigQuery DICOM schema](https://cloud.google.com/healthcare/docs/how-tos/dicom-bigquery-schema) Google Healthcare API documentation article. IDC users can access this table to conduct detailed exploration of the metadata content, and build cohorts using fine-grained controls not accessible from the IDC portal. The schema is too large to document here. Refer to the BQ table and the above referenced documentation.

{% hint style="warning" %}
Due to the existing limitations of Google Healthcare API, not all of the DICOM attributes are extracted and are available in BigQuery tables. Specifically:

* sequences that have more than 15 levels of nesting are not extracted \(see [https://cloud.google.com/bigquery/docs/nested-repeated](https://cloud.google.com/bigquery/docs/nested-repeated)\) - we believe this limitation does not affect the data stored in IDC
* sequences that contain around 1MiB of data are dropped from BigQuery export and RetrieveMetadata output currently. 1MiB is not an exact limit, but it can be used as a rough estimate of whether or not the API will drop the tag \(this limitation was not documented as of writing this\) - we know that some of the instances in IDC will be affected by this limitation. The fix for this limitation is targeted for sometime in 2021, according to the communication with Google Healthcare support. 
{% endhint %}

* `canceridc-data.idc_v<idc_version_number>.original_collections_metadata` \(also available via [`canceridc-data.idc_current.original_collections_metadata`](https://console.cloud.google.com/bigquery?p=canceridc-data&d=idc_current&t=original_collections_metadata&page=table)  view for the current version of IDC data Collection-level metadata for the original TCIA data collections hosted by IDC, for the most part corresponding to the content available in [this table at TCIA](https://www.cancerimagingarchive.net/collections/). One row per collection:
  * `tcia_api_collection_id:` The collection ID as is accepted by the TCIA AP
  * `tcia_wiki_collection_id:` The collection ID as on the TCIA wiki page
  * `idc_webapp_collection_id:`The collection ID as accepted by the IDC web app
  * `Status:`Collection status" Ongoing or complete
  * `Access:`Collection access conditions: Limited or Public
  * `ImageType:` Enumeration of image types/modalities in the collection
  * `Subjects:`Number of subjects in the collection
  * `DOI:`DOI that can be resolved at doi.org to the TCIA wiki page for this collection
  * `CancerType:`TCIA assigned cancer type of this collection
  * `SupportingData:`Type\(s\) of additional data available
  * `Location:`Body location that was studied
  * `Description:`TCIA description of the collection \(HTML format\) 
* `canceridc-data.idc.idc_v<idc_version_number>.analysis_results_metadata` \(also available via [`canceridc-data.idc_current.analysis_results_metadata`](https://console.cloud.google.com/bigquery?p=canceridc-data&d=idc_current&t=analysis_results_metadata&page=table) view for the current version of IDC data\)  Metadata for the TCIA analysis results hosted by IDC, for the most part corresponding to the content available in [this table at TCIA](https://www.cancerimagingarchive.net/tcia-analysis-results/). One row per analysis result:
  * `Collection:`Descriptive name
  * `DOI:`DOI that can be resolved at doi.org to the TCIA wiki page for this analysis result
  * `CancerType:`TCIA assigned cancer type of this analysis result
  * `Location:`Body location that was studied
  * `Subjects:`Number of subjects in the analysis result
  * `Collections:` Original collections studied
  * `AnalysisArtifactsonTCIA:` Type\(s\) of analysis artifacts generated
  * `Updated:` Data when results were last updated

In addition to the tables above, we provide the following [BigQuery views](https://cloud.google.com/bigquery/docs/views-intro) \(virtual tables defined by queries\) that extract specific subsets of metadata, or combine attributes across different tables, for convenience of the users

* `canceridc-data.idc_v<idc_version_number>.dicom_all` \(also available via [`canceridc-data.idccurrent.dicom_all`](https://console.cloud.google.com/bigquery?p=canceridc-data&d=idc_current&t=dicom_all&page=table) view for the current version of IDC data\) DICOM metadata together with selected auxiliary and collection metadata
* `canceridc-data.idc_v<idc_version_number>.segmentations` \(also available via [`canceridc-data.idc_current.segmentations`](https://console.cloud.google.com/bigquery?p=canceridc-data&d=idc_current&t=segmentations&page=table) view for the current version of IDC data\) Attributes of the segments stored in DICOM Segmentation object
* `canceridc-data.idc_v<idc_version_number>.measurement_groups` \(also available via[`canceridc-data.idc_current.measurement_groups`](https://console.cloud.google.com/bigquery?p=canceridc-data&d=idc_current&t=measurement_groups&page=table)\`\`

  view for the current version of IDC data\) Measurement group sequences extracted from the DICOM SR TID1500 objects

* `canceridc-data.idc_v<idc_version_number>.qualitative_measurements` \(also available via [`canceridc-data.idc_current.qualitative_measurements`](https://console.cloud.google.com/bigquery?p=canceridc-data&d=idc_current&t=qualitative_measurements&page=table) view for the current version of IDC data\) Coded evaluation results extracted from the DICOM SR TID1500 objects
* `canceridc-data.idc_v<idc_version_number>.quantitative_measurements` \(also available via [`canceridc-data.idc_current.quantitative_measurements`](https://console.cloud.google.com/bigquery?p=canceridc-data&d=idc_current&t=quantitative_measurements&page=table) view for the current version of IDC data\) Quantitative evaluation results extracted from the DICOM SR TID1500 objects

## DICOM Stores

IDC utilizes a single Google Healthcare DICOM store to host all of the instances in the current IDC version. That store, however, is primarily intended to support visualization of the data using OHIF Viewer. At this time, we do not support access of the hosted data via DICOMWeb interface by the IDC users. See more details in the [discussion here](https://discourse.canceridc.dev/t/dicomweb-access-to-hosted-collections/69), and please comment about your use case if you have a need to access data via the DICOMweb interface.

## BigQuery tables external to IDC

In addition to the DICOM data, some of the image-related data hosted by IDC is stored in additional tables. These include the following:

* BigQuery TCGA clinical data: [`isb-cgc:TCGA_bioclin_v0.clinical_v1`](https://console.cloud.google.com/bigquery?project=isb-cgc&p=isb-cgc&d=TCGA_bioclin_v0&t=clinical_v1&page=table) . Note that this table is hosted under the ISB-CGC Google project, as documented [here](https://isb-cancer-genomics-cloud.readthedocs.io/en/latest/sections/BigQuery/ISBCGC-BQ-Projects.html), and its location may change in the future!

