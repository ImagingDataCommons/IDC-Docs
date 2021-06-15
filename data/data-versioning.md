# Data Versioning

The IDC obtains curated DICOM radiology image and analysis data from The Cancer Imaging Archive \(TCIA\). In the future, IDC will obtain radiology, pathology and microscopy image data from TCIA and additional sources. Data from all these sources evolves over time as new data is added \(common\), existing files are corrected \(rare\), or data is removed \(extremely rare\).

Users interact with the IDC portal and API to define cohorts, and then perform analyses on these cohorts. The goal of IDC versioning is to create “snapshots” over time of the entirety of the evolving IDC imaging data, such that searching an IDC version according to some criteria \(creating a cohort\) will always identify exactly the same set of objects. Here “identify” particularly means providing URLs or other access methods to the corresponding physical data objects.

In order to reproduce the result of such analysis, it must be possible to precisely recreate a cohort. For this purpose an IDC cohort is specified and saved as a filter applied against a specified IDC data version. Because an IDC version exactly defines the set of data against which the filter is applied, and because all versions of all data, except data removed due to PHI/PII concerns, should continue to be available, a cohort is therefore reproducible.

Saving a cohort as a \(filter, IDC data version\) pair is much more efficient than saving a list of potentially millions of instances. Moreover, defining a cohort in terms of a filter explicitly conveys how cohort items were selected. The ability to reproduce a cohort in this way also enables refining the cohort by applying the original or a modified filter against the same or a future IDC data version.

## DICOM Entities are versioned

Additional DICOM series are sometimes added to a DICOM study over time while keeping the StudyInstanceUID unchanged, and/or a series may be added or removed from some study. Similarly, TCIA may, from time to time, correct details in the metadata of an instance while keeping the SOPInstanceUID of such an instance unchanged. These and other possible changes mean that DICOM instances, series and studies can change from one IDC data version to the next. That is, instances, series and studies have versions.

Because DICOM SOPInstanceUIDs, SeriesInstanceUIDs or StudyInstanceUIDs typically remain invariant even when the composition of an instance, series or study changes, IDC assigns each version of an instance, series or study a [_UUID_](https://en.wikipedia.org/wiki/Universally_unique_identifier#:~:text=A%20universally%20unique%20identifier%20%28UUID,%2C%20for%20practical%20purposes%2C%20unique.) to uniquely identify it and differentiate it from other versions of the same DICOM object.

The data in each IDC version, then, can be thought of as some set of versioned DICOM instances, series and studies. This set is defined in terms of the corresponding set of instance UUIDs, series UUIDs and study UUIDs. This means that if, e.g., some version of an instance having UUID _UUIDx_ that was in IDC version _Vm_ is changed, a new UUID, _UUIDy_, will be assigned to the new instance version. Subsequent IDC versions, _Vm+1_, _Vm+2, ..._ will include that new instance version identified by _UUIDy_ until that instance is again changed. Similarly if the composition of some series changes, either because an instance in the series is changed, or an instance is added or removed from that series, a new UUID is assigned to the new version of that series and identifies that version of the series in subsequent IDC versions. Similarly, a study is assigned a new UUID when its composition changes.

Note that instances, series and studies do not have an explicit version numbers. Versioning of an object is implicit in the associated UUIDs.

As we will see in [Organization of data](organization-of-data-1.md), the UUID of an instance is used in forming the GCS object name of the corresponding GCS object. Series and studies do not have corresponding GCS objects but each instance, series and study has a corresponding GA4GH DRS object, identified by a GUID based on the instance's, series's or study's UUID. Refer to the [GA4GH DRS Objects](ga4gh-drs-objects.md) section for details.

