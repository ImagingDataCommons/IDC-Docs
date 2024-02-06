# Cohort manifests

The cohort manifest identifies the sources of data in your cohort and allows you to store the content of the cohort outside of the portal, and download the files corresponding to the cohort.&#x20;

To export a cohort manifest, [Create a cohort](data-exploration-and-cohorts/understanding-cohorts.md) or click **Cohorts** on the top menu bar and select a cohort you previously created. Click the **Export Cohort Manifest** button: the Export Cohort Manifest dialog box appears. There are three options for exporting the cohort manifest, which serve different needs:

1. **`s5cmd` manifest**: this option is recommended if you want to download the files included in the cohort. You can choose the source of data while downloading the files between Google GCP and Amazon AWS buckets, depending on your needs (in either case, data egress is free).
2. **CSV/TSV/JSON**: use this option if you want to archive the manifest of your cohort. The information contained in this manifest will be sufficient to access the content cohort even after the IDC data is updated.
3. **BigQuery**: this option is most appropriate if you want to further refine your cohort using SQL interface.

## s5cmd cohort manifest

`s5cmd` manifest lists the commands needed to copy the files corresponding to the cohort using `s5cmd run` command from either GCP or AWS buckets (based on user preference). The header of the manifest includes the instructions to download the content of the cohort.&#x20;

{% code overflow="wrap" %}
```
# To download the files in this manifest, first install s5cmd (https://github.com/peak/s5cmd),
# then run the following command:
# s5cmd --no-sign-request --endpoint-url https://storage.googleapis.com run cohorts_996_20230504_80755_gcs.s5cmd
cp s3://public-datasets-idc/6079cc3b-4b0f-41ec-bdbb-c6e754e88229/* .
cp s3://public-datasets-idc/a1e77e5c-299f-49ef-a659-f4484e0cedd2/* .
...
```
{% endcode %}

Note that if you chose to download the cohort content from AWS buckets, the manifest header will contain AWS-specific instructions, and the source bucket location will be in AWS, as shown below. You can see that folder names, defined by `crdc_series_uuid`, are the same as in the GCP manifest, but the bucket is different.

{% code overflow="wrap" %}
```
# To download the files in this manifest, first install s5cmd (https://github.com/peak/s5cmd),
# then run the following command:
# s5cmd --no-sign-request --endpoint-url https://s3.amazonaws.com run cohorts_996_20230504_80755_aws.s5cmd
cp s3://idc-open-data/6079cc3b-4b0f-41ec-bdbb-c6e754e88229/* .
cp s3://idc-open-data/a1e77e5c-299f-49ef-a659-f4484e0cedd2/* .
...
```
{% endcode %}



## CSV/TSV/JSON cohort manifest

Comma-Separated Values (CSV) and Tab-Separated Values (TSV) files include header fields, so you can customize which of those fields you want to include in the export. You can also select which columns to include in the file.

The header of the CSV/TSV manifest contains the name of cohort, user, filters used, the date generated, and the total number of records. Data is separated by multiple files until the limit of 65,000 files is reached. If your data set is larger than this, you must export it using BigQuery.

![Example cohort manifest](../.gitbook/assets/mainfest-for-cohort.png)

{% hint style="warning" %}
Manifests exported as files are defined at the series level. There will be a single row for each DICOM series included in the manifest. This is due to the limitation on the maximum size of the file that can be exported. To download the individual DICOM files corresponding to the instances included in the series the CRDC UUID corresponding to the series object (`series_uuid`) will need to be [resolved to the URL of the underlying objects](../data/organization-of-data/organization-of-data-v2-through-v13-deprecated/guids-and-uuids.md).

Manifests exported into BigQuery are defined at the level of DICOM instances, with one row per instance.
{% endhint %}

The fields provided in a cohort manifest are:

* `PatientID`: value of the corresponding DICOM attribute
* `collection_id`: abbreviated identifier of the source data collection
* `StudyInstanceUID`: value of the corresponding DICOM attribute
* `SeriesInstanceUID`: value of the corresponding DICOM attribute
* `SOPInstanceUID`: value of the corresponding DICOM attribute (NB: included only when manifest is exported into BigQuery!)
* `source_DOI`: Digital Object Identifier (DOI) of the source data collection. Pre-pending `source_DOI` with `https://doi.org/` will give you the URL of the collection dataset
* `study_uuid`: CRDC UUID of the object maintained by CRDC IndexD corresponding to the DICOM study, which [can be resolved to the URL of the underlying objects](../data/organization-of-data/organization-of-data-v2-through-v13-deprecated/guids-and-uuids.md)
* `series_uuid`: CRDC UUID of the object maintained by CRDC IndexD corresponding to the DICOM series, which [can be resolved to the URL of the underlying objects](../data/organization-of-data/organization-of-data-v2-through-v13-deprecated/guids-and-uuids.md)
* `instance_uuid`: CRDC UUID of the object maintained by CRDC IndexD corresponding to the DICOM instance, which [can be resolved to the URL of the underlying objects](../data/organization-of-data/organization-of-data-v2-through-v13-deprecated/guids-and-uuids.md) (NB: included only when the manifest is exported into BigQuery!)
* `gcs_url`: `gs://` URL that can be used to access the object using the [GCP `gsutil` tool](https://cloud.google.com/storage/docs/gsutil)

An example of how you can use an IDC cohort manifest to retrieve the manifest-defined cohort files is shown in [colab notebooks](https://github.com/ImagingDataCommons/IDC-Examples/tree/master/notebooks).

{% hint style="info" %}
A multipart file export can have a maximum of ten files with 65,000 rows each. If your export is larger than this, you must export the manifest via BigQuery.
{% endhint %}

JSON files do not include any header information but like CSV and TSV files, require that you identify which columns to include in your export. You can import the JSON file to a BigQuery table for further analysis.

## BigQuery cohort manifest

{% hint style="warning" %}
You must have a Google account to use BigQuery.
{% endhint %}

Exporting a manifest to BigQuery allows you to run complex, analytical, SQL-based queries on large sets of data by combining metadata available in IDC BigQuery tables with the content of your manifest. After you start the export, the following window appears on the cohort details page, showing your unique link.

![](../.gitbook/assets/manifest-job.png)

The exported cohort manifest table is intended for short-term use, and **will be deleted after seven days**. However, you can always re-export the cohort manifest.
