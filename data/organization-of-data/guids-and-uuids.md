# Resolving CRDC Globally Unique Identifiers (GUIDs)

As described in the [Data Versioning](../data-versioning.md) section, a UUID identifies a particular version of an IDC data object. Thus, there is a UUID for every version of every DICOM instance in IDC hosted data. An IDC BigQuery manifest optionally includes the UUID (called a crdc\_instance\_uuid) of each instance (version) in the cohort.

{% hint style="info" %}
From the [GA4GH Data Repository Service API](https://ga4gh.github.io/data-repository-service-schemas/preview/release/drs-1.0.0/docs/#\_drs\_ids) specification:

"The Data Repository Service (DRS) API provides a generic interface to data repositories so data consumers, including workflow systems, can access data objects in a single, standard way regardless of where they are stored and how they are managed. The primary functionality of DRS is to map a logical ID to a means for physically retrieving the data represented by the ID."
{% endhint %}

Each such UUID can be used to form a [`DRS ID`](https://ga4gh.github.io/data-repository-service-schemas/preview/release/drs-1.0.0/docs/#\_drs\_ids) that has been indexed by the [NCI CRDC Data Commons Framework](https://dcf.gen3.org/) (DCF), and can be used to access data that defines that object. In particular this data includes the GCS and AWS URLs of the DICOM instance file. Though the GCS or AWS URL of an instance might change over time, the UUID of an instance can always be resolved to obtain its current URLs. Thus, for long term curation of data, it is recommended to record instance UUIDs.

The data object returned by the server is a GA4GH DRS [`DrsObject`](https://ga4gh.github.io/data-repository-service-schemas/preview/release/drs-1.0.0/docs/#\_drs\_ids)`:`

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



