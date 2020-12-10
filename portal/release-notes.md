# Portal Release Notes

{% hint style="warning" %}
The version of the portal is shown at the bottom of the portal page. The semantics of the version is the following:

_canceridc.&lt;date of webapp deployment in YYYYMMDDHHMM&gt;.&lt;first 6 characters of the commit hash&gt;_,

where revision hash corresponds to that of the [IDC WebApp repo](https://github.com/ImagingDataCommons/IDC-WebApp).
{% endhint %}

## 1.1.0 - December 2020 \(canceridc.202012091728.674fff0\)

The Imaging Data Commons Explore Image Data portal is a platform that allows users to explore, filter, create cohorts, and view image studies and series using cutting-edge technology viewers.

Main highlights of this release include:

* Case-level table is added to the portal.
* Cohorts can now be exported into BigQuery tables using the Export Cohort Manifest button
* Cohorts less than 650k rows can now be downloaded as a multipart file. Cohorts larger that 600k rows can only be be exported to BigQuery \(for users that are logged in with Google Accounts\).
* Quantitative filter ranges are updated dynamically with the updates to filter selection.
* Pie charts will display "No data available" message when zero cases are returned for the given filter selection.
* RTPLAN and Real World Mapping Attribute values are now disabled at the series level, since they cannot be visualized in the IDC Viewer.
* Various bug fixes in both the IDC Portal and IDC Viewer.

## 1.0.0 - October 2020 \(canceridc.202010190226.4e8597\)

The Imaging Data Commons Explore Image Data portal is a platform that allows users to explore, filter, create cohorts, and view image studies and series using cutting-edge technology viewers.

Main features in this initial release include:

* The ability to search for data in BigQuery and Solr
* The ability to search by multiple attributes:
  * Collection
  * Original attributes e.g., Modality
  * Derived attributes e.g., Segmentations 
  * Qualitative analysis e.g., Lobular pattern
  * Quantitative analysis e.g., Volume
  * related attributes e.g., Country
* Display of collections results in a tabular format with the following information:
  * Collection Name
  * Total Number of Cases
  * Number of Cases\(this cohort\)
* Display of the Selected Studies results in tabular format with the following information:
  * Project Name
  * Case ID
  * Study ID
  * Study Description
* Display of the Selected Series results in tabular format with the following information:
  * Study ID
  * Series Number
  * Modality
  * Body Part Examined
  * Series Description
* The ability to hide attributes with zero cases present
* The ability to save cohorts
* The ability to download the manifest of any cohort created
* The ability to promote, filter, and load multiple series instances in the OHIF viewer



