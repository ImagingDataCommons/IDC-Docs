# Organization of data

**TODO: need to check with the ISB team where TCGA clinical data lives**

IDC approach to storage and management of DICOM data is relying on the Google Cloud Platform [Healthcare API](https://cloud.google.com/healthcare/docs/how-tos/dicom). We maintain three representations of the data, which are fully synchronized and correspond to the same dataset, but are intended to serve different use cases.

{% hint style="warning" %}
In order to access the resources listed below, it is assumed you have completed the ["getting started" steps](../introduction/getting-started-with-gcp.md) to access Google Cloud console!
{% endhint %}

All of the resources listed below are accessible under the [`canceridc-data` GCP project](https://console.cloud.google.com/home/dashboard?project=canceridc-data).

### Storage Buckets

Storage buckets are named using the format `idc-tcia-<TCIA_COLLECTION_NAME>`, where `TCIA_COLLECTION_NAME` corresponds to the collection name in the collections table here.

Within the bucket, DICOM files are organized using the following directory naming conventions:

`dicom/<StudyInstanceUID>/<SeriesInstanceUID>/<SOPInstanceUID>.dcm`

where `*InstanceUID`s correspond to the respective value of the DICOM attributes in the stored DICOM files.

You can read about accessing GCP storage buckets from a Compute VM [here](https://cloud.google.com/compute/docs/disks/gcs-buckets).

### BigQuery Tables

Google [BigQuery \(BQ\)](https://cloud.google.com/bigquery) is a massively-parallel analytics engine ideal for working with tabular data. IDC utilizes the standard capabilities of the Google Healthcare API to extract all of the DICOM metadata from the hosted collections into a single BQ table. IDC users can access this table to conduct detailed exploration of the metadata content, and build cohorts using fine-grained controls not accessible from the IDC portal.

In addition to the DICOM metadata tables, we maintain several additional tables that curate metadata non-DICOM metadata \(e.g., attribution of a given item to a specific collection and DOI, collection-level metadata, etc\).

TODO: transfer the tables summary from [this document ](https://docs.google.com/document/d/14Wfu-q3hbRxlOSDGIWC0kkSI3CsYoKyB5EhmgY52TgM/edit#heading=h.sv8u3qvr2f7r)as we get closer to the release!

In addition to the BQ tables, we maintain several BQ views \(read more about BQ views here\) 

### DICOM Stores

IDC MVP utilizes a single Google Healthcare DICOM store to host all of the collections. That store, however, is primarily intended to support visualization of the data using OHIF Viewer. At this time, we do not support access of the hosted data via DICOMWeb interface by the IDC users. See more details in the [discussion here](https://discourse.canceridc.dev/t/dicomweb-access-to-hosted-collections/69).

