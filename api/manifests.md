# Manifests

## Manifests

A _manifest_ is a table of access methods and other metadata of the objects in some _cohort_. There are two manifest endpoints. The _**POST**_ _**/cohorts/manifest/{cohort\_id}**_ API endpoint returns a manifest of some previously defined cohort. Parameters are send to the endpoint in the request body. The JSON schema of the _**manifestBody**_ can be seen on the IDC API v2 UI page. Here is an example:

{% code overflow="wrap" %}
```
{
  "fields": [
    "Age_At_Diagnosis",
    "aws_bucket",
    "crdc_series_uuid",
    "Modality",
    "SliceThickness"
  ],
  "counts": false,
  "group_size": false,
  "sql": false,
  "page_size": 1000
}
```
{% endcode %}

The _**fields**_ parameter of the body indicates the fields whose values are to be included in the returned manifests. _The /f_**ields** API endpoint returns a list of the fields that can be included in a manifest.&#x20;

The _**counts**_, _**group\_size**_, _**sql**_ and  _**page\_size**_ parameters will be described in subsequent sections.

Every row in the returned manifest will include one value for each of the above fields.

The _**POST /cohorts/manifest/preview**_ API accepts both a _fields_ list, and a cohort definition in the _**manifestPreviewBody**_. Here is an example _manifestPreviewBody_:

```
{
  "cohort_def": {
    "name": "mycohort",
    "description": "Example description",
    "filters": {
      "collection_id": [
        "TCGA_luad",
        "%_kirc"
      ],
      "Modality": [
        "CT",
        "MR"
      ],
      "Race": [
        "WHITE"
      ],
      "age_at_diagnosis_btw": [
        65,
        75
      ]
    }
  },
  "fields": [
    "Age_At_Diagnosis",
    "aws_bucket",
    "crdc_series_uuid",
    "Modality",
    "SliceThickness"
  ],
  "counts": true,
  "group_size": true,
  "sql": true,
  "page_size": 1000
}
    
```

This endpoint behaves like the following API sequence:

```
POST /cohorts    #Create a cohort
POST /cohorts/manifest/{cohort_id} # Get a manifest for the new cohort
DELETE /cohorts/{cohort_id} # Delete the new cohort
```

That is, it behaves as if a cohort is created, a manifest for that cohort is returned and the new cohort is deleted.

The _**/cohorts/manifest/{cohort\_id}**_ endpoint returns a _**manifestResponse**_ JSON object and the _**/cohorts/manifest/preview**_ returns a _**manifestPreviewResponse**_ JSON object. Here is an example _manifestResponse_:

```
{
  "code": 200,
  "cohort_def": {
    "cohort_id": 23,
    "description": "Example description",
    "user_email": "somebody@somemail.com",
    "filterSet": {
      "filters": {
        "Modality": [
          "CT",
          "MR"
        ],
        "age_at_diagnosis_btw": [
          65,
          75
        ],
        "collection_id": [
          "tcga_luad",
          "%_kirc"
        ],
        "race": [
          "WHITE"
        ]
      },
      "idc_data_version": "16.0"
    },
    "name": "mycohort",
  },
  "manifest": {
    "manifest_data": [
      {
        "Modality": "MR",
        "SliceThickness": "10.0",
        "age_at_diagnosis": 66,
        "aws_bucket": "idc-open-data",
        "crdc_series_uuid": "09bc812b-53f7-48fc-8895-72f6b03f642b"
      },
      {
        "Modality": "CT",
        "SliceThickness": "2.5",
        "age_at_diagnosis": 66,
        "aws_bucket": "idc-open-data",
        "crdc_series_uuid": "102d676d-6c6f-4c20-bb36-77ec81b81b13"
      },
      {
        "Modality": "CT",
        "SliceThickness": "8.0",
        "age_at_diagnosis": 66,
        "aws_bucket": "idc-open-data",
        "crdc_series_uuid": "1d365f52-bff4-4348-a508-82d399ca8442"
      },   
      :
      {
        "Modality": "CT",
        "SliceThickness": "1000.090881",
        "age_at_diagnosis": 74,
        "aws_bucket": "idc-open-data",
        "crdc_series_uuid": "faa47e10-45df-44a7-9f8b-2923a41196b4"
      }
    ],
    "rowsReturned": 626,
    "totalFound": 626
  },
  "next_page": ""
}

```

The cohort definition is included so that the manifest is self-documenting. The _**manifest\_data**_ component of the _**manifest**_ component contains a row for each distinct combination of the requested fields in the cohort. The idc\_data\_version in the _**cohort\_def**_ is the IDC version when the cohort was created. To generate the manifest, the cohort's filter is applied against the data in that IDC version.

The structure of the _**manifestPreviewResponse**_ returned by the /_**cohorts/manifest/preview**_ API endpoint is identical to the _**manifestResponse**_ except that it does not have a _cohort\_id_ or _user\_email_ component.&#x20;

Because the /_**cohorts/manifest/preview**_ API endpoint is always applied against the current IDC version, the idc\_data\_version in the _**cohort\_def**_ is always that of the current IDC version.&#x20;

The _**next\_page**_ value is described in the next section.

### Groups and group\_size

We use the term _**group**_ to indicate the set of all instances in the cohort having the values of some row in the manifest. Thus the values of the first row above:

<pre><code>"Modality": "MR",
"SliceThickness": "10.0",
"age_at_diagnosis": 66,
"aws_bucket": "idc-open-data",
<strong>"crdc_series_uuid": "09bc812b-53f7-48fc-8895-72f6b03f642b" 
</strong></code></pre>

implicitly define a _group_ of instances in the cohort, each of which has those values.

When the _**group\_size**_ parameter in the _manifestBody_ or _manifestPreviewBody_ is _true,_ the resulting manifest includes the total size in bytes of the instances in the corresponding group. Following is a fragment of the manifest for the same cohort above, but where the _fields_ list includes _group\_size:_

```
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
        "age_at_diagnosis_btw": [
          65,
          75
        ],
        "collection_id": [
          "tcga_luad",
          "tcga_kirc"
        ],
        "race": [
          "WHITE"
        ]
      },
      "idc_data_version": "16.0"
    },
    "name": "mycohort",
    "sql": ""
  },
  "next_page": "",
  "manifest": {
    "manifest_data": [
      {
        "Modality": "MR",
        "SliceThickness": "10.0",
        "age_at_diagnosis": 66,
        "aws_bucket": "idc-open-data",
        "crdc_series_uuid": "09bc812b-53f7-48fc-8895-72f6b03f642b",
        "group_size": 2690320
      },
      {
        "Modality": "CT",
        "SliceThickness": "2.5",
        "age_at_diagnosis": 66,
        "aws_bucket": "idc-open-data",
        "crdc_series_uuid": "102d676d-6c6f-4c20-bb36-77ec81b81b13",
        "group_size": 42818868
      },
      {
        "Modality": "CT",
        "SliceThickness": "8.0",
        "age_at_diagnosis": 66,
        "aws_bucket": "idc-open-data",
        "crdc_series_uuid": "1d365f52-bff4-4348-a508-82d399ca8442",
        "group_size": 20064536
      },
      :
      :
      {
        "Modality": "CT",
        "SliceThickness": "1000.090881",
        "age_at_diagnosis": 74,
        "aws_bucket": "idc-open-data",
        "crdc_series_uuid": "faa47e10-45df-44a7-9f8b-2923a41196b4",
        "group_size": 6518724
      }
    ],
    "rowsReturned": 626,
    "totalFound": 626
  },
  "next_page": ""
}
```

Here we see that the instances in the group corresponding to the first result row have a total size of 2,690,320B.

The _**totalFound**_ value at the end of the manifest tells us that there are 626 rows in the manifest, meaning the manifest contains 626 different combinations of _Modality, SliceThickness, age\_at\_diagnosis, aws\_bucket,_ and crdc\_series uuid. (The group size does not add to the combinatorics.) The _**rowsReturned**_ value indicates that all the rows in the manifest were return in the first "page". If not all the rows had been returned, we can ask for additional "pages" as described in the next section.

The _group\_size_ parameter is optional and defaults to _false_ .

### Manifest granularity

If the _**counts**_ parameter is _true_, the resulting manifest will selectively include counts of the instances, series, studies, patients and collections in each group. Which counts are included in a manifest is determined by the _**granularity**_ and which, in turn, is determined by certain of the possible fields in the _fields_ parameter list of the _**manifestBody**_ or _**manifestPreviewBody**_.

For example, if the _fields_ parameter list includes the SOPInstanceUID field, there will one group per instance in the manifest. Thus the manifest has _**instance granularity**_. A manifest has one of instance, series, study, patient, collection or version granularity.

For a given manifest granularity,  and when _**counts**_ is True, counts of the "lower level" objects are reported in the manifest. Thus, if a cohort has _**series granularity**_, then the count of all instances in each group is reported. If a cohort has study granularity, then the count of all instances in each group and of all series in each group is reported. And so on. This is described in detail in the remainder of this section.

In the following, manifest examples are based on this _filterSet:_



```
   "filters": {
      "collection_id": [
        "tcga_luad",
        "tcga_kirc"
      ],
      "Modality": [
        "CT",
        "MR"
      ],
      "Race": [
        "WHITE"
      ],
      "age_at_diagnosis_btw": [
        65,
        75
      ]
    }

```

#### Instance granularity

A manifest will have _**instance granularity**_ if the _fields_ parameter list includes one or both of the fields:

* _**SOPInstanceUID**_
* _**crdc\_instance\_uuid**_

Both of these fields are unique to each instance. Therefore the resulting manifest will include one row for each instance in the specified cohort. For example, the following _**fields**_ list will result in a manifest having a row per instance:

```
{
  "fields": [
    "SOPInstanceUID",
    "Modality",
    "SliceThickness"
  ]
}
```

Each row will include the _SOPInstanceUID, Modality_ and _SliceThickness_ of the corresponding instance.

_The counts_ parameter is ignored because there are no 'lower level' objects than instances,&#x20;

#### Series granularity

A manifest will have _**series granularity**_ if it goes not have _**instance granularity**_ and the _**fields**_ parameter list includes one or more of thee field:

* _SeriesInstanceUID_
* _crdc\_series\_uuid_

&#x20;Both of these fields are unique to each series, and therefore the resulting manifest will include _at least_ one row per series in the specified cohort. For example, the following _fields_ list will result in a manifest having one or more rows per series:

<pre><code><strong>"fields": [
</strong>  "Modality",
  "SliceThickness",
  "collection_id",
  "patientID",
  "StudyInstanceUID",
  "SeriesInstanceUID"
]
</code></pre>

Because the _SeriesInstanceUID_ is unique to each series in a cohort (more accurately, all instances in a series have the same _SeriesInstanceUID_), there will be at least one row per series in the resulting manifest. However, _SliceThickness_ is not necessarily unique across all instance in a series. Therefore, the resulting manifest may have multiple rows for a given series...rows in which the _SeriesInstanceUID_ is the same but the _SliceThickness_ values differ. DICOM _modality_ should always be the same for all instances in a series; therefore it is not expected to result in multiple rows per series.

If the _counts_ parameter is _true_, each row of the manifest will have:

* an _**instance\_count**_ value that is the count of instances in the _group_ corresponding to the row

If the above _**fields**_ then this is a fragment of the _series granularity_ manifest of our example cohort:

<pre><code>{
  "code": 200,
  "cohort_def": {
    "description": "Example description",
    "filterSet": {
      "filters": {
        "Modality": [
          "CT",
          "MR"
        ],
        "age_at_diagnosis_btw": [
          65,
          75
        ],
        "collection_id": [
          "tcga_luad",
          "tcga_kirc"
        ],
        "race": [
          "WHITE"
        ]
      },
      "idc_data_version": "16.0"
    },
    "name": "mycohort",
    "sql": ""
  },
  "manifest": {
    "manifest_data": [
      {
        "Modality": "CT",
        "PatientID": "TCGA-50-6592",
        "SeriesInstanceUID": "1.3.6.1.4.1.14519.5.2.1.6450.9002.141004994853145237754973938025",
        "SliceThickness": null,
        "StudyInstanceUID": "1.3.6.1.4.1.14519.5.2.1.6450.9002.256822832756566055874151999412",
        "collection_id": "tcga_luad",
        "instance_count": "151"
      },
      {
        "Modality": "CT",
        "PatientID": "TCGA-50-6592",
        "SeriesInstanceUID": "1.3.6.1.4.1.14519.5.2.1.6450.9002.212096199865546132848990878032",
        "SliceThickness": null,
        "StudyInstanceUID": "1.3.6.1.4.1.14519.5.2.1.6450.9002.256822832756566055874151999412",
        "collection_id": "tcga_luad",
        "instance_count": "61"
      },
      {
        "Modality": "CT",
        "PatientID": "TCGA-50-6595",
        "SeriesInstanceUID": "1.3.6.1.4.1.14519.5.2.1.6450.9002.829269157955398706933292266867",
        "SliceThickness": "0.578125",
        "StudyInstanceUID": "1.3.6.1.4.1.14519.5.2.1.6450.9002.414530650520592976265083061155",
        "collection_id": "tcga_luad",
        "instance_count": "1"
      },
      :
      :
      {
        "Modality": "MR",
        "PatientID": "TCGA-B0-5109",
        "SeriesInstanceUID": "1.3.6.1.4.1.14519.5.2.1.6450.4004.370888372270096165934432087127",
        "SliceThickness": "20.0",
        "StudyInstanceUID": "1.3.6.1.4.1.14519.5.2.1.6450.4004.167173047835125001355984228239",
        "collection_id": "tcga_kirc",
        "instance_count": "50"
      }
    ],
    "rowsReturned": 742,
    "totalFound": 742
  }
<strong>  "next_page": ""
</strong><strong>}
</strong></code></pre>

This tells us that the group of instances corresponding to the first row of the manifest results has 151 members.

#### Study Granularity

A manifest will have _**study granularity**_ if it goes _not_ have _**series**_** or **_**instance granularity**_ and the queryFields list includes one or more of the fields:

* _StudyInstanceUID_
* _crdc\_study\_uuid_

Both of these fields are unique to each study, and therefore the resulting manifest will include _at least_ one row per study in the specified cohort. For example, the following _fields_ list will result in a manifest having a one or more  rows per study:

<pre><code><strong>"fields": [
</strong><strong>    "Modality",
</strong>    "SliceThickness",
    "collection_id",
    "patientID",
    "StudyInstanceUID",
    "group_size",
    "counts"
]
</code></pre>

Similarly, _SliceThickness_ can vary not only among the instances in a series, but among series in a study. Therefore, the resulting manifest may have multiple rows for a study, and which differ from each other in both _SliceThickness_ and _Modality._

If _counts_ is in the _fields_ list, each row of the manifest will have:

* an _instance\_count_ value that is the count of instances in the _group_ corresponding to the row
* a _series\_count_ value that is the count of series in the _group_ corresponding to the row

If the fields list is as above, then this is a fragment of the _study granularity_ manifest of our example cohort:

<pre><code>{
  "code": 200,
  "cohort_def": {
    "description": "Example description",
    "filterSet": {
      "filters": {
        "Modality": [
          "CT",
          "MR"
        ],
        "age_at_diagnosis_btw": [
          65,
          75
        ],
        "collection_id": [
          "tcga_luad",
          "tcga_kirc"
        ],
        "race": [
          "WHITE"
        ]
      },
      "idc_data_version": "16.0"
    },
    "name": "mycohort",
    "sql": ""
  },
<strong>  "manifest": {
</strong>    "manifest_data": [
      {
        "Modality": "CT",
        "PatientID": "TCGA-50-6592",
        "SliceThickness": null,
        "StudyInstanceUID": "1.3.6.1.4.1.14519.5.2.1.6450.9002.256822832756566055874151999412",
        "collection_id": "tcga_luad",
        "instance_count": 212,
        "series_count": 2
      },
      {
        "Modality": "CT",
        "PatientID": "TCGA-50-6595",
        "SliceThickness": "0.578125",
        "StudyInstanceUID": "1.3.6.1.4.1.14519.5.2.1.6450.9002.414530650520592976265083061155",
        "collection_id": "tcga_luad",
        "instance_count": 1,
        "series_count": 1
      },
      {
        "Modality": "CT",
        "PatientID": "TCGA-B8-4153",
        "SliceThickness": "0.6",
        "StudyInstanceUID": "1.3.6.1.4.1.14519.5.2.1.8421.4004.499780439902438461273732269226",
        "collection_id": "tcga_kirc",
        "instance_count": 2,
        "series_count": 1
      },
      :
      :
      {
        "Modality": "MR",
        "PatientID": "TCGA-B0-5109",
        "SliceThickness": "20.0",
        "StudyInstanceUID": "1.3.6.1.4.1.14519.5.2.1.6450.4004.167173047835125001355984228239",
        "collection_id": "tcga_kirc",
        "instance_count": 100,
        "series_count": 2
      }
    ],
    "rowsReturned": 324,
    "totalFound": 324
  },
  "next_page": ""
}
</code></pre>

This tells us that the group of instances corresponding to the first row of the manifest results has 212 members, divided among two series. The group of instances corresponding to the third row of the manifest results has two members in a single series.

#### Patient Granularity

A manifest will have _**patient granularity**_ if it goes not have _study_, _series_ or _instance granularity_ and the _fields_ list includes the field _PatientID._ This field is unique to each patient, and therefore the resulting manifest will include _at least_ one row per patient in the specified cohort. For example, the following _fields_ list will result in a manifest having a one or more  rows per study:

```
"fields": [
    "Modality",
    "SliceThickness",
    "collection_id",
    "patientID",
    "group_size",
    "counts"
]
```

Because the _PatientID_ is unique to each patient in a cohort (more accurately, all instances in a study have the same _PatientID_), there will be at least one row per patient in the resulting manifest. It is common for a patient's series to examine different body parts. Therefore, the resulting manifest may well have more than one row per patient.

If _counts_ is in the _fields_ list, each row of the manifest will have:

* an _instance\_count_ value that is the count of instances in the group corresponding to the row
* a _series\_count_ value that is the count of series in the group corresponding to the row
* a _study\_count_ value that is the count of studies in the group corresponding to the row

If the fields list is as above, then this is a fragment of the _**patient granularity**_ manifest of our example cohort:

```
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
        "age_at_diagnosis_btw": [
          65,
          75
        ],
        "collection_id": [
          "tcga_luad",
          "tcga_kirc"
        ],
        "race": [
          "WHITE"
        ]
      },
      "idc_data_version": "16.0"
    },
    "name": "mycohort",
    "sql": ""
  },
  "next_page": "",
  "manifest": {
    "manifest_data": [
     {
        "Modality": "CT",
        "PatientID": "TCGA-50-6592",
        "SliceThickness": null,
        "collection_id": "tcga_luad",
        "instance_count": "212",
        "series_count": "2",
        "study_count": "1"
      },
      {
        "Modality": "CT",
        "PatientID": "TCGA-50-6595",
        "SliceThickness": "0.578125",
        "collection_id": "tcga_luad",
        "instance_count": "1",
        "series_count": "1",
        "study_count": "1"
      },
      {
        "Modality": "CT",
        "PatientID": "TCGA-B8-4153",
        "SliceThickness": "0.6",
        "collection_id": "tcga_kirc",
        "instance_count": "6",
        "series_count": "2",
        "study_count": "2"
      },
      :
      :
      {
        "Modality": "MR",
        "PatientID": "TCGA-B0-5109",
        "SliceThickness": "20.0",
        "collection_id": "tcga_kirc",
        "instance_count": "100",
        "series_count": "2",
        "study_count": "1"
      }
    ],
    "rowsReturned": 301,
    "totalFound": 301
  }
}
```

This tells us that the group of instances corresponding to the first row of the manifest results has 212 members divided among two series, and both in a single study.

#### Collection Granularity

A manifest will have _**collection granularity**_ if it goes not have _patient,_ _study,_ _series_ or _instance granularity_ and the _fields_ parameter list includes the field _collection\_id._ This field is unique to each collection, and therefore the resulting manifest will include _at least_ one row per collection in the specified cohort. For example, the following _fields_ list will result in a manifest having a one or more  rows per study:

```
"fields": [
    "Modality",
    "SliceThickness",
    "collection_id",
    "patientID",
    "group_size",
    "counts"
]
```

Because the _collection\_id_ is unique to each collection in a cohort (more accurately, all instances in a collection have the same _collection\_id_), there will be at least one row per collection in the resulting manifest. It is common for a collection to have patients of different ages. Therefore, the resulting manifest may well have more than one row per patient.

If the fields list is as follows:

then this is a fragment of the _collection granularity_ manifest of our example cohort:

```
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
        "age_at_diagnosis_btw": [
          65,
          75
        ],
        "collection_id": [
          "tcga_luad",
          "tcga_kirc"
        ],
        "race": [
          "WHITE"
        ]
      },
      "idc_data_version": "16.0"
    },
    "name": "mycohort",
    "sql": ""
  },
  "manifest": {
    "manifest_data": [
      {
        "Modality": "CT",
        "SliceThickness": null,
        "collection_id": "tcga_luad"
        "instance_count": "212",
        "patient_count": "1",
        "series_count": "2",
        "study_count": "1"
      },
      {
        "Modality": "CT",
        "SliceThickness": "0.578125",
        "collection_id": "tcga_luad",
        "instance_count": "1",
        "patient_count": "1",
        "series_count": "1",
        "study_count": "1"
      },
      {
        "Modality": "CT",
        "SliceThickness": "0.6",
        "collection_id": "tcga_kirc",
        "instance_count": "29",
        "patient_count": "9",
        "series_count": "16",
        "study_count": "14"
      },
      :
      :
      {
        "Modality": "MR",
        "SliceThickness": "20.0",
        "collection_id": "tcga_kirc",
        "instance_count": "100",
        "patient_count": "1",
        "series_count": "2",
        "study_count": "1"
      }
    ],
    "rowsReturned": 88,
    "totalFound": 88
  }
  "next_page": "",
}
```

#### Version granularity

A manifest will have _**version granularity**_ if it does not have collection, _patient_, _study_, _series_ or _instance granularity._ At this granularity level, the rows in the manifest return the combinations of queried values across all collects, patients, studies, series and instances in the cohort.&#x20;

When the _fields_ list is as follows:

```
"fields": [
    "Modality",
    "SliceThickness",
    "patientID",
    "group_size",
    "counts"
]
```

then this is a fragment of the _version granularity_ manifest of our example cohort:

```
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
        "age_at_diagnosis_btw": [
          65,
          75
        ],
        "collection_id": [
          "tcga_luad",
          "tcga_kirc"
        ],
        "race": [
          "WHITE"
        ]
      },
      "idc_data_version": "16.0"
    },
    "name": "mycohort",
    "sql": ""
  },
  "manifest": {
    "manifest_data": [
      {
        "Modality": "CT",
        "SliceThickness": null,
        "collection_count": "1",
        "instance_count": "212",
        "patient_count": "1",
        "series_count": "2",
        "study_count": "1"
      },
      {
        "Modality": "CT",
        "SliceThickness": "0.578125",
        "collection_count": "1",
        "instance_count": "1",
        "patient_count": "1",
        "series_count": "1",
        "study_count": "1"
      },
      {
        "Modality": "CT",
        "SliceThickness": "0.6",
        "collection_count": "2",
        "instance_count": "34",
        "patient_count": "11",
        "series_count": "19",
        "study_count": "17"
      },
      {
      :
      :
      {
        "Modality": "MR",
        "SliceThickness": "20.0",
        "collection_count": "1",
        "instance_count": "100",
        "patient_count": "1",
        "series_count": "2",
        "study_count": "1"
      }
    ],
    "rowsReturned": 87,
    "totalFound": 87
  }
  "next_page": "",
}
```

Row one of the results tells us that the cohort has 212 instances having a Null SliceThickness and modality="CT". Also, there are apparently 87 different combinations of Modality and SliceThickness in the cohort as shown by the _**totalFound**_ value.
