# Organization of data

This section describes the organization of IDC data from IDC Version 2. IDC Version 1 data organization is described in the [Organization of data in V1](https://github.com/ImagingDataCommons/IDC-Docs-dev/tree/23f41756fb1fc5d0f3c3a552f6dd0d4c1325a96e/data/README.md) section.

IDC storage and management of DICOM data relies on the Google Cloud Platform. We maintain three representations of the data \(namely, as files in Storage buckets, metadata tables in BigQuery tables, and DICOM instances within DICOM stores\), which are fully synchronized and correspond to the same content, but are intended to serve different use cases.

{% hint style="warning" %}
In order to access the resources listed below, it is assumed you have completed the ["getting started" steps](../../introduction/getting-started-with-gcp.md) to access the Google Cloud console!
{% endhint %}

All of the resources listed below are accessible under the [`canceridc-data` GCP project](https://console.cloud.google.com/home/dashboard?project=canceridc-data).

## [Files and metadata](files-and-metadata.md)

## [GA4GH DRS objects](ga4gh-drs-objects.md)

## [Organization of data in v1 \(deprecated\)](organization-of-data-v1.md)

## 

