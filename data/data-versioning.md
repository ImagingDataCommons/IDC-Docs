# Data versioning

The IDC obtains curated DICOM radiology, pathology and microscopy image and analysis data from The Cancer Imaging Archive (TCIA) and additional sources. Data from all these sources evolves over time as new data is added (common), existing files are corrected (rare), or data is removed (extremely rare).

Users interact with IDC using one of the following interfaces to define cohorts, and then perform analyses on these cohorts:

* [IDC Portal](https://portal.imaging.datacommons.cancer.gov/explore/) directly or using [IDC API](https://learn.canceridc.dev/api/getting-started): while this approach is most convenient, it allows searching using a small subset of attributes, defines cohorts only in terms of cases that meet the defined criteria, and has very limited options for combining multiple search criteria
* [IDC BigQuery](https://console.cloud.google.com/bigquery?p=bigquery-public-data\&d=idc\_current\&t=dicom\_all\&page=table) tables via [SQL interface](https://cloud.google.com/bigquery/docs/reference/standard-sql/introduction): this approach is most powerful, as it allows the use of [any of the DICOM metadata attributes](https://cloud.google.com/healthcare-api/docs/how-tos/dicom-bigquery-schema) to define the cohort, while leveraging the expressiveness of SQL in defining the selection logic, and allows to define cohort at any level of the data model hierarchy (i.e., instances, series, studies or cases)

The goal of IDC versioning is to create a series of "snapshots” over time of the entirety of the evolving IDC imaging dataset, such that searching an IDC version according to some criteria (creating a cohort) will always identify exactly the same set of objects. Here “identify” particularly means providing URLs or other access methods to the corresponding physical data objects.

In order to reproduce the result of such analysis, it must be possible to precisely recreate a cohort. For this purpose an IDC cohort as defined in the Portal is specified and saved as a filter applied against a specified IDC data version. Alternatively, the cohort can be defined as an SQL query, or as a list of unique identifiers selecting specific files within a defined data release version.

Because an IDC version exactly defines the set of data against which the filter/query is applied, and because all versions of all data, except data removed due to PHI/PII concerns, should continue to be available, a cohort is therefore persistent over the course of the evolution of IDC data.

## DICOM Entities are versioned

There are various reasons that can cause modification of the existing collections in IDC:

* images for new patients can be added to an existing collections;
* additional DICOM series are sometimes added to a DICOM study over time (i.e., those that contain new annotations or analysis results);&#x20;
* a series may be added or removed from an existing study;
* metadata of an existing instance might be corrected (which may or may not lead to an update of the DICOM `SOPInstanceUID` corresponding to the instance).&#x20;

These and other possible changes mean that DICOM instances, series and studies can change from one IDC data version to the next, while their DICOM UIDs remain unchanged. This motivates the need for maintaining versioning of the DICOM entities.

Because DICOM `SOPInstanceUIDs`, `SeriesInstanceUIDs` or `StudyInstanceUIDs` can remain invariant even when the composition of an instance, series or study changes, IDC assigns each version of each instance, series or study a [_UUID_](https://en.wikipedia.org/wiki/Universally\_unique\_identifier) to uniquely identify it and differentiate it from other versions of the same DICOM object.

{% hint style="info" %}
It is very important to appreciate the difference between DICOM Unique Identifiers (UIDs) and CRDC Universally Unique Identifiers (UUIDs) assigned at the various levels of the DICOM hierarchy:

* **DICOM UID**s are available as DICOM metadata attributes within the DICOM files for each DICOM Study, Series and Instance. Those UIDs follow the conventions of the DICOM UI Value Representation. DICOM UIDs are not versioned. I.e., if a DICOM study is augmented with a new DICOM series, DICOM `StudyInstanceUID` will not change. If an instance within an existing DICOM series is modified, DICOM `SeriesInstanceUID` or the `SOPInstanceUID` of the modified instance may or may not change.
* **IDC UUID**s are **not** available as DICOM metadata attributes - they are generated for the DICOM studies, series and instances at the time of data ingestion, and are available in the IDC BigQuery tables. IDC UUIDs are tied to the content of the entity they correspond to. I.e., if anything within a DICOM study/series/instance is changed in a given IDC data release, a new UUID at the corresponding level of data hierarchy will be generated, while the previous version will be indexed and available via the prior UUID.&#x20;
{% endhint %}

The data in each IDC version, then, can be thought of as some set of versioned DICOM instances, series and studies. This set is defined in terms of the corresponding set of instance UUIDs, series UUIDs and study UUIDs. This means that if, e.g., some version of an instance having UUID _UUIDx_ that was in IDC version _Vm_ is changed, a new UUID, _UUIDy_, will be assigned to the new instance version. Subsequent IDC versions, _Vm+1_, _Vm+2, ..._ will include that new instance version identified by _UUIDy_ unless and until that instance is again changed. Similarly if the composition of some series changes, either because an instance in the series is changed, or an instance is added or removed from that series, a new UUID is assigned to the new version of that series and identifies that version of the series in subsequent IDC versions. Similarly, a study is assigned a new UUID when its composition changes.

A corollary is that only a single version of an instance, series or study is in an IDC version.

Note that instances, series and studies do not have an explicit version number in their metadata. Versioning of an object is implicit in the associated UUIDs.

As we will see in [Organization of data](organization-of-data/organization-of-data-v1.md), the UUID of a (version of an) instance, and the UUID of the (version of a) series to which it belongs, are used in forming the object (file) name of the corresponding GCS and AWS objects. In addition, each instance version has a corresponding GA4GH DRS object, identified by a GUID based on the instance version's UUID. Refer to the [GA4GH DRS Objects](organization-of-data/organization-of-data-v2-through-v13-deprecated/guids-and-uuids.md) section for details.
