# Viewer release notes

{% hint style="warning" %}
The version of the viewer is shown in the Debug Info option.
{% endhint %}

## 0.4.0 - October 2021 - Slim

**The Slim Viewer** is a lightweight server-less single-page application for interactive visualization of digital slide microscopy (SM) images and associated image annotations in standard DICOM format. The application is based on the [dicom-microscopy-viewer](https://github.com/MGHComputationalPathology/dicom-microscopy-viewer) library and can simply be placed in front of a [DICOMweb](https://www.dicomstandard.org/dicomweb/) compatible Image Management System (IMS), Picture Archiving and Communication (PACS), or Vendor Neutral Archive (VNA).

Main highlights of this release include:

**Improvements**
 * Include images with new flavor THUMBNAIL in image pyramid.

## 4.11.2 - October 2021 - OHIF

**The OHIF Viewer** is a zero-footprint medical image viewer provided by the [Open Health Imaging Foundation \(OHIF\)](http://ohif.org/). It is a configurable and extensible progressive web application with out-of-the-box support for image archives which support [DICOMweb](https://www.dicomstandard.org/dicomweb/).

Main highlights of this release include:

* Fix issues with segmentation orientations;
* Fix display of inconsistencies warning for segmentation thumbnails;
* Fix throttle thumbnail progress updates.

## 0.3.1 - September 2021 - Slim

**The Slim Viewer** is a lightweight server-less single-page application for interactive visualization of digital slide microscopy (SM) images and associated image annotations in standard DICOM format. The application is based on the [dicom-microscopy-viewer](https://github.com/MGHComputationalPathology/dicom-microscopy-viewer) library and can simply be placed in front of a [DICOMweb](https://www.dicomstandard.org/dicomweb/) compatible Image Management System (IMS), Picture Archiving and Communication (PACS), or Vendor Neutral Archive (VNA).

Main highlights of this release include:

**Bug fixes**
 * Set PUBLIC_URL in Dockerfile.

## 0.3.0 - September 2021 - Slim

**The Slim Viewer** is a lightweight server-less single-page application for interactive visualization of digital slide microscopy (SM) images and associated image annotations in standard DICOM format. The application is based on the [dicom-microscopy-viewer](https://github.com/MGHComputationalPathology/dicom-microscopy-viewer) library and can simply be placed in front of a [DICOMweb](https://www.dicomstandard.org/dicomweb/) compatible Image Management System (IMS), Picture Archiving and Communication (PACS), or Vendor Neutral Archive (VNA).

Main highlights of this release include:

**Improvements**
* Add button to display information about application and environment;
* Add ability to include logo;
* Verify content of SR documents before attempting to load annotations;
* Improve re-direction after authentication;
* Add retry logic and error handlers for DICOMweb requests;
* Improve documentation of application configuration in README;
* Add unit tests.

**Bug fixes**
* Disable zoom of overview map;
* Fix pagination of worklist;
* Prevent delay in tile rendering.

## 4.10.1 - September 2021 - OHIF

**The OHIF Viewer** is a zero-footprint medical image viewer provided by the [Open Health Imaging Foundation \(OHIF\)](http://ohif.org/). It is a configurable and extensible progressive web application with out-of-the-box support for image archives which support [DICOMweb](https://www.dicomstandard.org/dicomweb/).

Main highlights of this release include:

* Handle uncaught exception for non TID 1500 sr;
* Added display of badge numbers in the segmentation / rtstruct panel tabs;
* Study prefetcher with loading bar.

## 0.2.0 - August 2021 - Slim

**The Slim Viewer** is a lightweight server-less single-page application for interactive visualization of digital slide microscopy (SM) images and associated image annotations in standard DICOM format. The application is based on the [dicom-microscopy-viewer](https://github.com/MGHComputationalPathology/dicom-microscopy-viewer) library and can simply be placed in front of a [DICOMweb](https://www.dicomstandard.org/dicomweb/) compatible Image Management System (IMS), Picture Archiving and Communication (PACS), or Vendor Neutral Archive (VNA).

Main highlights of this release include:

**New features**
*Support for multiplexed immunofluorescence slide microscopy imaging;
*Client-side additive blending of multiple channels using WebGL;
*Client-side decoding of compressed frame items using WebAssembly based on Emscripten ports of libjpeg-turbo, openjpeg, and charls C/C++ libraries.

**Improvements**
*Continuous integration testing pipeline using circle CI;
*Deploy previews for manual regression testing.

**Major changes**
*Introduce new configuration parameter renderer.

## 4.9.20 - June 2021 - OHIF

**The OHIF Viewer** is a zero-footprint medical image viewer provided by the [Open Health Imaging Foundation \(OHIF\)](http://ohif.org/). It is a configurable and extensible progressive web application with out-of-the-box support for image archives which support [DICOMweb](https://www.dicomstandard.org/dicomweb/).

Main highlights of this release include:

* Add exponential backoff and retry after 500 error;
* Update to HTML SR viewport to display missing header tags.

## 0.1.0 - May 2021 - Slim

**The Slim Viewer** is a lightweight server-less single-page application for interactive visualization of digital slide microscopy (SM) images and associated image annotations in standard DICOM format. The application is based on the [dicom-microscopy-viewer](https://github.com/MGHComputationalPathology/dicom-microscopy-viewer) library and can simply be placed in front of a [DICOMweb](https://www.dicomstandard.org/dicomweb/) compatible Image Management System (IMS), Picture Archiving and Communication (PACS), or Vendor Neutral Archive (VNA).

Inital Release.

## 4.9.17 - May 2021 - OHIF

**The OHIF Viewer** is a zero-footprint medical image viewer provided by the [Open Health Imaging Foundation \(OHIF\)](http://ohif.org/). It is a configurable and extensible progressive web application with out-of-the-box support for image archives which support [DICOMweb](https://www.dicomstandard.org/dicomweb/).

Main highlights of this release include:

* Add disable server cache feature;
* Additional improvements on series inconsistencies report UI.

## 4.9.13 - April 2021 - OHIF

**The OHIF Viewer** is a zero-footprint medical image viewer provided by the [Open Health Imaging Foundation \(OHIF\)](http://ohif.org/). It is a configurable and extensible progressive web application with out-of-the-box support for image archives which support [DICOMweb](https://www.dicomstandard.org/dicomweb/).

Main highlights of this release include:

* Add acquisition storage SR sopclass to SR html ext;
* Fix missing items in the segmentation combobox items at loading;
* Fix slices are not sorted in geometrical order;
* Extend series inconsistencies checks to segmentation and improve UI.

## 4.9.7 - March 2021 - OHIF

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

## 4.8.10 - February 2021 - OHIF

**The OHIF Viewer** is a zero-footprint medical image viewer provided by the [Open Health Imaging Foundation \(OHIF\)](http://ohif.org/). It is a configurable and extensible progressive web application with out-of-the-box support for image archives which support [DICOMweb](https://www.dicomstandard.org/dicomweb/).

Main highlights of this release include:

* Replace instance dropdown to slider for dicom tag browser;
* Add error page and not found pages if failed to retrieve study data.

## 4.8.5 - Jannuary 2021 - OHIF

**The OHIF Viewer** is a zero-footprint medical image viewer provided by the [Open Health Imaging Foundation \(OHIF\)](http://ohif.org/). It is a configurable and extensible progressive web application with out-of-the-box support for image archives which support [DICOMweb](https://www.dicomstandard.org/dicomweb/).

Main highlights of this release include:

* Add UI error report for MPR buffer limit related errors;
* Add UI error report for hardware acceleration turned off errors;
* Add IDC funding acknowledgment;
* Fix RSTRUCT menu panel undefined variables;
* Fix RSTRUCT menu visibility when loading a series;
* Fix segments visibility control \(SEG menu\) bugs .

## 4.8.0 - December 2020 - OHIF

**The OHIF Viewer** is a zero-footprint medical image viewer provided by the [Open Health Imaging Foundation \(OHIF\)](http://ohif.org/). It is a configurable and extensible progressive web application with out-of-the-box support for image archives which support [DICOMweb](https://www.dicomstandard.org/dicomweb/).

Main highlights of this release include:

* Visualize overlapping segments;
* Use runtime value configuration to get pkg version;
* Fix navigation issues in the crosshair tool.

## 4.5.22 - October 2020 - OHIF

**The OHIF Viewer** is a zero-footprint medical image viewer provided by the [Open Health Imaging Foundation \(OHIF\)](http://ohif.org/). It is a configurable and extensible progressive web application with out-of-the-box support for image archives which support [DICOMweb](https://www.dicomstandard.org/dicomweb/).

Main highlights of this release include:

* Add MPR crosshair tool.

