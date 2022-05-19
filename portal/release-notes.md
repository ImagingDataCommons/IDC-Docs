# Portal release notes

{% hint style="warning" %}
The version of the portal is shown at the bottom of the portal page. The semantics of the version is the following:

_canceridc.\<date of webapp deployment in YYYYMMDDHHMM>.\<first 6 characters of the commit hash>_,

where revision hash corresponds to that of the [IDC WebApp repo](https://github.com/ImagingDataCommons/IDC-WebApp).
{% endhint %}


## 9.0 - May 19, 2022 (canceridc.202205191051)

Main highlights of this release include:

* Ability to select specific Analysis Results collections with segmentation and radiomic features

* Text boxes added to the slider panels to allow the user to input upper and lower slider bounds 

* Pie chart tooltips updated to improve viewability

## 8.0 - April 4, 2022 (canceridc.202204050856.2920c81)

Main highlights of this release include:

* Eleven new collections added

* Number of cases, studies, and series in a cohort are reported in the filter de
finition

* On the Exploration page the Access attribute is placed in the Search Scope

* On the Exploration page users are warned when they create a cohort that includes Limited Access collections

* Series Instance UID is reported in the Selected Series table



## 7.0 - February 7, 2022 (canceridc.202202071117.164252a)

Main highlights of this release include:



* The BigQuery query string corresponding to a cohort can now be displayed in user-readable format by pressing a button on either the cohort or cohort list pages

* On the exploration page collections can now be sorted alphabetically or by the number of cases. Selected cases are ordered at the top of the collection list

* Table rows can be selected by clicking anywhere within the row, not just on the checkbox

* The BigQuery export cohort manifest includes the IDC data version as an optional column  
 



## 6.0 - January 10, 2022 (canceridc.202201101504.eb0e309)

Main highlights of this release include:

* Collections which have limited access are now denoted as such in the Collection tab on the Exploration page&#x20;
* Links to image files belonging to limited collections have been removed from the Studies and Series tables on the Exploration page&#x20;
* The quota of image file data that can be served per user per day has been reduced from 137 to 40 GB

## 5.0 - December 9, 2021 (canceridc.202112091128.eb0e309)

Main highlights of this release include:

* New attributes including Manufacturer, Manufacturer Model Name, and Slice Thickness added&#x20;
* Checked attribute values are now shown at the top of the attribute value lists&#x20;
* Ability to search by CaseID added to the Selected Cases table&#x20;
* Ability to search by StudyID added to the Selected Studies table&#x20;
* Study Date added to the Studies Table&#x20;
* Changed the persistence of the StudyID tooltip in the tables so that the StudyID can be copied from the tooltip&#x20;
* Specific columns can now be selected in the BigQuery cohort export

## 2.1.0 - August 2021 (canceridc.202108261153.70f59e0)

The Imaging Data Commons Explore Image Data portal is a platform that allows users to explore, filter, create cohorts, and view image studies and series using cutting-edge technology viewers.

Main highlights of this release include:

* Support for slide microscopy series from the CPTAC-LSCC and CPTAC-LUAD collections is now included.
* The [Slim viewer](https://github.com/MGHComputationalPathology/slim) is now configured to view slide microscopy series
* Search boxes are included for very attribute to search for specific attribute values by name.

## 2.0.0 - June 2021 - (canceridc.202106250849.876f912)

The Imaging Data Commons Explore Image Data portal is a platform that allows users to explore, filter, create cohorts, and view image studies and series using cutting-edge technology viewers.

Main highlights of this release include:

* 112 data collections are now included
* Cohort data version is reported
* Cohort statistics - ie the number the cases, studies, and series per cohort are now reported
* Mechanism included to update a version cohort
* Species Attribute is included
* Checkbox and plus/minus icons are now used to select table rows

## 1.3.0 - March 2021 (canceridc.202103011131.27ce3b3)

The Imaging Data Commons Explore Image Data portal is a platform that allows users to explore, filter, create cohorts, and view image studies and series using cutting-edge technology viewers.

Main highlights of this release include:

* The user details page will no longer return a 500 error when selected
* Sorting of studies panel is now active for all fields
* Re-sending of an unreceived verification email is now more clearly explained.
* IDC identity login header and column selection is disabled for the exportation of a cohort manifest to BigQuery
* Detailed information panel added to efficiently describe why some pie charts have multiple facets even when a filter is selected
* Cohort manifest export popup can be scrolled down
* Use of Shift or Control (Command for Mac) selection of studies will now behave as expected: Shift-select for a contiguous series of rows, Control/Command-select for individual rows.
* All filter selections are now sorted by alphabetical characters

## 1.2.0 - January 2021 (canceridc.202101111506.0a8af57)

The Imaging Data Commons Explore Image Data portal is a platform that allows users to explore, filter, create cohorts, and view image studies and series using cutting-edge technology viewers.

Main highlights of this release include:

* Consistent number of files will be returned between the portal and BigQuery
* When the user clicks a non-gov link a popup will appear
* Cohort manifest export information now has clickable URLs to take you to the BigQuery console
* Collections list displays by default 100 entries
* Any empty search criteria is now highlighted in grey and no data will be listed
* The user will no longer need to scroll to see search criteria in the left search configuration panel
* Portal footer is now in compliance with NCI requirements
* Check/uncheck in the collections panel added for collection TCGA

## 1.1.0 - December 2020 (canceridc.202012091728.674fff0)

The Imaging Data Commons Explore Image Data portal is a platform that allows users to explore, filter, create cohorts, and view image studies and series using cutting-edge technology viewers.

Main highlights of this release include:

* Case-level table is added to the portal
* Cohorts can now be exported into BigQuery tables using the Export Cohort Manifest button
* Cohorts less than 650k rows can now be downloaded as a multipart file. Cohorts larger that 600k rows can only be be exported to BigQuery (for users that are logged in with Google Accounts)
* Quantitative filter ranges are updated dynamically with the updates to filter selection
* Pie charts will display "No data available" message when zero cases are returned for the given filter selection
* RTPLAN and Real World Mapping Attribute values are now disabled at the series level, since they cannot be visualized in the IDC Viewer
* Various bug fixes in both the IDC Portal and IDC Viewer

## 1.0.0 - October 2020 (canceridc.202010190226.4e8597)

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
  * Number of Cases(this cohort)
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
