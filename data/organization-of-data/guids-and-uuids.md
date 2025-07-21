# UUIDs and GUIDs

As described in the [Data Versioning](../data-versioning.md) section, a UUID identifies a particular version of an IDC data object. Thus, there is a UUID for every version of every DICOM instance in IDC hosted data. An IDC BigQuery manifest optionally includes the UUID (called a crdc\_instance\_uuid) of each instance (version) in the cohort.

### UIDs and UUIDs explained with an example

Consider an instance in the CPTAC-CM collection that has this `SOPInstanceUID`: `1.3.6.1.4.1.5962.99.1.171941254.777277241.1640849481094.35.0`\\

It is in a series having this `SeriesInstanceUID`:\
`1.3.6.1.4.1.5962.99.1.171941254.777277241.1640849481094.2.0`

The instance and series were added to the IDC Data set in IDC version 7. At that point, the instance was assigned UUID:\
`5dce0cf0-4694-4dff-8f9e-2785bf179267`\
and the series was assigned this UUID:\
`e127d258-37c2-47bb-a7d1-1faa7f47f47a`

In IDC version 10, a revision of this instance was added (keeping its original `SOPInstanceUID`), and assigned this UUID:\
`21e5e9ce-01f5-4b9b-9899-a2cbb979b542`

Because this instance was revised, the series containing it was implicitly revised. The revised series was thus issued a new UUID:\
`ee34c840-b0ca-4400-a6c8-c605cef17630`

Thus, the initial version of this instance has this file name:\
`e127d258-37c2-47bb-a7d1-1faa7f47f47a/5dce0cf0-4694-4dff-8f9e-2785bf179267.dcm`\
and the revised version of the instance has the this file name:\
`ee34c840-b0ca-4400-a6c8-c605cef17630/21e5e9ce-01f5-4b9b-9899-a2cbb979b542.dcm`

Both versions of the instance are in both AWS and GCS buckets.

{% code title="AWS bucket example" overflow="wrap" %}
```bash
s5cmd --no-sign-request ls s3://idc-open-data/e127d258-37c2-47bb-a7d1-1faa7f47f47a/5dce0cf0-4694-4dff-8f9e-2785bf179267.dcm
2023-04-09 11:49:55    3308170 5dce0cf0-4694-4dff-8f9e-2785bf179267.dcm
```
{% endcode %}

{% code title="GCS bucket example" overflow="wrap" %}
```bash
s5cmd --no-sign-request --endpoint-url https://storage.googleapis.com ls s3://public-datasets-idc/e127d258-37c2-47bb-a7d1-1faa7f47f47a/5dce0cf0-4694-4dff-8f9e-2785bf179267.dcm
   3308170  2023-04-01T01:21:31Z  gs://public-datasets-idc/e127d258-37c2-47bb-a7d1-1faa7f47f47a/5dce0cf0-4694-4dff-8f9e-2785bf179267.dcm
TOTAL: 1 objects, 3308402 bytes (3.16 MiB)
```
{% endcode %}

Note that GCS and AWS bucket names are different. In fact, DICOM instance data is distributed across multiple buckets in both GCS and AWS. We will discuss obtaining GCS and AWS URLs more a little later.

{% hint style="info" %}
It is possible that a series is revised, but one or more instances in the series are not revised. For example if a single instance in a series (assume the series has a uuid \<series\_uuid\_old>) is revised, that instance gets a new UUID, and there is implicitly a new version of the series, which gets a new UUID (call it \<series\_uuid\_new>). If an instance that is not revised has UUID \<invariant\_instance\_uuid>, then its corresponding file in cloud storage will the have name:\
`<series_uuid_old>/<invariant_instance_uuid>.dcm` in the "old" series. But, because that same instance version is in the revised series, there must also be a file in cloud storage named:\
`<series_uuid_new>/<invariant_instance_uuid>.dcm`\
The result will be two distinct but identical files.
{% endhint %}

Utilities like gsutil, s3 and s5cmd "understand" the implied hierarchy in these file names. Thus the series UUID now acts like the name of a directory that contains all the instance versions in the series version:

{% code overflow="wrap" %}
```bash
s5cmd --no-sign-request --endpoint-url https://storage.googleapis.com ls s3://public-datasets-idc/ee34c840-b0ca-4400-a6c8-c605cef17630/
2023/04/01 03:00:34           1719696 18c206a6-2db4-45cd-89a2-e83273a38f42.dcm
2023/04/01 03:00:36           3308402 21e5e9ce-01f5-4b9b-9899-a2cbb979b542.dcm
2023/04/01 01:50:29          29477804 3cfc3da3-8389-49f6-a6ee-6ba6406f639e.dcm
2023/04/01 01:50:27         214715792 428590a0-816c-4041-a3ae-676a68411794.dcm
2023/04/01 03:00:30           2301902 57ff4432-c29d-4ccf-964c-0b421302add3.dcm
2023/04/01 03:00:33           3540080 77ff406a-a236-4846-83dd-ae3bd7a6bc71.dcm
```
{% endcode %}

and similarly for AWS buckets, thus making it easy to transfer all instances in a series from the cloud.

Because file names are more or less opaque, the user will not typically select files by listing the contents of a bucket. Instead, one should use either the IDC Portal or IDC BigQuery tables to identify items of interest and, then, generate a manifest of objects that can be passed to a utility like s5cmd.

### Resolving CRDC Globally Unique Identifiers (GUIDs)

{% hint style="info" %}
From the [GA4GH Data Repository Service API](https://ga4gh.github.io/data-repository-service-schemas/preview/release/drs-1.0.0/docs/#_drs_ids) specification:

"The Data Repository Service (DRS) API provides a generic interface to data repositories so data consumers, including workflow systems, can access data objects in a single, standard way regardless of where they are stored and how they are managed. The primary functionality of DRS is to map a logical ID to a means for physically retrieving the data represented by the ID."
{% endhint %}

Each such UUID can be used to form a [`DRS ID`](https://ga4gh.github.io/data-repository-service-schemas/preview/release/drs-1.0.0/docs/#_drs_ids) that has been indexed by the [NCI CRDC Data Commons Framework](https://dcf.gen3.org/) (DCF), and can be used to access data that defines that object. In particular this data includes the GCS and AWS URLs of the DICOM instance file. Though the GCS or AWS URL of an instance might change over time, the UUID of an instance can always be resolved to obtain its current URLs. Thus, for long term curation of data, it is recommended to record instance UUIDs.

The data object returned by the server is a GA4GH DRS [`DrsObject`](https://ga4gh.github.io/data-repository-service-schemas/preview/release/drs-1.0.0/docs/#_drs_ids)`:`

This is a typical IDC instance UUID:\
`641121f1-5ca0-42cc-9156-fb5538c14355`\
of a (version of a) DICOM instance, and this is the corresponding DRS ID:\
`dg.4DFC/641121f1-5ca0-42cc-9156-fb5538c14355`

A DRS ID can be resolved by appending it to the following URL, which is the resolution service within CRDC: `https://nci-crdc.datacommons.io/ga4gh/drs/v1/objects/` . For example, the following `curl` command:

`>> curl https://nci-crdc.datacommons.io/ga4gh/drs/v1/objects/dg.4DFC/641121f1-5ca0-42cc-9156-fb5538c14355`

returns this DrsObject:

```
{
  "access_methods": [
    {
      "access_id": "gs",
      "access_url": {
        "url": "gs://public-datasets-idc/cc9c8541-949d-48d9-beaf-7028aa4906dc/641121f1-5ca0-42cc-9156-fb5538c14355.dcm"
      },
      "region": "",
      "type": "gs"
    },
    {
      "access_id": "s3",
      "access_url": {
        "url": "s3://idc-open-data/cc9c8541-949d-48d9-beaf-7028aa4906dc/641121f1-5ca0-42cc-9156-fb5538c14355.dcm"
      },
      "region": "",
      "type": "s3"
    }
  ],
  "aliases": [],
  "checksums": [
    {
      "checksum": "f338e8c5e3d8955d222a04d5f3f6e2b4",
      "type": "md5"
    }
  ],
  "created_time": "2020-06-01T00:00:00",
  "description": "DICOM instance",
  "form": "object",
  "id": "dg.4DFC/641121f1-5ca0-42cc-9156-fb5538c14355",
  "index_created_time": "2023-06-26T18:27:45.810110",
  "index_updated_time": "2023-06-26T18:27:45.810110",
  "mime_type": "application/json",
  "name": "1.3.6.1.4.1.14519.5.2.1.7695.1700.277743171070833720282648319465",
  "self_uri": "drs://dg.4DFC:641121f1-5ca0-42cc-9156-fb5538c14355",
  "size": 135450,
  "updated_time": "2020-06-01T00:00:00",
  "version": "IDC version: 1"
}
```

AS can be seen, the `access_methods` component in the returned DrsObject includes a URL for each of the corresponding files in Google GCS and AWS S3.



