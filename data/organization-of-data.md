# Organization of data

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

### DICOM Stores

All of the publicly available DICOM collections hosted by IDC are available in the single DICOM store &lt;LINK/LOCATION TBD&gt;

### BigQuery Tables

