# IDC Data Model Concepts

The IDC API is based on several IDC Data Model Concepts.

## Cohort

"_In statistics, marketing and demography, a **cohort** is a group of_ [_subjects_](https://en.wikipedia.org/wiki/Research_subject) _who share a defining characteristic \(typically subjects who experienced a common event in a selected time period, such as birth or graduation\)._" \([Wikipedia](https://en.wikipedia.org/wiki/Cohort_%28statistics%29)\)

In IDC, a _cohort_ is a set of subjects \(DICOM patients\) that are identified by applying a **Filter** **Set** to the **Data** **Sources** of some **IDC** **data** **version**. Because a _cohort_ is defined with respect to an _IDC data version_, the set of subjects in the _cohort_, as well as all metadata associated with those subjects_,_ is exactly and repeatably defined.

## IDC Data Version

Over time, the set of data hosted by the IDC will change. For the most part, such changes will be due to new data having been added. The totality of IDC hosted data resulting from any such change is represented by a unique _IDC data version_ ID. That is, each time that the set of publicly available data changes, a new IDC version is created that exactly defines the revised data set.

The _IDC data version_ is intended to enable the reproducibility of research results. For example, consider a patient in the DICOM data model. Over time, new studies might be performed on a patient and become associated with that patient, and the corresponding DICOM instances added to the IDC hosted data. Moreover, additional patients might well be added to the IDC data set over time. This means that the set of subjects defined by some filtering operation will change over time. Thus, for purposes of reproducibility, we define a _cohort_ in terms of a set of _filter groups_ and an _IDC data version_.

{% hint style="info" %}
IDC cohort is uniquely defined by the combination of a set of _filter groups_ and an _IDC data version_.
{% endhint %}

Note that on occasion some data might be removed from a collection, though this is expected to be rare. Such a removal will result in a new _IDC data version_ which excludes that data. Such removed data will, however, continue to be available in any previous _IDC data version_ in which it was available. There is one exception: data that is found to contain Personally Identifiable Information \(PII\) or Protected Health Information \(PHI\) will be removed from all _IDC data versions_.

Note: currently _cohort_ is always defined in terms of a single _filter group_ and an _IDC Data Version_. In the future we may add support for multiple filter groups.

## Filter Group

A _filter group_ selects some set of subjects in the IDC hosted data, and is a set of conditions, where each condition is defined by an **attribute** and an array of values. An attribute identifies a field \(column\) in some data source \(BQ table\). Each _filter group_ also specifies the _IDC data version_ upon which it operates.

A _filter group_ selects a subject if, for every _attribute_ in the _filter group_, some datum associated with the subject satisfies one or more of the values in the associated array of values. A datum satisfies a value if it is equal to, less than, less than or equal to, between, greater than or equal to, or greater than, as required by the _attribute_. This is explained further below.

For example, the \(attribute, \[values\]\) pair \(Modality, \[MR, CT\]\) is satisfied if a subject "has" a Modality of MR or CT in any data associated with that subject. For example, it is satisfied if the subject "has" an MR series _or_ a CT series. Thus this \(attribute, \[values\]\) pair would be satisfied, for example, by a subject who has one or more MR series but no CT series.

Note that if a _filter group_ includes more than one \(attribute, \[values\]\) pair having the same attribute, then only the last such \(attribute, \[values\]\) pair is used. Thus if a _filter group_ includes the \(attribute, \[values\]\) pairs \(Modality, \[MR\]\) and \(Modality, \[CT\]\), in that order, only \(Modality, \[CT\]\) is used.

Here is an example _filter group_:

```text
  {
    "idc_data_version": "1.0",
    "filters": {
      "collection_id": [
        "TCGA-LUAD",
        "TCGA-KIRC"
      ],
      "Modality": [
        "CT",
        "MR"
      ],
      "race": [
        "WHITE"
      ],
      "age_at_diagnosis_btw": [
        53, 69
      ]
    }
```

This _filter group_ will select subjects in both the TCGA-LUAD and TCGA-KIRC collections, if the subject has any DICOM instances having a modality of CT or MR, the subject's race is WHITE, and the subjects age at diagnosis is between 53 and 69.

## Collections

A _collection_ is a set of DICOM data provided by a single source. _Collections_ are further categorized as **Original** _collections_ or **Analysis** _collections_. Original _collections_ are comprised primarily of DICOM image data that was obtained from some set of patients. Typically, the patients in an Original _collection_ are related by a common disease.

Analysis _collections_ are comprised of DICOM data that was generated by analyzing other \(typically Original\) _collections._ Typically such analysis is performed by a different entity than that which provided the original _collection\(s\)_ on which the analysis is based. Examples of data in analysis _collections_ include segmentations, annotations and further processing of original images. Note that some Original _collections_ include such data, though most of the data in Original collections are original images.

## Data Source

A data source is a BQ table that contains some part of the IDC metadata complement. API queries are performed against one or more such tables that are joined \(in the relational database model sense\). Data sources are classified as being of type Original, Derived or Related. **Original** _data sources_ contain DICOM metadata from the DICOM objects in TCIA Original and TCIA Analysis collections. **Derived** _data sources_ contain processed data: in general this as analytical data has been processed to enable easier SQL searches. **Related** _data sources_ contain ancillary data that may be specific to some set of collections. For example, TCGA biospecimen and clinical data are maintained in such tables.

_Data sources_ are versioned. That is, when the data in a _data source_ changes, a new version of that set of data is defined. An _IDC data version_ is defined in terms of a specific version of each data source. Note that over time, new data sources may be added \(or, less likely, removed\). Thus two _IDC data versions_ may have a different number of data sources.

## Attribute

Both the IDC Web App and API expose selected fields in the various _data sources_ against which queries can be performed. Each _attribute_ has a data type, one of:

* String An _attribute_ with data type String may have an arbitrary string value. For example, the possible values of a StudyDescription _attribute_ are arbitrary. When the values array of a \(String attribute, \[values\]\) pair contains a single value, an SQL LIKE operator is used and standard SQL syntax and semantics are supported. Thus a \('StudyDescription",\["%SKULL%"\]\) will match any StudyDescription that contains "SKULL",  When the values array of a \(String attribute, \[values\]\) pair contains more that one value, an SQL IN UNNEST operator is used and standard SQL syntax and semantics are supported. See the [Google BigQuery](https://cloud.google.com/bigquery/docs/reference/standard-sql/operators#comparison_operators) documentation for details.
* Categorical String An _attribute_ with data type Categorical String will have one of a defined set of string values. For example, Modality is an _attribute_ , and has possible values 'CT', 'MR', 'SR', etc. In this case, the values are defined by the DICOM specification. The defined values of other Categorical String attribute in may be established by other entities. When the values array of a \(Categorical String attribute, \[values\]\) pair contains a single value, an SQL LIKE operator is used and standard SQL syntax and semantics are supported. Thus a \('StudyDescription",\["%SKULL%"\]\) will match any StudyDescription that contains "SKULL",  When the values array of a \(Categorical String attribute, \[values\]\) pair contains more that one value, an SQL IN UNNEST operator is used and standard SQL syntax and semantics are supported. See the [Google BigQuery](https://cloud.google.com/bigquery/docs/reference/standard-sql/operators#comparison_operators) documentation for details.
* Continuous Numeric An _attribute_ with data type Continuous Number will have a numeric \(float\) value. For example, age\_at\_diagnosis is an _attribute_ of data type Continuous Numeric.  In order to enable relative numeric queries, the API exposes 6 variations of each Continuous Numeric attributes as filter set _attribute_ names. These variations are the base _attribute_ name with no suffix, as well as the base _attribute_ name with one of the suffixes: \__gt, \_gte, \_btw, \_lte, \_lt._ For all variations except _btw,_ the corresponding value array must have a single numeric value, _\_while the \_btw variation requires exactly two values in the value array \(order does not matter\). The \(attribute, value array\) pair for a Continuous Numeric \_attribute_ is satisfied according to the suffix as follows:
  * &lt;no suffix&gt;: If an _attribute_ is equal to the value in the value array
  * gt: If an _attribute_ is greater than the value in the value array
  * gte: If an _attribute_ is greater than or equal to the value in the value array
  * btw: if an _attribute_ is between \(inclusively\) the two values in the value array
  * lte: If an _attribute_ is less than or equal to the value in the value array
  * lt: If an _attribute_ is less than the value in the value array
* Categorical Numeric An _attribute_ with data type Categorical Numeric has one of a defined set of numeric values. The corresponding value array must have a single numeric value.  There are currently no _attributes_ having this data type.

## Manifest

A _manifest_ is a list of access methods of the data objects corresponding to the objects in some _cohort_. There are two types of access methods:

* GUID

  A GUID is a persistent identifier that can be resolved to a [GA4GH DRS object.](https://ga4gh.github.io/data-repository-service-schemas/preview/release/drs-1.0.0/docs/) GUID persistence ensures that the data which the GUID represents can continue to be located and accessed even if it has been moved to a different hosting site.  
  A GUID identifies a particular version of an IDC data object, and there is a GUID for every version of every DICOM instance, series, and study in IDC hosted data.  
  GUID are issued by the NCI Cancer Research Data Commons. This is a typical CRDC GUID:  
  dg.4DFC/83fdfb25-ad87-4879-b0f3-b9850ef0b216  
  A GUID can be resolved at [https://nci-crdc.datacommons.io/ga4gh/drs/v1/objects/](https://nci-crdc.datacommons.io/ga4gh/drs/v1/objects/) by appending the UUID to the that URL. E.G. \(formatting added to the curl response for clarity\):

  `>> curl https://nci-crdc.datacommons.io/ga4gh/drs/v1/objects/bd68332e-521f-4c45-9a88-e9cc426f5a8d    
  {    
  "access_methods":[`

* `{ "access_id":"gs", "access_url":{ "url":"gs://idc-open/bd68332e-521f-4c45-9a88-e9cc426f5a8d.dcm" }, "region":"", "type":"gs" } ], "aliases":[ ], "checksums":[ { "checksum":"9a63c81a4b3b4bc3950678a4e9acc930", "type":"md5" } ], "contents":[ ], "created_time":"2021-08-27T21:15:02.385181", "description":null, "form":"object", "id":"dg.4DFC/bd68332e-521f-4c45-9a88-e9cc426f5a8d", "mime_type":"application/json", "name":"", "self_uri":"drs://nci-crdc.datacommons.io/dg.4DFC/bd68332e-521f-4c45-9a88-e9cc426f5a8d", "size":528622, "updated_time":"2021-08-27T21:15:02.385185", "version":"faf7385b" }` [ ](https://nci-crdc.datacommons.io/ga4gh/drs/v1/objects/)Resolving such a GUID returns a DRSObject. The access methods in the returned DRSObject include one or more URLs at which corresponding DICOM entities can be accessed. GUID manifests are recommended for long term archival and reference.
* URL  
  The URLs in a URL based _manifest_ can be used to directly access a DICOM instance in Google Cloud Storage. URLs are structured as follows:  
  `gs://<GCS bucket>/<GUID>.dcm`

  This is a typical URL:  
  `gs://idc-open/bd68332e-521f-4c45-9a88-e9cc426f5a8d.dcm`

  The URL of some object can change over time. For instance if an object is replaced by a newer version, the previous version will be moved to a new GCS bucket. The corresponding DRSObject will be updated with new URL. However, the original URL will then be "stale". Therefore manifests are recommended when the objects in the manifest will be accessed "soon".

  Note: Some URLs currently have the format :  
  `gs://<GCS-bucket>/<study_uid>/<series_uid>/<instance_uid>.dcm#<gcs version>`

  The URLS of these instances will change to the above format. At that time, the corresponding GUIDs will resolve to DrsObjects that contain the new URL.

* Additional values can optionally be included in the returned manifest. See the manifest API descriptions for more details.

## **IDC API UI**

The [IDC API UI](https://api.imaging.datacommons.cancer.gov/v1/swagger) can be used to see details about the syntax for each call, and also provides an interface to test requests.

## Authenticating to the UI

Some of the API calls require authentication. This is denoted by a small lock symbol. Authentication can be performed by clicking on the ‘Authorize’ button at the top right of the page.

## Make a Request

For a quick demonstration of the syntax of an API call, test the [GET/collections](https://api.imaging.datacommons.cancer.gov/v1/collections) request. You can experiment with this endpoint by clicking the ‘Try it out’ button.

This API takes an optional _idc\_data\_version_ query parameter. In the UI, this parameter defaults to the empty string. You can change this values or simply leave the default. If you leave the defaults, the API will return collection metadata for the current IDC data version. The request can be run by selecting ‘Execute’.

**Request Response**

The Swagger UI submits the request and shows the curl code that was submitted. The ‘Response body’ section will display the response to the request. The expected format of the response to this API request is shown below:

```text
{
  "collections": [
    {
      "active": "string",
      "cancer_type": "string",
      "collection_id": "string",
      "date_updated": "string",
      "description": "string",
      "doi": "string",
      "image_types": "string",
      "location": "string",
      "species": "string",
      "subject_count": 0,
      "supporting_data": "string",
      "idc_data_versions": [
        "string"
      ]
    }
  ],
  "code": 0
}
```

The actual JSON formatted response can be downloaded by selecting the ‘Download’ button.

The syntax for all of API data structures is detailed at the bottom of the UI page.

