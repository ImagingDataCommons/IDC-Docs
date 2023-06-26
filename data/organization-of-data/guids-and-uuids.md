# Resolving CRDC Globally Unique Identifiers (GUIDs)

An IDC manifest may include study, series, and/or instance UUIDs that can be resolved to the underlying DICOM instance files in GCS. Such use of UUIDs in a manifest enables a much shorter manifest compared to a list of per-instance GCS URLs. Also, as explained below, a UUID is expected to be resolvable even when the data which it represents has been moved.

{% hint style="info" %}
From the [GA4GH Data Repository Service API](https://ga4gh.github.io/data-repository-service-schemas/preview/release/drs-1.0.0/docs/#\_introduction) specification:

"The Data Repository Service (DRS) API provides a generic interface to data repositories so data consumers, including workflow systems, can access data objects in a single, standard way regardless of where they are stored and how they are managed. The primary functionality of DRS is to map a logical ID to a means for physically retrieving the data represented by the ID."
{% endhint %}

In IDC, we use the term _GUID_ to mean a persistent identifier that can be resolved to a GA4GH DrsObject. GUID persistence ensures that the data which the GUID represents can continue to be located and accessed even if it has been moved to a different hosting site.

As described in the [Data Versioning](../data-versioning.md) section, a UUID identifies a particular version of an IDC data object. There is a UUID for every version of every DICOM instance, series, and study in IDC hosted data. Each such UUID can be used to form a GUID that is registered by the NCI Cancer Research Data Commons (CRDC), and can be used to access the data that defines that object.

This is a typical UUID:\
`641121f1-5ca0-42cc-9156-fb5538c14355`\
of a (version of a) DICOM instance, and this is the corresponding CRDC GUID:\
`dg.4DFC/641121f1-5ca0-42cc-9156-fb5538c14355`

A UUID can be resolved by appending it to this URL, which is the GUID resolution service within CRDC: `https://nci-crdc.datacommons.io/ga4gh/drs/v1/objects/` . For example, the following `curl` command:

`>> curl https://nci-crdc.datacommons.io/ga4gh/drs/v1/objects/dg.4DFC/641121f1-5ca0-42cc-9156-fb5538c14355`

returns this DrsObject:

<pre><code>{
   "access_methods":[
      {
         "access_id":"gs",
         "access_url":{
            "url":"gs://public-datasets-idc/cc9c8541-949d-48d9-beaf-7028aa4906dc/641121f1-5ca0-42cc-9156-fb5538c14355.dcm"
         },
         "region":"us",
         "type":"gs"
      },
      {
         "access_id":"s3",
         "access_url":{
            "url":"s3://idc-open-data/cc9c8541-949d-48d9-beaf-7028aa4906dc/641121f1-5ca0-42cc-9156-fb5538c14355.dcm"
         },
         "region":"us-east-1",
         "type":"s3"
      }
<strong>   ],
</strong>   "aliases":[

   ],
   "checksums":[
      {
         "checksum":"f338e8c5e3d8955d222a04d5f3f6e2b4",
         "type":"md5"
      }
   ],
   "contents":[

   ],
   "created_time":"2020-09-18T02:14:02.830862",
   "description":"DICOM instance",
   "form":"object",
   "id":"dg.4DFC/641121f1-5ca0-42cc-9156-fb5538c14355",
   "mime_type":"application/json",
   "name":"1.3.6.1.4.1.14519.5.2.1.7695.1700.277743171070833720282648319465",
   "self_uri":"drs://nci-crdc.datacommons.io/dg.4DFC/641121f1-5ca0-42cc-9156-fb5538c14355",
   "size":"135450",
   "updated_time":"2020-09-18T02:14:02.830868",
   "version":"idc_v1"
}
</code></pre>

The `access_methods` component in the returned DrsObject includes a URL for each of the corresponding files in Google GCS and AWS S3.

{% hint style="info" %}
Prior to IDC version V14, IDC data was  available only from Google GCS, and instance URLs were of the form gs://\<bucket\_name>/\<instance\_uuid>.dcm (which we refer to as the flat namespace). Therefore, the DrsObject that was returned when resolving an instance GUID had a single access\_method that had a URL with a flat file name.

The IDC and DCF are currently revising the DrsObjects returned for IDC instances to comprehend the new naming convention and AWS deployment. In the meantime. resolving an instance GUID may return a DrsObject having a single GCS access\_method in which the URL includes a flat file name. IDC will keep those files having a flat file name available until after the DrsObject revisioning is complete.
{% endhint %}

{% hint style="warning" %}
The [GA4GH Cloud Work Stream](https://www.ga4gh.org/work\_stream/cloud/) is expected to deprecate DRS bundle functionality. IDC had initially been registering DICOM series and studies as DRS bundles. However, due the impending deprecation, IDC has only registered series and studies in IDC V1. Therefore, most series and study UUIDs cannot be resolved to DrsObjects, and the following discussion is therefore mostly irrelevant at this time.

IDC and other CRDC entities are working to define a replacement for DRS bundles.
{% endhint %}

When the GUID of a series is resolved, the DrsObject that is returned does not include access methods because there are no series file objects. Instead, the `contents` component of the returned DrsObject contains the URLs that can be accessed to obtain the DrsObjects of the instances in the series.

Thus, when we resolve `cc9c8541-949d-48d9-beaf-7028aa4906dc`, the UUID of the series containing the instance above:

`curl -o foo https://nci-crdc.datacommons.io/ga4gh/drs/v1/objects/dg.4DFC/cc9c8541-949d-48d9-beaf-7028aa4906dc`

the `contents` component includes the GUID of that instance as well as the GUID of another instance:

```
{
   "aliases":[

   ],
   "checksums":[
      {
         "checksum":"0512207cb222fa2f085bc110c8474fa2",
         "type":"md5"
      }
   ],
   "contents":[
      {
         "drs_uri":"drs://nci-crdc.datacommons.io/dg.4DFC/ccafd781-ef39-4d39-ad74-e09de1ada476",
         "id":"dg.4DFC/ccafd781-ef39-4d39-ad74-e09de1ada476",
         "name":null
      },
      {
         "drs_uri":"drs://nci-crdc.datacommons.io/dg.4DFC/641121f1-5ca0-42cc-9156-fb5538c14355",
         "id":"dg.4DFC/641121f1-5ca0-42cc-9156-fb5538c14355",
         "name":null
      }
   ],
   "created_time":"2020-12-04T19:11:58.072088",
   "description":"",
   "form":"bundle",
   "id":"dg.4DFC/cc9c8541-949d-48d9-beaf-7028aa4906dc",
   "mime_type":"application/json",
   "name":"dg.4DFCcc9c8541-949d-48d9-beaf-7028aa4906dc",
   "self_uri":"drs://nci-crdc.datacommons.io/dg.4DFC/cc9c8541-949d-48d9-beaf-7028aa4906dc",
   "size":270902,
   "updated_time":"2020-12-04T19:11:58.072094",
   "version":""
}
```

Similarly, the GUID of a DICOM study resolves to a DrsObject whose `contents` component consists of the GUIDs of the series in that study.
