# Portal Known Issues

## 1.0.0 - October 2020

The Imaging Data Commons Explore Image Data portal is a platform that allows users to explore, filter, create cohorts, and view image studies and series using cutting-edge technology viewers.

### Initial release

### **Data Exploration**

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

### **Image Visualization**

* The about box of the OHIF viewer reports the incorrect version number
  * [GitHub ticket \#2203](https://github.com/OHIF/Viewers/issues/2023)
* Selection of the Object class real-world mapping attribute will cause the OHIF viewer not to load
  * [GitHub ticket \#332](https://github.com/ImagingDataCommons/IDC-WebApp/issues/332)
* When working with the Safari browser, the 2D MPR functionality causes the viewer to freeze. We recommend using Google Chrome or Firefox when working with the Imaging Data Commons
  * [GitHub ticket \#213](https://github.com/ImagingDataCommons/IDC-WebApp/issues/213)
* Tag viewer version is currently broken for RT Struct series in the OHIF viewer
  * [GitHub ticket \#2122](https://github.com/OHIF/Viewers/issues/2122)
* The occasional return of a Chunk Load Error on the viewer load not handled correctly and will cause the viewer to fail
  * [GitHub ticket \#2121](https://github.com/OHIF/Viewers/issues/2121)
* The scroll bar is missing in the Tag viewer window
  * [GitHub ticket \#2129](https://github.com/OHIF/Viewers/issues/2129)
* When working with RT Structure sets, the RightSidePanel: Cannot read property 'SeriesInstanceUID' of undefined error returned in the viewer
  * [GitHub ticket \#2123](https://github.com/OHIF/Viewers/issues/2123)
* On occasion, the RTSTRUCT tab is shown when not applicable, does not load when opened
  * [GitHub ticket \#2117](https://github.com/OHIF/Viewers/issues/2117)
* The OHIF viewer doesn't load  kidney and tumor plane segmentations found in TCIA
  * [GitHub ticket \#1345](https://github.com/OHIF/Viewers/issues/1345)
* Zoom does not Update Crosshairs in MPR Mode
  * [GitHub ticket \#1419](https://github.com/OHIF/Viewers/issues/1419)
* Unable to display the same scan in two viewports and displaying different RTSTRUCTs, SEGs, and cornerstone planar measurements in each
  * [GitHub ticket \#1522](https://github.com/OHIF/Viewers/issues/1522)
* SVG Crosshair issues in VTK
  * [GitHub ticket \#1696](https://github.com/OHIF/Viewers/issues/1696)

More detailed information can be found in GitHub within the OHIF/Viewers repository. All tickets related to the Imaging Data Commons are labeled with either an [IDC:candidate](https://github.com/OHIF/Viewers/labels/IDC%3Acandidate) or an [IDC:priority](https://github.com/OHIF/Viewers/labels/IDC%3Apriority) label.

### Black screen on OHIF load fix

At this time we are rapidly updating the OHIF viewer version to fit the communities needs at a very rapid pace. This causes the occasion black screen to load on the OHIF viewer if you have used the viewer on an older version in a previous analysis. 

Below is a screenshot of what you could possibly run into and the error in the developers console.

![](../.gitbook/assets/96320286-dbc51680-0fc6-11eb-81ba-290e940f2f3f.png)

We are actively working to resolve this situation. In the meantime we ask as a workaround if you experience this issue please go back to the data portal and open the study and/or series you would like to view again. 

{% hint style="success" %}
This will be load the most recent version of the OHIF viewer and display the image you wish to view!
{% endhint %}

To track this issue in GitHub please go to [ticket \#2121](https://github.com/OHIF/Viewers/issues/2121). 

