# Resolving CRDC Globally Unique Identifiers \(GUIDs\)

An IDC manifest may include study and/or series _GUIDs_ that can be resolved to the underlying DICOM instance files in GCS. Such use of GUIDs in a manifest enables a much shorter manifest compared to a list of per-instance GCS URLs. Also, as explained below, a GUID is expected to be resolvable even when the data which it represents has been moved.

{% hint style="info" %}
From the [GA4GH Data Repository Service API](https://github.com/ImagingDataCommons/IDC-Docs-dev/tree/f90c4ff43658a6d016ca80352826c835aee38043/data-repository-service-schemas/preview/release/drs-1.1.0/docs/README.md) specification:

"The Data Repository Service \(DRS\) API provides a generic interface to data repositories so data consumers, including workflow systems, can access data objects in a single, standard way regardless of where they are stored and how they are managed. The primary functionality of DRS is to map a logical ID to a means for physically retrieving the data represented by the ID."
{% endhint %}

In IDC, we use the term _GUID_ to mean a persistent identifier that can be resolved to a GA4GH DrsObject. GUID persistence ensures that the data which the GUID represents can continue to be located and accessed even if it has been moved to a different hosting site. 

As described in the [Data Versioning](../data-versioning.md) section, a UUID identifies a particular version of an IDC data object. There is a UUID for every version of every DICOM instance, series, and study in IDC hosted data. Each such UUID can be used to form a GUID that is registered by the NCI Cancer Research Data Commons \(CRDC\), and can be used to access the data that defines that object. 

This is a typical UUID:  
`641121f1-5ca0-42cc-9156-fb5538c14355`  
of a \(version of a\) DICOM instance, and this is the corresponding CRDC GUID:  
`dg.4DFC/641121f1-5ca0-42cc-9156-fb5538c14355`

A GUID can be resolved by appending it to this URL, which is the GUID resolution service within CRDC: [https://nci-crdc.datacommons.io/ga4gh/drs/v1/objects/](https://nci-crdc.datacommons.io/ga4gh/drs/v1/objects/) . For example, the following `curl` command:

`>> curl https://nci-crdc.datacommons.io/ga4gh/drs/v1/objects/dg.4DFC/641121f1-5ca0-42cc-9156-fb5538c14355`

returns:

```text
{
   "access_methods":[
      {
         "access_id":"gs",
         "access_url":{
            "url":"gs://idc-open/641121f1-5ca0-42cc-9156-fb5538c14355.dcm"
         },
         "region":"",
         "type":"gs"
      }
   ],
   "aliases":[
      
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
   "description":null,
   "form":"object",
   "id":"dg.4DFC/641121f1-5ca0-42cc-9156-fb5538c14355",
   "mime_type":"application/json",
   "name":null,
   "self_uri":"drs://nci-crdc.datacommons.io/dg.4DFC/641121f1-5ca0-42cc-9156-fb5538c14355",
   "size":"135450",
   "updated_time":"2020-09-18T02:14:02.830868",
   "version":"9e13fb30"
}
which is a DrsObject. Because we resolved the GUID of an instance, the access_methods in the returned DrsObject includes a URL at which the corresponding DICOM entity can be accessed.
```

When the GUID of a series is resolved, the DrsObject that is returned does not include access methods because there are no series file objects. Instead, the `contents` component of the returned DrsObject contains the URLs that can be accessed to obtain the DrsObjects of the instances in the series.  
Thus, we see that when we resolve `dg.4DFC/cc9c8541-949d-48d9-beaf-7028aa4906dc`, the GUID of the series containing the instance above:

`curl -o foo https://nci-crdc.datacommons.io/ga4gh/drs/v1/objects/dg.4DFC/cc9c8541-949d-48d9-beaf-7028aa4906dc`

we see that the `contents` component includes the GUID of that instance as well as the GUID of another instance:

```text
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

{% hint style="warning" %}
At this time, most GUIDs have not been registered with the CRDC. If such a GUID is presented to the CRDC for resolution, an HTTP 404 error is returned.
{% endhint %}

{% hint style="warning" %}
As discussed in the _Organization of data_ section of this document, the DICOM instance file naming convention changed with IDC version 2. At this time, when an instance GUID is resolved, the returned DrsObject may method may include a URI to the V1 GCS bucket location. Those GUID will re-indexed such that in the future they point to the new GCS bucket location.
{% endhint %}

