# Portal Known Issues

## 1.0.0 - October 2020

The Imaging Data Commons Explore Image Data portal is a platform that allows users to explore, filter, create cohorts, and view image studies and series using cutting-edge technology viewers.

### Initial release

**Data Exploration**

* Lack of consistent punctuation throughout the web app
  * [GitHub ticket \#414](https://github.com/ImagingDataCommons/IDC-WebApp/issues/414)
* Attribute values appear as blank when zero cases are present for the Search Configuration panel
  * [GitHub ticket \#423](https://github.com/ImagingDataCommons/IDC-WebApp/issues/423)
* An attribute selection combined with a Quantitative Analysis attribute selection will display all value options when no cases are available in certain use cases
  * [GitHub ticket \#341](https://github.com/ImagingDataCommons/IDC-WebApp/issues/341)
* Attribute slider for Quantitative values will display with zero cases present in attribute selection
  * [GitHub ticket \#425](https://github.com/ImagingDataCommons/IDC-WebApp/issues/425) 
* The selection of the Age at Diagnosis None attribute is not reflected in the cohorts confirmation pop-up or the cohort details page
  * [GitHub ticket \#420](https://github.com/ImagingDataCommons/IDC-WebApp/issues/420)
* The download of the cohort manifest is truncated at 65,000 records ordered by PatientID, StudyID, SeriesID, and InstanceID
  * [GitHub ticket \#394](https://github.com/ImagingDataCommons/IDC-WebApp/issues/394)
* Duplication of value Digital Mammography X-Ray Image  for Object Class attribute present in the data portal under the original tab
  * [GitHub ticket \#233](https://github.com/ImagingDataCommons/IDC-WebApp/issues/233)

More detailed information can be found on GitHub under the ImagingDataCommons/Web-App repository. All tickets related to known issues within the data portal are labeled with either a [bug](https://github.com/ImagingDataCommons/IDC-WebApp/issues?q=is%3Aissue+is%3Aopen+label%3Abug) or an [enhancement](https://github.com/ImagingDataCommons/IDC-WebApp/issues?q=is%3Aissue+is%3Aopen+label%3Aenhancement) label.

**Viewer**

* Selection of the Object class real-world mapping attribute will cause the OHIF viewer not to load
* When working with the Safari browser, the 2D MPR functionality causes the viewer to freeze. We recommend using Google Chrome or Firefox when working with the Imaging Data Commons
* Tag viewer version is currently broken for the OHIF viewer
* The occasional return of a Chunk Load Error on the viewer load not handled correctly and will cause the viewer to fail
* The scroll bar is missing in the Tag viewer window
* When working with RT Structure sets, the RightSidePanel: Cannot read property 'SeriesInstanceUID' of undefined error returned in the viewer
* On occasion, the RTSTRUCT tab is shown when not applicable, does not load when opened
* The OHIF viewer doesn't load  kidney and tumor plane segmentations found in TCIA
* Unable to display the same scan in two viewports and displaying different RTSTRUCTs, SEGs, and cornerstone planar measurements in each
* SVG Crosshair issues in VTK
* An old version of the OHIF viewer will cause the page not to load. A hard refresh is needed to resolve the issue.

More detailed information can be found in GitHub within the OHIF/Viewers repository. All tickets related to the Imaging Data Commons are labeled with either an [IDC:candidate](https://github.com/OHIF/Viewers/labels/IDC%3Acandidate) or an [IDC:priority](https://github.com/OHIF/Viewers/labels/IDC%3Apriority) label.

