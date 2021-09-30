# Endpoint Details

This section provides documentation on each endpoint of IDC API.

{% api-method method="get" host=" " path="https://api.imaging.datacommons.cancer.gov/v1/about" %}
{% api-method-summary %}
about
{% endapi-method-summary %}

{% api-method-description %}
The **about** endpoint returns a short greeting and pointers to both the demo UI and to .
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}
 API Description and link to SwaggerUI interface.
{% endapi-method-response-example-description %}

```text
{
  "message": "string",
  "code": 0,
  "not_found": [
    "string"
  ]
}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=500 %}
{% api-method-response-example-description %}
 Server error
{% endapi-method-response-example-description %}

```
{
  "message": "string",
  "code": 0,
  "not_found": [
    "string"
  ]
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% api-method method="get" host=" " path="https://api.imaging.datacommons.cancer.gov/v1/versions" %}
{% api-method-summary %}
versions
{% endapi-method-summary %}

{% api-method-description %}
Get a list of IDC data versions and per-version data model metadata, including the data sources of which each IDC data version is comprised.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}
A list of IDC data versions. For each IDC data version, the data sources and programs associated with that version are listed.
{% endapi-method-response-example-description %}

```
{
  "idc_data_versions": [
    {
      "idc_data_version": "string",
      "date_active": "string",
      "active": true,
      "data_sources": [
        {
          "name": "string",
          "data_type": "Image Data"
        }
      ],
      "programs": [
        [
          {
            "short_name": "string",
            "name": "string",
            "description": "string"
          }
        ]
      ]
    }
  ],
  "code": 0
}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=500 %}
{% api-method-response-example-description %}
Server error
{% endapi-method-response-example-description %}

```
{
  "message": "string",
  "code": 0,
  "not_found": [
    "string"
  ]
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% api-method method="get" host=" " path="https://api.imaging.datacommons.cancer.gov/v1/collections" %}
{% api-method-summary %}
collections
{% endapi-method-summary %}

{% api-method-description %}
 Get list of collections for some IDC data version
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-query-parameters %}
{% api-method-parameter name="idc\_data\_version" type="string" required=false %}
The IDC data version for which collection data is to be returned. If not specified, collection data for the current IDC data version is returned.
{% endapi-method-parameter %}
{% endapi-method-query-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}
A list of the collections, with related metadata.
{% endapi-method-response-example-description %}

```
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
{% endapi-method-response-example %}

{% api-method-response-example httpCode=400 %}
{% api-method-response-example-description %}
Bad request
{% endapi-method-response-example-description %}

```
{
  "message": "string",
  "code": 0,
  "not_found": [
    "string"
  ]
}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=500 %}
{% api-method-response-example-description %}
Server error
{% endapi-method-response-example-description %}

```
{
  "message": "string",
  "code": 0,
  "not_found": [
    "string"
  ]
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% api-method method="get" host=" " path="https://api.imaging.datacommons.cancer.gov/v1/analysis\_results" %}
{% api-method-summary %}
analysis\_results
{% endapi-method-summary %}

{% api-method-description %}
Get a list of analysis results for some IDC data version
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-query-parameters %}
{% api-method-parameter name="idc\_data\_version" type="string" required=false %}
The IDC data version for which analysis results are to be returned. If not specified, analysis results for the current IDC data version are returned.
{% endapi-method-parameter %}
{% endapi-method-query-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}
A list of analysis results metadata.
{% endapi-method-response-example-description %}

```
{
  "analysisResults": [
    {
      "active": "string",
      "analysisArtifacts": "string",
      "cancer_type": "string",
      "collections": "string",
      "date_updated": "string",
      "description": "string",
      "doi": "string",
      "location": "string",
      "subject_count": 0,
      "idc_data_versions": [
        "string"
      ]
    }
  ]
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% api-method method="get" host=" " path="https://api.imaging.datacommons.cancer.gov/v1/attributes" %}
{% api-method-summary %}
attributes 
{% endapi-method-summary %}

{% api-method-description %}
Get a list of filter attributes 
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-query-parameters %}
{% api-method-parameter name="idc\_data\_version" type="string" required=false %}
The IDC data version for which attribute data is to be returned. If not specified, attribute data for the current IDC data version is returned.
{% endapi-method-parameter %}

{% api-method-parameter name="data\_source" type="string" required=false %}
The data source whose attributes are to be returned. If not specified, the attributes for all data sources are returned.
{% endapi-method-parameter %}
{% endapi-method-query-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}
A list of attribute metadata
{% endapi-method-response-example-description %}

```
{
  "data_sources": [
    {
      "name": "string",
      "attributes": [
        {
          "name": "string",
          "data_type": "CONTINUOUS_NUMERIC", "CATEGORICAL_NUMERIC", "CATEGORICAL", "TEXT", "STRING"]
          "units": "string",
          "idc_data_version": "string"
        }
      ]
    }
  ]
}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=400 %}
{% api-method-response-example-description %}
Bad request
{% endapi-method-response-example-description %}

```
{
  "message": "string",
  "code": 0,
  "not_found": [
    "string"
  ]
}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=500 %}
{% api-method-response-example-description %}
Server error
{% endapi-method-response-example-description %}

```
{
  "message": "string",
  "code": 0,
  "not_found": [
    "string"
  ]
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% api-method method="get" host=" " path="https://api.imaging.datacommons.cancer.gov/v1/cohorts" %}
{% api-method-summary %}
cohorts
{% endapi-method-summary %}

{% api-method-description %}
Get metadata on the user's cohorts, including cohorts created through the web app. 
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}
A list of cohort metadata
{% endapi-method-response-example-description %}

```
{
  "cohorts": [
    {
      "cohort_id": 0,
      "name": "string",
      "description": "string",
      "owner": "string",
      "permission": "OWNER"
    }
  ]
}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=500 %}
{% api-method-response-example-description %}
Server error
{% endapi-method-response-example-description %}

```
{
  "message": "string",
  "code": 0,
  "not_found": [
    "string"
  ]
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% api-method method="post" host=" " path="https://api.imaging.datacommons.cancer.gov/v1/cohorts" %}
{% api-method-summary %}
cohorts
{% endapi-method-summary %}

{% api-method-description %}
Create a cohort as defined by the specified filterSet and IDC data version.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-body-parameters %}
{% api-method-parameter name="cohort\_def" type="object" required=true %}
Structure, including filterSet and IDC data version, defining the cohort. Example:  
`{  
  "name": "mycohort",  
  "description: "Example description",  
  "filterSet": {  
    "idc_data_version": "1.0"  
    "filters": {  
      "collection_id": [  
        "TCGA-LUAD",  
        "TCGA_KIRC"  
      ],  
      "Modality": [  
        "CT",  
        "MR"  
      ],  
      "race": [  
        "WHITE"  
      ]  
    }  
  }  
}`  
 
{% endapi-method-parameter %}
{% endapi-method-body-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}
Response includes the cohort's name and description, and the filterSet and IDC data version that define the cohort. The cohort\_id is used to refer to this cohort in other APIs that take a cohort\_id parameter. The response to the example request is like:
{% endapi-method-response-example-description %}

```
{
  "code": 200,
  "cohort_id": 979,
  "description": "Example description",
  "filterSet": {
    "filters": {
      "Modality": [
        "CT",
        "MR"
      ],
      "collection_id": [
        "TCGA-LUAD",
        "TCGA-KIRC"
      ],
      "race": [
        "WHITE"
      ]
    },
    "idc_data_version": "1.0"
  },
  "name": "mycohort"
}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=400 %}
{% api-method-response-example-description %}
Bad request
{% endapi-method-response-example-description %}

```
{
  "message": "string",
  "code": 0,
  "not_found": [
    "string"
  ]
}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=500 %}
{% api-method-response-example-description %}
Server error
{% endapi-method-response-example-description %}

```
{
  "message": "string",
  "code": 0,
  "not_found": [
    "string"
  ]
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% api-method method="delete" host="  " path="https://api.imaging.datacommons.cancer.gov/v1/cohorts" %}
{% api-method-summary %}
cohorts
{% endapi-method-summary %}

{% api-method-description %}

{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-body-parameters %}
{% api-method-parameter name="cohort\_list" type="object" required=true %}
A list of the user's cohorts to be deleted. Schema:  
`{  
  "cohorts: [  
    "string"  
  ]  
}`  
    {
{% endapi-method-parameter %}
{% endapi-method-body-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}
A list of each cohort to be deleted and the result, indicating that the cohort was deleted, or could not be deleted for some specified reason.
{% endapi-method-response-example-description %}

```
  "cohorts": [
    {
      "cohort_id": 0,
      "result": "string"
    }
  ]
}
500	

```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=400 %}
{% api-method-response-example-description %}
Bad request
{% endapi-method-response-example-description %}

```
{
  "message": "string",
  "code": 0,
  "not_found": [
    "string"
  ]
}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=500 %}
{% api-method-response-example-description %}
Server error
{% endapi-method-response-example-description %}

```
{
  "message": "string",
  "code": 0,
  "not_found": [
    "string"
  ]
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% api-method method="delete" host=" " path="https://api.imaging.datacommons.cancer.gov/v1/cohorts/{cohort\_id}" %}
{% api-method-summary %}
cohort/{cohort\_id}
{% endapi-method-summary %}

{% api-method-description %}
Delete a cohort
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="cohort\_id" type="integer" required=true %}
The cohort to be deleted
{% endapi-method-parameter %}
{% endapi-method-path-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}
If the cohort was successly deleted, the response includes the filterSet that defined the cohort. If the cohort was not successfully deleted, the response indicates the reason it was not deleted.
{% endapi-method-response-example-description %}

```
{
  "cohorts": [
    {
      "cohort_id": 0,
      "result": "string"
    }
  ]
}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=302 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```

```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=400 %}
{% api-method-response-example-description %}
Bad request
{% endapi-method-response-example-description %}

```
{
  "message": "string",
  "code": 0,
  "not_found": [
    "string"
  ]
}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=500 %}
{% api-method-response-example-description %}
Server error
{% endapi-method-response-example-description %}

```
{
  "message": "string",
  "code": 0,
  "not_found": [
    "string"
  ]
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}



{% api-method method="get" host=" " path="https://api.imaging.datacommons.cancer.gov/v1/cohorts/manifest/{cohort\_id}" %}
{% api-method-summary %}
cohorts/manifest/{cohort\_id}
{% endapi-method-summary %}

{% api-method-description %}
Return a manifest of access\_methods of items in a specified cohort.  
The returned data is paged. If the response includes a non-null next\_page value, then additional data is available and can be obtained by passing the returned next page value in a subsequent call of this endpoint. When a non-null next\_page value is passed, parameters other than page\_size are ignored.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="cohort\_id" type="integer" required=true %}
The cohort\_id of the cohort whose manifest is to be returned.
{% endapi-method-parameter %}
{% endapi-method-path-parameters %}

{% api-method-query-parameters %}
{% api-method-parameter name="CRDC\_Study\_GUID" type="boolean" required=false %}
if True, return the CRDC GUID of each study.
{% endapi-method-parameter %}

{% api-method-parameter name="CRDC\_Series\_GUID" type="boolean" required=false %}
If True, return the CRDC GUID of each series.
{% endapi-method-parameter %}

{% api-method-parameter name="CRDC\_Instance\_GUID" type="boolean" required=false %}
If True, return the CRDC GUID of each instance.
{% endapi-method-parameter %}

{% api-method-parameter name="GCS\_URL" type="boolean" required=false %}
If True, return the GCS URL of each instance.
{% endapi-method-parameter %}

{% api-method-parameter name="sql" type="boolean" required=false %}
If True, return the BigQuery SQL that was used to generate the returned data.  
Default: False
{% endapi-method-parameter %}

{% api-method-parameter name="Collection\_ID" type="boolean" required=false %}
If True, return the Collection ID of each instance.  
Default: False
{% endapi-method-parameter %}

{% api-method-parameter name="Patient\_ID" type="boolean" required=false %}
If True, return the Patient ID of each instance.  
Default: False
{% endapi-method-parameter %}

{% api-method-parameter name="StudyInstanceUID" type="boolean" required=false %}
If True, return the StudyInstanceUID of each instance.  
Default: False
{% endapi-method-parameter %}

{% api-method-parameter name="SeriesInstanceUID" type="boolean" required=false %}
If True, return the SeriesInstanceUID of each instance.  
Default: False
{% endapi-method-parameter %}

{% api-method-parameter name="SOPInstanceUID" type="boolean" required=false %}
If True, return the SOPInstanceUID of each instance.  
Default: False
{% endapi-method-parameter %}

{% api-method-parameter name="Source\_DOI" type="boolean" required=false %}
If True, include  the DOI of the TCIA collection description page of each instance.  
Default: False
{% endapi-method-parameter %}

{% api-method-parameter name="page\_size" type="integer" required=false %}
The maximum number of items to return.  
Default: 1000
{% endapi-method-parameter %}

{% api-method-parameter name="next\_page" type="string" required=false %}
A token identifying the next available page of data. This value is obtained from the response to a previous access of this endpoint. If the value of next\_page from the previous access is "null", there is no more available data.  
Default: ""
{% endapi-method-parameter %}
{% endapi-method-query-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}
This is an example response when the query specified that CRDC\_Instance\_GUIDs, Collection\_IDs, Patient\_IDs, and GCS\_URLs be returned. The `total_found` value indicates that 196844 rows are available, and the "rows\_returned" value indicates that 2 rows were returned in this response \(`page_size` was 2\).  
The `next_page` value in the response can be used in a subsequent call to this API to retrieve the additional results.
{% endapi-method-response-example-description %}

    {
      "code": 200,
      "cohort": {
        "description": "Example description",
        "filterSet": {
          "filters": {
            "Modality": [
              "CT",
              "MR"
            ],
            "collection_id": [
              "tcga_luad",
              "tcga_kirc"
            ],
            "race": [
              "ASIAN"
            ]
          },
          "idc_data_version": "4.0"
        },
        "name": "mycohort",
        "sql": "\n            #standardSQL\n    \n        SELECT dicom_pivot_v4.collection_id,dicom_pivot_v4.PatientID,dicom_pivot_v4.crdc_instance_uuid,dicom_pivot_v4.gcs_url\n        FROM `idc-dev-etl.idc_v4.dicom_pivot_v4` dicom_pivot_v4 \n        \n        JOIN `idc-dev-etl.idc_v4.tcga_clinical_rel9` tcga_clinical_rel9\n        ON dicom_pivot_v4.PatientID = tcga_clinical_rel9.case_barcode\n    \n        WHERE (dicom_pivot_v4.collection_id IN ('tcga_luad','tcga_kirc')) AND (dicom_pivot_v4.Modality IN ('CT','MR')) AND (tcga_clinical_rel9.race = 'ASIAN')\n        GROUP BY dicom_pivot_v4.collection_id, dicom_pivot_v4.PatientID, dicom_pivot_v4.crdc_instance_uuid, dicom_pivot_v4.gcs_url\n        ORDER BY dicom_pivot_v4.gcs_url ASC\n        \n        \n    "
      },
      "manifest": {
        "json_manifest": [
          {
            "CRDC_Instance_GUID": "dg.4DFC/000c8565-76f2-4bc8-9a34-33dd3d3924b3",
            "Collection_ID": "tcga_kirc",
            "GCS_URL": "gs://idc_dev/000c8565-76f2-4bc8-9a34-33dd3d3924b3.dcm",
            "Patient_ID": "TCGA-B0-4821"
          },
          {
            "CRDC_Instance_GUID": "dg.4DFC/002b13fe-8f12-415a-a5e8-401eedba2909",
            "Collection_ID": "tcga_kirc",
            "GCS_URL": "gs://idc_dev/002b13fe-8f12-415a-a5e8-401eedba2909.dcm",
            "Patient_ID": "TCGA-BP-4970"
          }
        ],
        "rowsReturned": 2,
        "totalFound": 1581
      },
      "next_page": "gAAAAABhS2WcO-Ibwdo__0L0z6xMqcnnXoXo5A1EzHeEqqdoQRrupw9PxN29uZxUBvkaJyA_Y3Uf3XEHHA831zGeM9Xae8bOAdL-z_lCFcJ00JC-sGG4BMb4-9UwVsJ1qBkwx1WsXawGWd9jY8W5QnvKKL5eZtEhPwwd_iR-O1i8RWwPLdUsGnUrM78VmsKHXMQmvCZWQEvYuhmXx6uLn8rDSEczfuJLYvF_GzdFNpWgYSGjLzZYibqgZ18eFQyljhcuCX1UjLeW8YNsmO8HMn2Co-fV_W6FyNCt2XyhRFpfg4NOBR2UxLKRuA9b9sPKwJuoqY5-XlPLu6qjWEPKUX48tHNh50dD4FdJoREskSdOw2ulND_jHrg="
    }
{% endapi-method-response-example %}

{% api-method-response-example httpCode=400 %}
{% api-method-response-example-description %}
Bad request
{% endapi-method-response-example-description %}

```
{
  "message": "string",
  "code": 0,
  "not_found": [
    "string"
  ]
}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=500 %}
{% api-method-response-example-description %}
Server error
{% endapi-method-response-example-description %}

```
{
  "message": "string",
  "code": 0,
  "not_found": [
    "string"
  ]
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% api-method method="post" host=" " path="https://api.imaging.datacommons.cancer.gov/v1/cohorts/manifest/preview" %}
{% api-method-summary %}
cohorts/manifest/preview
{% endapi-method-summary %}

{% api-method-description %}
This API is equivalent to the following API sequence:  
`POST /cohorts  
GET /cohorts/{cohort_id}/manifest  
DELETE /cohorts/{cohort_id}`  
However, this API does not actually create a cohort.  
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-query-parameters %}
{% api-method-parameter name="CRDC\_Study\_GUID" type="boolean" required=false %}
If True, return the CRDC GUID of each study.
{% endapi-method-parameter %}

{% api-method-parameter name="CRDC\_Series\_GUID" type="boolean" required=false %}
If True, return the CRDC GUID of each series.
{% endapi-method-parameter %}

{% api-method-parameter name="CRDC\_Instance\_GUID" type="boolean" required=false %}
If True, return the CRDC GUID of each instance.
{% endapi-method-parameter %}

{% api-method-parameter name="GCS\_URL" type="boolean" required=false %}
If True, return the GCS URL of each instance.
{% endapi-method-parameter %}

{% api-method-parameter name="sql" type="boolean" required=false %}
If True, return the BigQuery SQL that was used to generate the returned data.  
Default: False
{% endapi-method-parameter %}

{% api-method-parameter name="Collection\_ID" type="boolean" required=false %}
If True, return the Collection ID of each instance.  
Default: False
{% endapi-method-parameter %}

{% api-method-parameter name="Patient\_ID" type="boolean" required=false %}
If True, return the Patient ID of each instance.  
Default: False
{% endapi-method-parameter %}

{% api-method-parameter name="StudyInstanceUID" type="boolean" required=false %}
If True, return the StudyInstanceUID of each instance.  
Default: False
{% endapi-method-parameter %}

{% api-method-parameter name="SeriesInstanceUID" type="boolean" required=false %}
If True, return the SeriesInstanceUID of each instance..  
Default: False
{% endapi-method-parameter %}

{% api-method-parameter name="SOPInstanceUID" type="boolean" required=false %}
If True, return the SOPInstanceUID of each instance.  
Default: False
{% endapi-method-parameter %}

{% api-method-parameter name="Source\_DOI" type="boolean" required=false %}
If True, include the DOI of the TCIA collection description page of each instance  
Default: False
{% endapi-method-parameter %}

{% api-method-parameter name="page\_size" type="integer" required=false %}
The maximum number of rows to return  
Default: 1000
{% endapi-method-parameter %}

{% api-method-parameter name="next\_page" type="string" required=false %}
A token identifying the next available page of data. This value is obtained from the response to a previous access of this endpoint. If the value of next\_page from the previous access is "null", there is no more data available.  
If the empty string is passed, a new BQ query is performed.  
Default: ""
{% endapi-method-parameter %}
{% endapi-method-query-parameters %}

{% api-method-body-parameters %}
{% api-method-parameter name="cohort\_def" type="object" required=false %}
A structure, including filterSet and IDC data version, defining the cohort. See `POST cohorts` for an example.
{% endapi-method-parameter %}
{% endapi-method-body-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}
Refer to `GET cohorts/manifest/{cohort_id}` for an example response and discussion.
{% endapi-method-response-example-description %}

```

```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=400 %}
{% api-method-response-example-description %}
Bad request
{% endapi-method-response-example-description %}

```
{
  "message": "string",
  "code": 0,
  "not_found": [
    "string"
  ]
}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=500 %}
{% api-method-response-example-description %}
Server error
{% endapi-method-response-example-description %}

```
{
  "message": "string",
  "code": 0,
  "not_found": [
    "string"
  ]
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% api-method method="get" host=" " path="https://api.imaging.datacommons.cancer.gov/v1/cohorts/manifest/nextPage" %}
{% api-method-summary %}
cohorts/manifest/nextPage
{% endapi-method-summary %}

{% api-method-description %}
Get the next page of results from a cohorts/manifest/{cohort\_id}, cohorts/manifest/preview or cohorts/manifest/nextPage query.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-query-parameters %}
{% api-method-parameter name="page\_size" type="integer" required=false %}
The maximum number of rows to return  
Default: 1000
{% endapi-method-parameter %}

{% api-method-parameter name="next\_page" type="string" required=true %}
The next\_page token returned by a previous /cohorts/manifest/xxx request.
{% endapi-method-parameter %}
{% endapi-method-query-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}
This is an example response when the _page\_size_ was 10.
{% endapi-method-response-example-description %}

```
{
  "code": 200,
  "manifest": {
    "json_manifest": [
      {
        "CRDC_Instance_GUID": "dg.4DFC/00606631-64e6-4d1c-95dd-2373648aafb6",
        "Collection_ID": "tcga_kirc",
        "GCS_URL": "gs://idc_dev/00606631-64e6-4d1c-95dd-2373648aafb6.dcm",
        "Patient_ID": "TCGA-B0-4821"
      },
      {
        "CRDC_Instance_GUID": "dg.4DFC/0077877c-6172-488b-8754-bfa7df7589fd",
        "Collection_ID": "tcga_kirc",
        "GCS_URL": "gs://idc_dev/0077877c-6172-488b-8754-bfa7df7589fd.dcm",
        "Patient_ID": "TCGA-B0-4821"
      },
      {
        "CRDC_Instance_GUID": "dg.4DFC/00be9515-8b58-45c0-91ee-4c5022be2c00",
        "Collection_ID": "tcga_kirc",
        "GCS_URL": "gs://idc_dev/00be9515-8b58-45c0-91ee-4c5022be2c00.dcm",
        "Patient_ID": "TCGA-CJ-4899"
      },
      {
        "CRDC_Instance_GUID": "dg.4DFC/00bfdcd1-518d-4e1a-b105-6a08ff8b6556",
        "Collection_ID": "tcga_kirc",
        "GCS_URL": "gs://idc_dev/00bfdcd1-518d-4e1a-b105-6a08ff8b6556.dcm",
        "Patient_ID": "TCGA-CJ-4899"
      },
      {
        "CRDC_Instance_GUID": "dg.4DFC/00e0d3a5-36c1-400f-a901-44de946b2283",
        "Collection_ID": "tcga_kirc",
        "GCS_URL": "gs://idc_dev/00e0d3a5-36c1-400f-a901-44de946b2283.dcm",
        "Patient_ID": "TCGA-B0-4821"
      },
      {
        "CRDC_Instance_GUID": "dg.4DFC/00fc78b4-afc5-4518-bf28-ac4a2cfc7eca",
        "Collection_ID": "tcga_kirc",
        "GCS_URL": "gs://idc_dev/00fc78b4-afc5-4518-bf28-ac4a2cfc7eca.dcm",
        "Patient_ID": "TCGA-B0-4821"
      },
      {
        "CRDC_Instance_GUID": "dg.4DFC/0110057d-83ac-4cc6-a547-1bc2d1f91d5c",
        "Collection_ID": "tcga_kirc",
        "GCS_URL": "gs://idc_dev/0110057d-83ac-4cc6-a547-1bc2d1f91d5c.dcm",
        "Patient_ID": "TCGA-CJ-4899"
      },
      {
        "CRDC_Instance_GUID": "dg.4DFC/01856791-a91b-4ea1-992c-6eb1a19fe0a7",
        "Collection_ID": "tcga_kirc",
        "GCS_URL": "gs://idc_dev/01856791-a91b-4ea1-992c-6eb1a19fe0a7.dcm",
        "Patient_ID": "TCGA-CJ-4899"
      },
      {
        "CRDC_Instance_GUID": "dg.4DFC/018b5550-4258-4f31-ad96-b0c780482a63",
        "Collection_ID": "tcga_kirc",
        "GCS_URL": "gs://idc_dev/018b5550-4258-4f31-ad96-b0c780482a63.dcm",
        "Patient_ID": "TCGA-BP-4970"
      },
      {
        "CRDC_Instance_GUID": "dg.4DFC/01be03e9-fe11-4272-b90d-ff8f57cb26b4",
        "Collection_ID": "tcga_kirc",
        "GCS_URL": "gs://idc_dev/01be03e9-fe11-4272-b90d-ff8f57cb26b4.dcm",
        "Patient_ID": "TCGA-BP-4970"
      }
    ],
    "rowsReturned": 10,
    "totalFound": 1581
  },
  "next_page": "gAAAAABhS2VDWJ23H32nZ2KYUvC4skz15c2uZKpdPc7zvTebFTl6J0IePA8-VU7RMq9S3EOBsfvC-hz2W1GD48q8YB0GOjfLFzja0Y0AKHmWT_BR4na3Ag7B9rmnDSqfj8mOkHG9VS7aUHw6qauORxX5RGejUSUhryJ-e2TcsvbV6C7MAV5n5wknK9gkl900GjTdAksnhObZOPpC7lTd1YZI3vjHAh0H4RSxU8ffdVsvvZsJTHu7uF_JZHc4jNpE5_JLDZzYGUU7jlUlU9qaENv-ErNDkjdht_tK4LScqvrNULPeDEuU1wRa68bK9E6fv3ta3xqQNVduGA8XFIBKxR7TW17lDtRJxiZq47ZjmJ0Wd76P3FAhJSQ="
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% api-method method="get" host=" " path="https://api.imaging.datacommons.cancer.gov/v1/cohorts/query/{cohort\_id}" %}
{% api-method-summary %}
cohorts/query/{cohort\_id}
{% endapi-method-summary %}

{% api-method-description %}
Return attributes of the instances in a cohort.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="cohort\_id" type="integer" required=true %}
The cohort\_id of the cohort on which to perform the query.
{% endapi-method-parameter %}
{% endapi-method-path-parameters %}

{% api-method-query-parameters %}
{% api-method-parameter name="sql" type="boolean" required=false %}
If True, return the BigQuery SQL that was used to generate the returned data.  
Default: False
{% endapi-method-parameter %}

{% api-method-parameter name="page\_size" type="integer" required=false %}
The maximum number of items to return  
Default: 1000
{% endapi-method-parameter %}

{% api-method-parameter name="next\_page" type="string" required=false %}
A token identifying the next available page of data. This value is obtained from the response to a previous access of this endpoint. If the value of next\_page from the previous access is "null", there is no more available data.  
Default: Null
{% endapi-method-parameter %}
{% endapi-method-query-parameters %}

{% api-method-body-parameters %}
{% api-method-parameter name="queryFields" type="array" required=true %}
A queryFields object. The _fields_ element is an array of field identifiers whose values are to be returned. The supported queryable fields are enumerated in the definition of the queryFields object.
{% endapi-method-parameter %}
{% endapi-method-body-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}
The following is an example of a response when the queryFields was :  
{ "fields": \["SOPInstanceUID", "Modality", "BodyPartExamined", "SliceThickness" \] }. sql was True and  page\_size was 5.   
If SOPInstanceUID had been False, only the distinct combinations of Modality, BodyPartExamined and SliceThickness would have been returned. 
{% endapi-method-response-example-description %}

    {
      "code": 200,
      "cohort_def": {
        "description": "Example description",
        "filterSet": {
          "filters": {
            "Modality": [
              "CT",
              "MR"
            ],
            "collection_id": [
              "tcga_luad",
              "tcga_kirc"
            ],
            "race": [
              "WHITE"
            ]
          },
          "idc_data_version": "4.0"
        },
        "filters": {
          "Modality": [
            "CT",
            "MR"
          ],
          "collection_id": [
            "tcga_luad",
            "tcga_kirc"
          ],
          "race": [
            "WHITE"
          ]
        },
        "name": "mycohort",
        "sql": "\n            #standardSQL\n    \n        SELECT dicom_pivot_v4.SOPInstanceUID,dicom_pivot_v4.Modality,dicom_pivot_v4.BodyPartExamined,dicom_pivot_v4.SliceThickness\n        FROM `idc-dev-etl.idc_v4.dicom_pivot_v4` dicom_pivot_v4 \n        \n        JOIN `idc-dev-etl.idc_v4.tcga_clinical_rel9` tcga_clinical_rel9\n        ON dicom_pivot_v4.PatientID = tcga_clinical_rel9.case_barcode\n    \n        WHERE (dicom_pivot_v4.collection_id IN ('tcga_luad','tcga_kirc')) AND (dicom_pivot_v4.Modality IN ('CT','MR')) AND (tcga_clinical_rel9.race = 'WHITE')\n        GROUP BY dicom_pivot_v4.SOPInstanceUID, dicom_pivot_v4.Modality, dicom_pivot_v4.BodyPartExamined, dicom_pivot_v4.SliceThickness\n        ORDER BY dicom_pivot_v4.SOPInstanceUID ASC, dicom_pivot_v4.Modality ASC, dicom_pivot_v4.BodyPartExamined ASC, dicom_pivot_v4.SliceThickness ASC\n        \n        \n    "
      },
      "next_page": "gAAAAABhSOtUCk358O_qvpQhFMzWCYA2XlF0Jr1h1Pm8-qAOyYYrxLBUwcAdZZDBcDqh40ryNFcsg6AYmFJ0JCg7D6222-mIoeCaqhYCAEU-YQ6Rvco7sEG-QDJq0UYCRZKqKsldJNgnN1n0CaosdOCHe9rZCsjAMlOsjW6AyZSmgqGjYFbWjlJp7tuiwncR0O6rrnAsb4GfiTt_YG-BEEifJRP0xNFJaOgvjixpr2DCsuwTwp4Hpqk23hzWR-SSlgF1vRSWG-OngxK4MUtytYGnkjWqyfWlYCG3WllmCDrXe4prr23AfG88rTpSLxVjFqsq2-G964ve",
      "query_results": {
        "json": [
          {
            "BodyPartExamined": "KIDNEY",
            "Modality": "CT",
            "SOPInstanceUID": "1.3.6.1.4.1.14519.5.2.1.1357.4004.100052696407437114116810952334",
            "SliceThickness": "2.5"
          },
          {
            "BodyPartExamined": "KIDNEY",
            "Modality": "CT",
            "SOPInstanceUID": "1.3.6.1.4.1.14519.5.2.1.1357.4004.100100699868028287673029927535",
            "SliceThickness": "2.5"
          },
          {
            "BodyPartExamined": "KIDNEY",
            "Modality": "CT",
            "SOPInstanceUID": "1.3.6.1.4.1.14519.5.2.1.1357.4004.100189985561196457997390127394",
            "SliceThickness": "5"
          },
          {
            "BodyPartExamined": "KIDNEY",
            "Modality": "CT",
            "SOPInstanceUID": "1.3.6.1.4.1.14519.5.2.1.1357.4004.100190135643049697554629929009",
            "SliceThickness": "2"
          },
          {
            "BodyPartExamined": "KIDNEY",
            "Modality": "CT",
            "SOPInstanceUID": "1.3.6.1.4.1.14519.5.2.1.1357.4004.100209053339233505710613826900",
            "SliceThickness": "2.5"
          }
        ],
        "rowsReturned": 5,
        "totalFound": 196844
      }
    }
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% api-method method="post" host=" " path="https://api.imaging.datacommons.cancer.gov/v1/cohorts/query/preview" %}
{% api-method-summary %}
cohorts/query/preview
{% endapi-method-summary %}

{% api-method-description %}
This API is equivalent to the following API sequence:  
`POST /cohorts  
GET /cohorts/{cohort_id}/query  
DELETE /cohorts/{cohort_id}  
However, this API does not actually create a cohort.`
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-query-parameters %}
{% api-method-parameter name="sql" type="boolean" required=false %}
If True, return the BigQuery SQL that was used to generate the returned query
{% endapi-method-parameter %}

{% api-method-parameter name="page\_size" type="integer" required=false %}
The maximum number of rows to return.  
Default: 1000
{% endapi-method-parameter %}

{% api-method-parameter name="next\_page" type="string" required=false %}
A token identifying the next available page of data. This value is obtained from the response to a previous access of this endpoint. If the value of next\_page from the previous access is "null", there is no more data available.  
If the empty string is passed, a new BQ query is performed.  
Default: ""
{% endapi-method-parameter %}
{% endapi-method-query-parameters %}

{% api-method-body-parameters %}
{% api-method-parameter name="queryPreviewBody" type="object" required=false %}
The cohort definition and list of fields to query. The queryable fields are enumerated in the definition of the queryPreviewBody object.
{% endapi-method-parameter %}
{% endapi-method-body-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}
Refer to cohorts/{cohort\_id}/query for an example response.
{% endapi-method-response-example-description %}

```

```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=400 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```
{
  "message": "string",
  "code": 0,
  "not_found": [
    "string"
  ]
}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=500 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```
{
  "message": "string",
  "code": 0,
  "not_found": [
    "string"
  ]
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% api-method method="get" host=" " path="https://api.imaging.datacommons.cancer.gov/v1/cohorts/query/nextPage" %}
{% api-method-summary %}
cohorts/query/nextPage
{% endapi-method-summary %}

{% api-method-description %}
Get the next page of results from a cohorts/query/{cohort\_id}, cohorts/query/preview or cohorts/query/nextPage query.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-query-parameters %}
{% api-method-parameter name="page\_size" type="integer" required=false %}
The maximum number of rows to return.  
Default: 1000
{% endapi-method-parameter %}

{% api-method-parameter name="next\_page" type="string" required=true %}
The next\_page token returned by a previous cohorts/query/xxx request.
{% endapi-method-parameter %}
{% endapi-method-query-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}
This is an example response when the page\_size was 5.
{% endapi-method-response-example-description %}

```
{
  "code": 200,
  "cohort": {},
  "next_page": "gAAAAABhS2n0xuQpzbCFI9ort38HcRd0u-bD3aDPax8vNMMQU3dGaZMqvEqXiud3ngZFubv9JdFCgq3ZGZ8PyoSHsVjfupRNArfo2dB6I9jUBSrXwRiBqIut1WkSJ5oLj23Frbe6jrlzxlzxl-1V9FmU6MngbVGtmRfuT4sr4Mz2licIyyYjjf9tDQCyp2V_rvJVg3g9-t-VA1wQ7WGdxXYI_FyIgxSQUvw36s75Bmu5_N_93e0sGvydAgzFKz9sFsPkxlJUTpU7Pg24IXmPSQ9B7s-C44XPEd1eiVHZ0cgGbxgvPjU63Nf03GSYWx0bGj9NfWgBfik87RR6uN3PYVbRHFtjS5oBh-NMgWBEde-cqXdq_ec7DRA=",
  "query_results": {
    "json": [
      {
        "BodyPartExamined": "KIDNEY",
        "Modality": "CT",
        "SOPInstanceUID": "1.3.6.1.4.1.14519.5.2.1.1357.4004.100242095173296621138367847007",
        "SliceThickness": "2.5"
      },
      {
        "BodyPartExamined": "KIDNEY",
        "Modality": "CT",
        "SOPInstanceUID": "1.3.6.1.4.1.14519.5.2.1.1357.4004.100286137220775916594295647320",
        "SliceThickness": "2.5"
      },
      {
        "BodyPartExamined": "KIDNEY",
        "Modality": "CT",
        "SOPInstanceUID": "1.3.6.1.4.1.14519.5.2.1.1357.4004.100299903414395290320432591136",
        "SliceThickness": "5.0"
      },
      {
        "BodyPartExamined": "KIDNEY",
        "Modality": "CT",
        "SOPInstanceUID": "1.3.6.1.4.1.14519.5.2.1.1357.4004.100326498692037285111664897679",
        "SliceThickness": "2.5"
      },
      {
        "BodyPartExamined": "KIDNEY",
        "Modality": "CT",
        "SOPInstanceUID": "1.3.6.1.4.1.14519.5.2.1.1357.4004.100346341369046277744111428817",
        "SliceThickness": "5.0"
      }
    ],
    "rowsReturned": 5,
    "totalFound": 196844
  }
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% api-method method="get" host=" " path="https://api.imaging.datacommons.cancer.gov/v1/cohorts/dicomMetadata" %}
{% api-method-summary %}
dicomMetadata
{% endapi-method-summary %}

{% api-method-description %}
Get a defined, fixed set of metadata for all the instances in IDC data.See the example response for the list of attributes.  
The returned data is paged. If the response includes a non-null next\_page value, additional data is available and can be obtained by passing the returned next page value in a subsequent call of this endpoint.   
When a non-null next page value is passed, parameters other than page\_size are ignored.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-query-parameters %}
{% api-method-parameter name="page\_size" type="integer" required=false %}
The maximum number of rows to return.  
Default: 1000
{% endapi-method-parameter %}

{% api-method-parameter name="next\_page" type="string" required=false %}
A token identifying the next available page of data. This value is obtained from a previous access of this endpoint. If the value of next\_page from the previous response is "null", there is no more data available.  
If the empty string is passed, a new BQ query is performed.
{% endapi-method-parameter %}
{% endapi-method-query-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}
This is an example response when the page\_size query param was 2. As can be seen, the total number of rows found was 3075233. Additional data can be obtained by calling the API again and passing the returned next\_page token.
{% endapi-method-response-example-description %}

```
{
  "code": 200,
  "next_page": "gAAAAABhSS0u_hWWjCxrcGViF0vo0bl-vQ66-RF0HFfeLyr7Y878BsUXehMhEOws4TpicguEVEk7dVe5vHbk147XVrIKkNdvkCd-lwzJGULS90PQ_J7iKaAH_4Q7v5ijPlZ6zO2MSwwWZf8ZzRH6pw1WGYicGYcm_0ErwFlyrDfPZGmrSP0wIZ0EBvK_lKolPvxv77VTMf5DeqqPzuqVHXEIeSe4Lh9v8d58IvT_2Ma8UR5RdyNwzRyi7p8lGhtFmpyUbs8MGAjR336-DnTQEwXIIOVhXEvecJG0AfYvbRR0NoaJu76cgMjkLq8gqxRy8IH7x3TAGsD2",
  "query_results": {
    "json": [
      {
        "AdditionalPatientHistory": null,
        "Allergies": [],
        "AnatomicRegionSequence": null,
        "Apparent_Diffusion_Coefficient": null,
        "BodyPartExamined": "LUNG",
        "Calcification": null,
        "Diameter": null,
        "EthnicGroup": null,
        "FrameOfReferenceUID": null,
        "Glycolysis_Within_First_Quarter_of_Intensity_Range": null,
        "Glycolysis_Within_Fourth_Quarter_of_Intensity_Range": null,
        "Glycolysis_Within_Second_Quarter_of_Intensity_Range": null,
        "Glycolysis_Within_Third_Quarter_of_Intensity_Range": null,
        "ImageType": [
          "ORIGINAL",
          "PRIMARY",
          "AXIAL"
        ],
        "Internal_structure": null,
        "LastMenstrualDate": null,
        "Lobular_Pattern": null,
        "Malignancy": null,
        "Manufacturer": "Varian Imaging Laboratories, Switzerland",
        "ManufacturerModelName": "Trilogy Cone Beam CT",
        "Margin": null,
        "MedicalAlerts": [],
        "Modality": "CT",
        "Occupation": null,
        "PatientAge": "000Y",
        "PatientComments": null,
        "PatientID": "100_HM10395",
        "PatientSize": null,
        "PatientWeight": null,
        "Percent_Within_First_Quarter_of_Intensity_Range": null,
        "Percent_Within_Fourth_Quarter_of_Intensity_Range": null,
        "Percent_Within_Second_Quarter_of_Intensity_Range": null,
        "Percent_Within_Third_Quarter_of_Intensity_Range": null,
        "PregnancyStatus": null,
        "ReasonForStudy": null,
        "RequestedProcedureComments": null,
        "SOPClassUID": "1.2.840.10008.5.1.4.1.1.2",
        "SOPInstanceUID": "1.3.6.1.4.1.14519.5.2.1.6834.5010.106491109248026203442466315815",
        "SUVbw": null,
        "SegmentAlgorithmType": null,
        "SegmentNumber": null,
        "SegmentedPropertyCategoryCodeSequence": null,
        "SegmentedPropertyTypeCodeSequence": null,
        "SeriesDescription": "P4^P100^S102^I0, Gated, 10.0%",
        "SeriesInstanceUID": "1.3.6.1.4.1.14519.5.2.1.6834.5010.155488857019753544209698582121",
        "SeriesNumber": "501",
        "SliceThickness": "3.0",
        "SmokingStatus": null,
        "Sphericity": null,
        "Spiculation": null,
        "Standardized_Added_Metabolic_Activity": null,
        "Standardized_Added_Metabolic_Activity_Background": null,
        "StudyDate": "1997-09-17",
        "StudyDescription": "p4",
        "StudyInstanceUID": "1.3.6.1.4.1.14519.5.2.1.6834.5010.117291650651265592494655001035",
        "Subtlety_score": null,
        "Surface_area_of_mesh": null,
        "Texture": null,
        "Total_Lesion_Glycolysis": null,
        "Volume": null,
        "collection_id": "4d_lung",
        "crdc_instance_uuid": "6372331d-308b-4ff4-bf9d-5d81db1ecbfe",
        "crdc_series_uuid": "085e85b3-3780-47f6-8538-585bcca95336",
        "crdc_study_uuid": "bd36de65-5768-4438-808c-dd14667e9a0f",
        "gcs_url": "gs://idc_dev/6372331d-308b-4ff4-bf9d-5d81db1ecbfe.dcm",
        "license_short_name": "CC BY 3.0",
        "program": "Community",
        "source_DOI": "10.7937/K9/TCIA.2016.ELN8YGLE",
        "tcia_species": "Human",
        "tcia_tumorLocation": "Lung"
      },
      {
        "AdditionalPatientHistory": null,
        "Allergies": [],
        "AnatomicRegionSequence": null,
        "Apparent_Diffusion_Coefficient": null,
        "BodyPartExamined": "LUNG",
        "Calcification": null,
        "Diameter": null,
        "EthnicGroup": null,
        "FrameOfReferenceUID": null,
        "Glycolysis_Within_First_Quarter_of_Intensity_Range": null,
        "Glycolysis_Within_Fourth_Quarter_of_Intensity_Range": null,
        "Glycolysis_Within_Second_Quarter_of_Intensity_Range": null,
        "Glycolysis_Within_Third_Quarter_of_Intensity_Range": null,
        "ImageType": [
          "ORIGINAL",
          "PRIMARY",
          "AXIAL"
        ],
        "Internal_structure": null,
        "LastMenstrualDate": null,
        "Lobular_Pattern": null,
        "Malignancy": null,
        "Manufacturer": "Varian Imaging Laboratories, Switzerland",
        "ManufacturerModelName": "Trilogy Cone Beam CT",
        "Margin": null,
        "MedicalAlerts": [],
        "Modality": "CT",
        "Occupation": null,
        "PatientAge": "000Y",
        "PatientComments": null,
        "PatientID": "100_HM10395",
        "PatientSize": null,
        "PatientWeight": null,
        "Percent_Within_First_Quarter_of_Intensity_Range": null,
        "Percent_Within_Fourth_Quarter_of_Intensity_Range": null,
        "Percent_Within_Second_Quarter_of_Intensity_Range": null,
        "Percent_Within_Third_Quarter_of_Intensity_Range": null,
        "PregnancyStatus": null,
        "ReasonForStudy": null,
        "RequestedProcedureComments": null,
        "SOPClassUID": "1.2.840.10008.5.1.4.1.1.2",
        "SOPInstanceUID": "1.3.6.1.4.1.14519.5.2.1.6834.5010.114169789422251889001325116468",
        "SUVbw": null,
        "SegmentAlgorithmType": null,
        "SegmentNumber": null,
        "SegmentedPropertyCategoryCodeSequence": null,
        "SegmentedPropertyTypeCodeSequence": null,
        "SeriesDescription": "P4^P100^S102^I0, Gated, 10.0%",
        "SeriesInstanceUID": "1.3.6.1.4.1.14519.5.2.1.6834.5010.155488857019753544209698582121",
        "SeriesNumber": "501",
        "SliceThickness": "3.0",
        "SmokingStatus": null,
        "Sphericity": null,
        "Spiculation": null,
        "Standardized_Added_Metabolic_Activity": null,
        "Standardized_Added_Metabolic_Activity_Background": null,
        "StudyDate": "1997-09-17",
        "StudyDescription": "p4",
        "StudyInstanceUID": "1.3.6.1.4.1.14519.5.2.1.6834.5010.117291650651265592494655001035",
        "Subtlety_score": null,
        "Surface_area_of_mesh": null,
        "Texture": null,
        "Total_Lesion_Glycolysis": null,
        "Volume": null,
        "collection_id": "4d_lung",
        "crdc_instance_uuid": "76320e2a-fc48-4a88-947c-12c6e0250a49",
        "crdc_series_uuid": "085e85b3-3780-47f6-8538-585bcca95336",
        "crdc_study_uuid": "bd36de65-5768-4438-808c-dd14667e9a0f",
        "gcs_url": "gs://idc_dev/76320e2a-fc48-4a88-947c-12c6e0250a49.dcm",
        "license_short_name": "CC BY 3.0",
        "program": "Community",
        "source_DOI": "10.7937/K9/TCIA.2016.ELN8YGLE",
        "tcia_species": "Human",
        "tcia_tumorLocation": "Lung"
      }
    ],
    "rowsReturned": 2,
    "totalFound": 36017268
  }
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% api-method method="get" host=" " path="https://api.imaging.datacommons.cancer.gov/v1/dicomMetadata/nextPage" %}
{% api-method-summary %}
dicomMetadata/nextPage
{% endapi-method-summary %}

{% api-method-description %}
Get the next page of results from a /cohorts/dicomMetadata or /cohorts/dicomMetadata/nextPage query.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-query-parameters %}
{% api-method-parameter name="page\_size" type="integer" required=false %}
The maximum number of rows to return.  
Default: 1000
{% endapi-method-parameter %}

{% api-method-parameter name="next\_page" type="string" required=true %}
The next\_page token returned by a previous query.
{% endapi-method-parameter %}
{% endapi-method-query-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}
This is an example response when the next\_page parameter was 2.
{% endapi-method-response-example-description %}

```
{
  "code": 200,
  "cohort": {},
  "next_page": "gAAAAABhS3TYko6_OC9qrO_xLPRcnztRLx-tfAeWP0szPb06poePYNAb5c8wNDJznjfhzK_7nHKSatdTh1X3p_eNEGgBDU_Ff0vEdAg2iPvngrEY6KQ6KC2M2Npcl51ZHq3p1aGfnSeECz10ucmySyq4GHU63mZwWr3fGWvnpXD6YnDQZCuGNjImQPaYn2bPG3UlxvVZJMwrHsq36mq9rZHqUetnUyDaMtIH0I7IflRjjiLnf-21GG8GkD_DUvcNLjBkpUiArH7PJHAapeR8aCeCBd4l29togIPy-f2mNDddlJCRtrQ-2ZItyaPs-eLrEsoxEmK2v75O",
  "query_results": {
    "json": [
      {
        "AdditionalPatientHistory": null,
        "Allergies": [],
        "AnatomicRegionSequence": null,
        "Apparent_Diffusion_Coefficient": null,
        "BodyPartExamined": "LUNG",
        "Calcification": null,
        "Diameter": null,
        "EthnicGroup": null,
        "FrameOfReferenceUID": null,
        "Glycolysis_Within_First_Quarter_of_Intensity_Range": null,
        "Glycolysis_Within_Fourth_Quarter_of_Intensity_Range": null,
        "Glycolysis_Within_Second_Quarter_of_Intensity_Range": null,
        "Glycolysis_Within_Third_Quarter_of_Intensity_Range": null,
        "ImageType": [
          "ORIGINAL",
          "PRIMARY",
          "AXIAL"
        ],
        "Internal_structure": null,
        "LastMenstrualDate": null,
        "Lobular_Pattern": null,
        "Malignancy": null,
        "Manufacturer": "Varian Imaging Laboratories, Switzerland",
        "ManufacturerModelName": "Trilogy Cone Beam CT",
        "Margin": null,
        "MedicalAlerts": [],
        "Modality": "CT",
        "Occupation": null,
        "PatientAge": "000Y",
        "PatientComments": null,
        "PatientID": "100_HM10395",
        "PatientSize": null,
        "PatientWeight": null,
        "Percent_Within_First_Quarter_of_Intensity_Range": null,
        "Percent_Within_Fourth_Quarter_of_Intensity_Range": null,
        "Percent_Within_Second_Quarter_of_Intensity_Range": null,
        "Percent_Within_Third_Quarter_of_Intensity_Range": null,
        "PregnancyStatus": null,
        "ReasonForStudy": null,
        "RequestedProcedureComments": null,
        "SOPClassUID": "1.2.840.10008.5.1.4.1.1.2",
        "SOPInstanceUID": "1.3.6.1.4.1.14519.5.2.1.6834.5010.129058176471898833449874870134",
        "SUVbw": null,
        "SegmentAlgorithmType": null,
        "SegmentNumber": null,
        "SegmentedPropertyCategoryCodeSequence": null,
        "SegmentedPropertyTypeCodeSequence": null,
        "SeriesDescription": "P4^P100^S102^I0, Gated, 10.0%",
        "SeriesInstanceUID": "1.3.6.1.4.1.14519.5.2.1.6834.5010.155488857019753544209698582121",
        "SeriesNumber": "501",
        "SliceThickness": "3.0",
        "SmokingStatus": null,
        "Sphericity": null,
        "Spiculation": null,
        "Standardized_Added_Metabolic_Activity": null,
        "Standardized_Added_Metabolic_Activity_Background": null,
        "StudyDate": "1997-09-17",
        "StudyDescription": "p4",
        "StudyInstanceUID": "1.3.6.1.4.1.14519.5.2.1.6834.5010.117291650651265592494655001035",
        "Subtlety_score": null,
        "Surface_area_of_mesh": null,
        "Texture": null,
        "Total_Lesion_Glycolysis": null,
        "Volume": null,
        "collection_id": "4d_lung",
        "crdc_instance_uuid": "0c392524-1086-4fcf-a6c3-170ef8acb12e",
        "crdc_series_uuid": "085e85b3-3780-47f6-8538-585bcca95336",
        "crdc_study_uuid": "bd36de65-5768-4438-808c-dd14667e9a0f",
        "gcs_url": "gs://idc_dev/0c392524-1086-4fcf-a6c3-170ef8acb12e.dcm",
        "license_short_name": "CC BY 3.0",
        "program": "Community",
        "source_DOI": "10.7937/K9/TCIA.2016.ELN8YGLE",
        "tcia_species": "Human",
        "tcia_tumorLocation": "Lung"
      },
      {
        "AdditionalPatientHistory": null,
        "Allergies": [],
        "AnatomicRegionSequence": null,
        "Apparent_Diffusion_Coefficient": null,
        "BodyPartExamined": "LUNG",
        "Calcification": null,
        "Diameter": null,
        "EthnicGroup": null,
        "FrameOfReferenceUID": null,
        "Glycolysis_Within_First_Quarter_of_Intensity_Range": null,
        "Glycolysis_Within_Fourth_Quarter_of_Intensity_Range": null,
        "Glycolysis_Within_Second_Quarter_of_Intensity_Range": null,
        "Glycolysis_Within_Third_Quarter_of_Intensity_Range": null,
        "ImageType": [
          "ORIGINAL",
          "PRIMARY",
          "AXIAL"
        ],
        "Internal_structure": null,
        "LastMenstrualDate": null,
        "Lobular_Pattern": null,
        "Malignancy": null,
        "Manufacturer": "Varian Imaging Laboratories, Switzerland",
        "ManufacturerModelName": "Trilogy Cone Beam CT",
        "Margin": null,
        "MedicalAlerts": [],
        "Modality": "CT",
        "Occupation": null,
        "PatientAge": "000Y",
        "PatientComments": null,
        "PatientID": "100_HM10395",
        "PatientSize": null,
        "PatientWeight": null,
        "Percent_Within_First_Quarter_of_Intensity_Range": null,
        "Percent_Within_Fourth_Quarter_of_Intensity_Range": null,
        "Percent_Within_Second_Quarter_of_Intensity_Range": null,
        "Percent_Within_Third_Quarter_of_Intensity_Range": null,
        "PregnancyStatus": null,
        "ReasonForStudy": null,
        "RequestedProcedureComments": null,
        "SOPClassUID": "1.2.840.10008.5.1.4.1.1.2",
        "SOPInstanceUID": "1.3.6.1.4.1.14519.5.2.1.6834.5010.136942845453235090793455027072",
        "SUVbw": null,
        "SegmentAlgorithmType": null,
        "SegmentNumber": null,
        "SegmentedPropertyCategoryCodeSequence": null,
        "SegmentedPropertyTypeCodeSequence": null,
        "SeriesDescription": "P4^P100^S102^I0, Gated, 10.0%",
        "SeriesInstanceUID": "1.3.6.1.4.1.14519.5.2.1.6834.5010.155488857019753544209698582121",
        "SeriesNumber": "501",
        "SliceThickness": "3.0",
        "SmokingStatus": null,
        "Sphericity": null,
        "Spiculation": null,
        "Standardized_Added_Metabolic_Activity": null,
        "Standardized_Added_Metabolic_Activity_Background": null,
        "StudyDate": "1997-09-17",
        "StudyDescription": "p4",
        "StudyInstanceUID": "1.3.6.1.4.1.14519.5.2.1.6834.5010.117291650651265592494655001035",
        "Subtlety_score": null,
        "Surface_area_of_mesh": null,
        "Texture": null,
        "Total_Lesion_Glycolysis": null,
        "Volume": null,
        "collection_id": "4d_lung",
        "crdc_instance_uuid": "bf17b0bf-aa53-4c06-a83b-a4d280d9517e",
        "crdc_series_uuid": "085e85b3-3780-47f6-8538-585bcca95336",
        "crdc_study_uuid": "bd36de65-5768-4438-808c-dd14667e9a0f",
        "gcs_url": "gs://idc_dev/bf17b0bf-aa53-4c06-a83b-a4d280d9517e.dcm",
        "license_short_name": "CC BY 3.0",
        "program": "Community",
        "source_DOI": "10.7937/K9/TCIA.2016.ELN8YGLE",
        "tcia_species": "Human",
        "tcia_tumorLocation": "Lung"
      }
    ],
    "rowsReturned": 2,
    "totalFound": 36017268
  }
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% api-method method="get" host=" " path="https://api.imaging.datacommons.cancer.gov/v1/user/account\_details" %}
{% api-method-summary %}
user/account\_details
{% endapi-method-summary %}

{% api-method-description %}
Get account information for the currently authorized user
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}
An AccountDetailsResponse object:  
{% endapi-method-response-example-description %}

```
accountDetailsResponse:
  type: "object"
  properties:
    account_details:
      type: "object"
      properties:
        date_joined:
          type: "string"
        email:
          type: "string"
        id:
          type: "string"
        last_login:
          type: "string"
        username:
          type:  "string"
        extra_data:
          type: "object"
          properties:
            id':
              type: "string"
            email:
              type: "string"
            verified_email:
              type: "boolean"
            name':
              type: "string"
            given_name:
              type: "string"
            family_name:
              type: "string"
            picture:
              type: "string"
            locale:
              type: "string"
            hd:
              type: "string"
        first_name:
          type:  "string"
        last_name:
          type: "string"
    code:
      type: "integer"
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}



