# Viewer release notes

{% hint style="warning" %}
The version of the viewer is shown in the Debug Info option.
{% endhint %}

## 4.10.1 - September 2020

**The OHIF Viewer** is a zero-footprint medical image viewer provided by the [Open Health Imaging Foundation \(OHIF\)](http://ohif.org/). It is a configurable and extensible progressive web application with out-of-the-box support for image archives which support [DICOMweb](https://www.dicomstandard.org/dicomweb/).

Main highlights of this release include:

* Handle uncaught exception for non TID 1500 sr;
* Added display of badge numbers in the segmentation / rtstruct panel tabs;
* Study prefetcher with loading bar.

## 4.9.19 - June 2020

**The OHIF Viewer** is a zero-footprint medical image viewer provided by the [Open Health Imaging Foundation \(OHIF\)](http://ohif.org/). It is a configurable and extensible progressive web application with out-of-the-box support for image archives which support [DICOMweb](https://www.dicomstandard.org/dicomweb/).

Main highlights of this release include:

* Add exponential backoff and retry after 500 error;
* Update to HTML SR viewport to display missing header tags.

## 4.9.17 - May 2020

**The OHIF Viewer** is a zero-footprint medical image viewer provided by the [Open Health Imaging Foundation \(OHIF\)](http://ohif.org/). It is a configurable and extensible progressive web application with out-of-the-box support for image archives which support [DICOMweb](https://www.dicomstandard.org/dicomweb/).

Main highlights of this release include:

* Add disable server cache feature;
* Additional improvements on series inconsistencies report UI.

## 4.9.13 - April 2020

**The OHIF Viewer** is a zero-footprint medical image viewer provided by the [Open Health Imaging Foundation \(OHIF\)](http://ohif.org/). It is a configurable and extensible progressive web application with out-of-the-box support for image archives which support [DICOMweb](https://www.dicomstandard.org/dicomweb/).

Main highlights of this release include:

* Add acquisition storage SR sopclass to SR html ext;
* Fix missing items in the segmentation combobox items at loading;
* Fix slices are not sorted in geometrical order;
* Extend series inconsistencies checks to segmentation and improve UI.

## 4.9.7 - March 2020

**The OHIF Viewer** is a zero-footprint medical image viewer provided by the [Open Health Imaging Foundation \(OHIF\)](http://ohif.org/). It is a configurable and extensible progressive web application with out-of-the-box support for image archives which support [DICOMweb](https://www.dicomstandard.org/dicomweb/).

Main highlights of this release include:

* Add new log service to be used by debugger extension;
* Add UI to communicate to the users inconsistencies within a single series;
* Add time in the dates of the items of the segmentation combobox list;
* Order segmentation combobox list in reverse time order;
* Fix failure to load a valid SEG object because of incorrect expectations about ReferencedSegmentNumber;
* Fix RSTRUCT menu visibility when loading a series;
* Fix image load slowness regression;
* Fix choppy scrolling in 2D mod;
* Fix failure to load segmentations when filtering study with '?seriesInstanceUID=' syntax.

## 4.8.10 - February 2020

**The OHIF Viewer** is a zero-footprint medical image viewer provided by the [Open Health Imaging Foundation \(OHIF\)](http://ohif.org/). It is a configurable and extensible progressive web application with out-of-the-box support for image archives which support [DICOMweb](https://www.dicomstandard.org/dicomweb/).

Main highlights of this release include:

* Replace instance dropdown to slider for dicom tag browser;
* Add error page and not found pages if failed to retrieve study data.

## 4.8.5 - Jannuary 2020

**The OHIF Viewer** is a zero-footprint medical image viewer provided by the [Open Health Imaging Foundation \(OHIF\)](http://ohif.org/). It is a configurable and extensible progressive web application with out-of-the-box support for image archives which support [DICOMweb](https://www.dicomstandard.org/dicomweb/).

Main highlights of this release include:

* Add UI error report for MPR buffer limit related errors;
* Add UI error report for hardware acceleration turned off errors;
* Add IDC funding acknowledgment;
* Fix RSTRUCT menu panel undefined variables;
* Fix RSTRUCT menu visibility when loading a series;
* Fix segments visibility control \(SEG menu\) bugs .

## 4.8.0 - December 2020

**The OHIF Viewer** is a zero-footprint medical image viewer provided by the [Open Health Imaging Foundation \(OHIF\)](http://ohif.org/). It is a configurable and extensible progressive web application with out-of-the-box support for image archives which support [DICOMweb](https://www.dicomstandard.org/dicomweb/).

Main highlights of this release include:

* Visualize overlapping segments;
* Use runtime value configuration to get pkg version;
* Fix navigation issues in the crosshair tool.

## 4.5.22 - October 2020

**The OHIF Viewer** is a zero-footprint medical image viewer provided by the [Open Health Imaging Foundation \(OHIF\)](http://ohif.org/). It is a configurable and extensible progressive web application with out-of-the-box support for image archives which support [DICOMweb](https://www.dicomstandard.org/dicomweb/).

Main highlights of this release include:

* Add MPR crosshair tool.

