# Viewer release notes

{% hint style="warning" %}
The version of the viewer is shown in the Debug Info option.
{% endhint %}

## 4.12.33 - August 2022 - OHIF

**The OHIF Viewer** is a zero-footprint medical image viewer provided by the [Open Health Imaging Foundation (OHIF)](http://ohif.org). It is a configurable and extensible progressive web application with out-of-the-box support for image archives which support [DICOMweb](https://www.dicomstandard.org/dicomweb/).

Main highlights of this release include:

* Fix parsing of qualitative slice annotation;
* Disable measurements panel interactions in MPR mode;
* Fix parsing of segmentation when orientation values are close to zero.


## 0.8.1 - June 2022 - Slim

**The Slim Viewer** is a lightweight server-less single-page application for interactive visualization of digital slide microscopy (SM) images and associated image annotations in standard DICOM format. The application is based on the [dicom-microscopy-viewer](https://github.com/MGHComputationalPathology/dicom-microscopy-viewer) library and can simply be placed in front of a [DICOMweb](https://www.dicomstandard.org/using/dicomweb) compatible Image Management System (IMS), Picture Archiving and Communication (PACS), or Vendor Neutral Archive (VNA).

Main highlights of this release include:

**New features**

* Add panel for clinical trial information to case viewer;
* Sort digital slides by Container Identifier attribute.

**Enhancements**

* Reset style of optical paths to default when deactivating presentation state.

**Bug fixes**

* Fix rendering of ROI annotations by upgrading to React version 1;
* Correctly update UIDs of visible/active optical paths;
* Fix type declarations of DICOMweb search resources.

## 4.12.30 - June 2022 - OHIF

**The OHIF Viewer** is a zero-footprint medical image viewer provided by the [Open Health Imaging Foundation (OHIF)](http://ohif.org). It is a configurable and extensible progressive web application with out-of-the-box support for image archives which support [DICOMweb](https://www.dicomstandard.org/dicomweb/).

Main highlights of this release include:

* Add support for SR qualitative annotation per instance.

## 0.7.2 - June 2022 - Slim

**The Slim Viewer** is a lightweight server-less single-page application for interactive visualization of digital slide microscopy (SM) images and associated image annotations in standard DICOM format. The application is based on the [dicom-microscopy-viewer](https://github.com/MGHComputationalPathology/dicom-microscopy-viewer) library and can simply be placed in front of a [DICOMweb](https://www.dicomstandard.org/dicomweb/) compatible Image Management System (IMS), Picture Archiving and Communication (PACS), or Vendor Neutral Archive (VNA).

Main highlights of this release include:

**New features**

* Support DICOM Advanced Blending Presentation State to parametrize the display of multiplexed IF microscopy images;
* Add key bindings for annotations tools;
* Enable configuration of tile preload;
* Enable configuration of annotation geometry type per finding;
* Expose equipment metadata in user interface.

**Enhancements**

* Improve default presentation of multiplexed IF microscopy images in the absence of presentation state instances;
* Correctly configure DCM4CHEE Archive to use reverse proxy URL prefix for BulkDataURI in served metadata;
* Enlarge display settings interfaces and add input fields for opacity, VOI limits, and colors;
* Update dicom-microscopy-viewer version to use web workers for frame decoding/transformation operations;
* Add button for user logout;
* Disable optical path selection when a presentation state has been selected.

**Bug fixes**

* Fix parsing of URL path upon redirect after successful authentication/authorization;
* Fix configuration of optical path display settings when switching between presentation states;
* Fix caching of presentation states and for selection via drop-down menu.

**Security**

* Update dependencies with critical security issues.

## 0.5.1 - April 2022 - Slim

**The Slim Viewer** is a lightweight server-less single-page application for interactive visualization of digital slide microscopy (SM) images and associated image annotations in standard DICOM format. The application is based on the [dicom-microscopy-viewer](https://github.com/MGHComputationalPathology/dicom-microscopy-viewer) library and can simply be placed in front of a [DICOMweb](https://www.dicomstandard.org/dicomweb/) compatible Image Management System (IMS), Picture Archiving and Communication (PACS), or Vendor Neutral Archive (VNA).

Main highlights of this release include:

**Enhancements**

* Make overview panel collapsible and hide it entirely if lowest-resolution image is too large.

**Bug fixes**

* Fix update of optical path settings when switching between slides.

## 4.12.26 - April 2022 - OHIF

**The OHIF Viewer** is a zero-footprint medical image viewer provided by the [Open Health Imaging Foundation (OHIF)](http://ohif.org). It is a configurable and extensible progressive web application with out-of-the-box support for image archives which support [DICOMweb](https://www.dicomstandard.org/dicomweb/).

Main highlights of this release include:

* Fix regression in logic for finding segmentations referenced source image;
* Fix segmentations loading issues;
* Fix thumbnail series type for unsupported SOPClassUID;
* Fix toolbar error when getDerivedDatasets finds no referenced series are found.

## 0.5.0 - March 2022 - Slim

**The Slim Viewer** is a lightweight server-less single-page application for interactive visualization of digital slide microscopy (SM) images and associated image annotations in standard DICOM format. The application is based on the [dicom-microscopy-viewer](https://github.com/MGHComputationalPathology/dicom-microscopy-viewer) library and can simply be placed in front of a [DICOMweb](https://www.dicomstandard.org/dicomweb/) compatible Image Management System (IMS), Picture Archiving and Communication (PACS), or Vendor Neutral Archive (VNA).

Main highlights of this release include:

**New features**

* Display of analysis results stored as DICOM Segmentation, Parametric Map, or Microscopy Bulk Simple Annotations instances;
* Dynamic selection of DICOMweb server by user (can be enabled by setting AppConfig.enableServerSelection to true);
* Dark app mode for fluorescence microscopy (can be enabled by setting App.mode to "dark");
* Support display of segments stored in DICOM Segmentation instances;
* Support display of parameter mappings stored in DICOM Parametric Map instances;
* Support display of annotation groups stored in DICOM Microscopy Bulk Simple Annotations instances;
* Implement color transformations using ICC Profiles to correct color images client side in a browser-independent manner;
* Implement grayscale transformations using Palette Color Lookup Tables to pseudo-color grayscale images.

**Improvements**

* Unify handling of optical paths for color and grayscale images;
* Add loading indicator;
* Improve styling of overview map;
* Render specimen metadata in compacter form;
* Improve fetching of WASM library code;
* Improve styling of slide viewer sidebar;
* Sort slides by Series Number;
* Work around common standard compliance issues;
* Update docker-compose configuration;
* Upgrade dependencies;
* Show examples in README;
* Decode JPEG, JPEG 2000, and JPEG-LS compressed image frames client side in a browser-independent manner;
* Improve performance of transformation and rendering operations using WebGL for both grayscale as well as color images;
* Optimize display of overview images and keep overview image fixed when zooming or panning volume images;
* Optimize HTTP Accept header field for retrieval of frames to work around issues with various server implementations.

**Bug fixes**

* Ensure ROI annotations are re-rendered upon modification;
* Clean up memory and recreate viewers upon page reload;
* Fix selection of volume images;
* Fix color space conversion during decoding of JPEG 2000 compressed image frames;
* Fix unit of area measurements for ROI annotations;
* Publish events when bulkdata loading starts and ends.

## 4.12.22 - March 2022 - OHIF

**The OHIF Viewer** is a zero-footprint medical image viewer provided by the [Open Health Imaging Foundation (OHIF)](http://ohif.org). It is a configurable and extensible progressive web application with out-of-the-box support for image archives which support [DICOMweb](https://www.dicomstandard.org/dicomweb/).

Main highlights of this release include:

* Improve logic for finding segmentations referenced source image;
* Improve debug dialog: fix text overflow and adding active viewports referenced SEGs and RTSTRUCT series.

## 4.12.17 - February 2022 - OHIF

**The OHIF Viewer** is a zero-footprint medical image viewer provided by the [Open Health Imaging Foundation (OHIF)](http://ohif.org). It is a configurable and extensible progressive web application with out-of-the-box support for image archives which support [DICOMweb](https://www.dicomstandard.org/dicomweb/).

Main highlights of this release include:

* Fix fail to load SEG related to geometry assumptions;
* Fix fail to load SEG related to tolerance;
* Add initial support for SR planar annotations.

## 0.4.5 - January 2022 - Slim

**The Slim Viewer** is a lightweight server-less single-page application for interactive visualization of digital slide microscopy (SM) images and associated image annotations in standard DICOM format. The application is based on the [dicom-microscopy-viewer](https://github.com/MGHComputationalPathology/dicom-microscopy-viewer) library and can simply be placed in front of a [DICOMweb](https://www.dicomstandard.org/dicomweb/) compatible Image Management System (IMS), Picture Archiving and Communication (PACS), or Vendor Neutral Archive (VNA).

Main highlights of this release include:

**Bug fixes**

* Fix selection of VOLUME or THUMBNAIL images with different Photometric Interpretation.

## 4.12.12 - January 2022 - OHIF

**The OHIF Viewer** is a zero-footprint medical image viewer provided by the [Open Health Imaging Foundation (OHIF)](http://ohif.org). It is a configurable and extensible progressive web application with out-of-the-box support for image archives which support [DICOMweb](https://www.dicomstandard.org/dicomweb/).

Main highlights of this release include:

* Fix RTSTRUCT right panel updates;
* Fix SEG loading regression.

## 4.12.7 - December 2021 - OHIF

**The OHIF Viewer** is a zero-footprint medical image viewer provided by the [Open Health Imaging Foundation (OHIF)](http://ohif.org). It is a configurable and extensible progressive web application with out-of-the-box support for image archives which support [DICOMweb](https://www.dicomstandard.org/dicomweb/).

Main highlights of this release include:

* Fix handling of datasets with unsupported modalities;
* Fix backward fetch of images for the current active series.
* Fix tag browser slider.

## 0.4.3 - November 2021 - Slim

**The Slim Viewer** is a lightweight server-less single-page application for interactive visualization of digital slide microscopy (SM) images and associated image annotations in standard DICOM format. The application is based on the [dicom-microscopy-viewer](https://github.com/MGHComputationalPathology/dicom-microscopy-viewer) library and can simply be placed in front of a [DICOMweb](https://www.dicomstandard.org/dicomweb/) compatible Image Management System (IMS), Picture Archiving and Communication (PACS), or Vendor Neutral Archive (VNA).

Main highlights of this release include:

**Bug fixes**

* Rotate box in overview map outlining the extent of the current view together with the image.

## 4.12.5 - November 2021 - OHIF

**The OHIF Viewer** is a zero-footprint medical image viewer provided by the [Open Health Imaging Foundation (OHIF)](http://ohif.org). It is a configurable and extensible progressive web application with out-of-the-box support for image archives which support [DICOMweb](https://www.dicomstandard.org/dicomweb/).

Main highlights of this release include:

* Fix segmentation/rtstruct menu badge update when switching current displayed series;
* Add to series thumbnail link icon if they are connected to any annotation (segmentation, etc...);
* Fix problems opening series when the study includes many series;
* Fix segments visibility handler.

## 0.4.1 - October 2021 - Slim

**The Slim Viewer** is a lightweight server-less single-page application for interactive visualization of digital slide microscopy (SM) images and associated image annotations in standard DICOM format. The application is based on the [dicom-microscopy-viewer](https://github.com/MGHComputationalPathology/dicom-microscopy-viewer) library and can simply be placed in front of a [DICOMweb](https://www.dicomstandard.org/dicomweb/) compatible Image Management System (IMS), Picture Archiving and Communication (PACS), or Vendor Neutral Archive (VNA).

Main highlights of this release include:

**Improvements**

* Include images with new flavor THUMBNAIL in image pyramid;
* Properly fit overview map into HTML element and disable re-centering of overview map when user navigates main map;
* Allow drawing of ROIs that extent beyond the slide coordinate system (i.e., allow negative ROI coordinates).

**Bug fixes**

* Prevent display of annotation marker when ROI is deactivated

## 4.11.2 - October 2021 - OHIF

**The OHIF Viewer** is a zero-footprint medical image viewer provided by the [Open Health Imaging Foundation (OHIF)](http://ohif.org). It is a configurable and extensible progressive web application with out-of-the-box support for image archives which support [DICOMweb](https://www.dicomstandard.org/dicomweb/).

Main highlights of this release include:

* Fix issues with segmentation orientations;
* Fix display of inconsistencies warning for segmentation thumbnails;
* Fix throttle thumbnail progress updates.

## 0.3.1 - September 2021 - Slim

**The Slim Viewer** is a lightweight server-less single-page application for interactive visualization of digital slide microscopy (SM) images and associated image annotations in standard DICOM format. The application is based on the [dicom-microscopy-viewer](https://github.com/MGHComputationalPathology/dicom-microscopy-viewer) library and can simply be placed in front of a [DICOMweb](https://www.dicomstandard.org/dicomweb/) compatible Image Management System (IMS), Picture Archiving and Communication (PACS), or Vendor Neutral Archive (VNA).

Main highlights of this release include:

**Bug fixes**

* Set PUBLIC\_URL in Dockerfile.

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

**The OHIF Viewer** is a zero-footprint medical image viewer provided by the [Open Health Imaging Foundation (OHIF)](http://ohif.org). It is a configurable and extensible progressive web application with out-of-the-box support for image archives which support [DICOMweb](https://www.dicomstandard.org/dicomweb/).

Main highlights of this release include:

* Handle uncaught exception for non TID 1500 sr;
* Added display of badge numbers in the segmentation / rtstruct panel tabs;
* Study prefetcher with loading bar.

## 0.2.0 - August 2021 - Slim

**The Slim Viewer** is a lightweight server-less single-page application for interactive visualization of digital slide microscopy (SM) images and associated image annotations in standard DICOM format. The application is based on the [dicom-microscopy-viewer](https://github.com/MGHComputationalPathology/dicom-microscopy-viewer) library and can simply be placed in front of a [DICOMweb](https://www.dicomstandard.org/dicomweb/) compatible Image Management System (IMS), Picture Archiving and Communication (PACS), or Vendor Neutral Archive (VNA).

Main highlights of this release include:

**New features**

* Support for multiplexed immunofluorescence slide microscopy imaging;
* Client-side additive blending of multiple channels using WebGL;
* Client-side decoding of compressed frame items using WebAssembly based on Emscripten ports of libjpeg-turbo, openjpeg, and charls C/C++ libraries.

**Improvements**

* Continuous integration testing pipeline using circle CI;
* Deploy previews for manual regression testing.

**Major changes**

* Introduce new configuration parameter renderer.

## 4.9.20 - June 2021 - OHIF

**The OHIF Viewer** is a zero-footprint medical image viewer provided by the [Open Health Imaging Foundation (OHIF)](http://ohif.org). It is a configurable and extensible progressive web application with out-of-the-box support for image archives which support [DICOMweb](https://www.dicomstandard.org/dicomweb/).

Main highlights of this release include:

* Add exponential backoff and retry after 500 error;
* Update to HTML SR viewport to display missing header tags.

## 0.1.0 - May 2021 - Slim

**The Slim Viewer** is a lightweight server-less single-page application for interactive visualization of digital slide microscopy (SM) images and associated image annotations in standard DICOM format. The application is based on the [dicom-microscopy-viewer](https://github.com/MGHComputationalPathology/dicom-microscopy-viewer) library and can simply be placed in front of a [DICOMweb](https://www.dicomstandard.org/dicomweb/) compatible Image Management System (IMS), Picture Archiving and Communication (PACS), or Vendor Neutral Archive (VNA).

Inital Release.

## 4.9.17 - May 2021 - OHIF

**The OHIF Viewer** is a zero-footprint medical image viewer provided by the [Open Health Imaging Foundation (OHIF)](http://ohif.org). It is a configurable and extensible progressive web application with out-of-the-box support for image archives which support [DICOMweb](https://www.dicomstandard.org/dicomweb/).

Main highlights of this release include:

* Add disable server cache feature;
* Additional improvements on series inconsistencies report UI.

## 4.9.13 - April 2021 - OHIF

**The OHIF Viewer** is a zero-footprint medical image viewer provided by the [Open Health Imaging Foundation (OHIF)](http://ohif.org). It is a configurable and extensible progressive web application with out-of-the-box support for image archives which support [DICOMweb](https://www.dicomstandard.org/dicomweb/).

Main highlights of this release include:

* Add acquisition storage SR sopclass to SR html ext;
* Fix missing items in the segmentation combobox items at loading;
* Fix slices are not sorted in geometrical order;
* Extend series inconsistencies checks to segmentation and improve UI.

## 4.9.7 - March 2021 - OHIF

**The OHIF Viewer** is a zero-footprint medical image viewer provided by the [Open Health Imaging Foundation (OHIF)](http://ohif.org). It is a configurable and extensible progressive web application with out-of-the-box support for image archives which support [DICOMweb](https://www.dicomstandard.org/dicomweb/).

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

**The OHIF Viewer** is a zero-footprint medical image viewer provided by the [Open Health Imaging Foundation (OHIF)](http://ohif.org). It is a configurable and extensible progressive web application with out-of-the-box support for image archives which support [DICOMweb](https://www.dicomstandard.org/dicomweb/).

Main highlights of this release include:

* Replace instance dropdown to slider for dicom tag browser;
* Add error page and not found pages if failed to retrieve study data.

## 4.8.5 - Jannuary 2021 - OHIF

**The OHIF Viewer** is a zero-footprint medical image viewer provided by the [Open Health Imaging Foundation (OHIF)](http://ohif.org). It is a configurable and extensible progressive web application with out-of-the-box support for image archives which support [DICOMweb](https://www.dicomstandard.org/dicomweb/).

Main highlights of this release include:

* Add UI error report for MPR buffer limit related errors;
* Add UI error report for hardware acceleration turned off errors;
* Add IDC funding acknowledgment;
* Fix RSTRUCT menu panel undefined variables;
* Fix RSTRUCT menu visibility when loading a series;
* Fix segments visibility control (SEG menu) bugs .

## 4.8.0 - December 2020 - OHIF

**The OHIF Viewer** is a zero-footprint medical image viewer provided by the [Open Health Imaging Foundation (OHIF)](http://ohif.org). It is a configurable and extensible progressive web application with out-of-the-box support for image archives which support [DICOMweb](https://www.dicomstandard.org/dicomweb/).

Main highlights of this release include:

* Visualize overlapping segments;
* Use runtime value configuration to get pkg version;
* Fix navigation issues in the crosshair tool.

## 4.5.22 - October 2020 - OHIF

**The OHIF Viewer** is a zero-footprint medical image viewer provided by the [Open Health Imaging Foundation (OHIF)](http://ohif.org). It is a configurable and extensible progressive web application with out-of-the-box support for image archives which support [DICOMweb](https://www.dicomstandard.org/dicomweb/).

Main highlights of this release include:

* Add MPR crosshair tool.
