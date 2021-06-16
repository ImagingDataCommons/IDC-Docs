# Derived objects

In IDC we define "derived" DICOM objects as those that are obtained by analyzing or post-processing the "original" image objects. Examples of derived objects can be annotations of the images to define image regions, or to describe findings about those regions, or voxel-wise parametric maps calculated for the original images.

Although DICOM standard provides a variety of mechanisms that can be used to store specific types of derived objects, most of the image-derived objects currently stored in IDC fall into the following categories:

* voxel segmentations stored as DICOM Segmentation objects \(SEG\)
* segmentations defined as a set of planar regions stored as DICOM Radiotherapy Structure Set objects \(RTSS\)
* quantitative measurements and qualitative evaluations for the regions defined by DICOM Segmentations, those will be stored as a specific type of DICOM Structured Reporting \(SR\) objects that follows DICOM SR template [TID 1500 "Measurements report"](http://dicom.nema.org/medical/dicom/current/output/chtml/part16/chapter_A.html#sect_TID_1500) \(SR-TID1500\)

The type of the object is defined by the object class unique identifier stored in the `SOPClassUID` attribute of each DICOM object. In the IDC Portal we allow the user to define the search filter based on the human-readable name of the class instead of the value of that identifier.

{% hint style="info" %}
You can find detailed descriptions of these objects applied to specific datasets in TICA in the following open access publications:

* Fedorov, A., Clunie, D., Ulrich, E., Bauer, C., Wahle, A., Brown, B., Onken, M., Riesmeier, J., Pieper, S., Kikinis, R., Buatti, J. & Beichel, R. R. DICOM for quantitative imaging biomarker development: a standards based approach to sharing clinical data and structured PET/CT analysis results in head and neck cancer research. PeerJ **4,** e2057 \(2016\). [https://peerj.com/articles/2057/](https://peerj.com/articles/2057/)
* Fedorov, A., Hancock, M., Clunie, D., Brochhausen, M., Bona, J., Kirby, J., Freymann, J., Pieper, S., J W L Aerts, H., Kikinis, R. & Prior, F. DICOM re-encoding of volumetrically annotated Lung Imaging Database Consortium \(LIDC\) nodules. Med. Phys. \(2020\). [doi:10.1002/mp.14445 ](http://dx.doi.org/10.1002/mp.14445)
{% endhint %}

## Segmentation objects

DICOM Segmentation object \(SEG\) can be identified by `SOPClassUID`= `1.2.840.10008.5.1.4.1.1.66.4` Unlike most "original" image objects that you will find in IDC, SEG belongs to the family of _enhanced multiframe_ image objects, which means that it stores all of the frames \(slices\) in a single object. SEG can contain multiple _segments_, segment being a separate label/entity being segmented, with each segment containing one or more frames \(slices\). All of the frames for all of the segments are stored in the `PixelData` attribute of the object.

Metadata describing the segments is contained in the `SegmentSequence` of the object, and is also available in the BigQuery table view maintained by IDC in [`canceridc-data:idc_views.segmentations`](https://console.cloud.google.com/bigquery?project=canceridc-data&p=canceridc-data&d=idc_views&t=segmentations&page=table).

**TODO** explain the hierarchy of content organization in SEG: segmentation vs segments

Typically, it is most expedient to use one of the specialized tools to access the content of SEG. Since the image content of this multi-frame object is often encoded using one bit per pixel representation, and since it can contain multiple segments with potentially spatially overlapping frames, some of the tools that can are commonly used for processing image objects may not be able to parse DICOM SEG correctly. We recommend you use one of the following tools to interpret the content of the DICOM SEG and convert it into alternative representations:

* [dcmqi](https://github.com/QIICR/dcmqi): open source DCMTK-based C++ library and command line converters that aim to help with the conversion between imaging research formats and the standard DICOM representation for image analysis results
* [highdicom](https://github.com/MGHComputationalPathology/highdicom): high-level DICOM abstractions for the Python programming language
* [DCMTK](https://dicom.offis.de/dcmtk.php.en): C++ library that provides API abstractions for reading and writing SEG objects

Tools referenced above can be used to 1\) extract volumetrically reconstructed mask images corresponding to the individual segments stored in DICOM SEG; 2\) extract segment-specific metadata describing its content; 3\) generate standard-compliant DICOM SEG objects from research formats.

{% hint style="info" %}
SEG-specific metadata attributes are available in the table views maintained by IDC. See details [here](../data/organization-of-data-1/organization-of-data.md).
{% endhint %}

## Radiotherapy Structure Sets

DICOM SEG stores the segmented mask as a set of labeled image pixels, which are arranged in planes usually corresponding to the slices of the segmented images. DICOM Radiotherapy Structure Sets \(RTSS, or RTSTRUCT\) define segmented volumes by a set of planar contours. In order to use those segmentations in conjunction with the

## TID 1500 Structured Reports

DICOM SR uses data elements to encode a higher level abstraction that is a tree of content, where nodes of the tree and their relationships are formalized. SR-TID1500 is one of many standard templates that define constraints on the structure of the tree, and is intended for generic tasks involving image-based measurements. DICOM SR uses standard terminologies and codes to deliver structured content. These codes are used both for defining the concept names and values assigned to those concepts \(name-value pairs\). Measurements include coded concepts corresponding to the quantity being measured, and a numeric value accompanied by coded units. Coded categorical or qualitative values may also be present. In SR-TID1500, measurements are accompanied by additional context that helps interpret and reuse that measurement, such as finding type, location, method and derivation. Measurements computed from segmentations can reference the segmentation defining the region and the image segmented, using unique identifiers of the respective objects.

{% hint style="warning" %}
At this time, only the measurements that accompany regions of interest defined by segmentations are exposed in the IDC Portal, and in the measurements views maintained by IDC!
{% endhint %}

**TODO** explain the hierarchy of content organization in SR-TID1500: instance &gt; root container &gt; measurements group &gt; measurement, connect with the tables

Open source DCMTK [dsr2html](https://support.dcmtk.org/docs/dsr2html.html) tool can be used to render the content of the DICOM SR tree in a human-readable form \(you can see one example of such rendering [here](https://peerj.com/articles/2057/#fig-7)\). Reconstructing this content using tools that operate with DICOM content at the level of individual attributes can be tedious. We recommend the tools referenced above that also provide capabilities for reading and writing SR-TID1500 content:

* [dcmqi](https://github.com/QIICR/dcmqi): open source DCMTK-based C++ library and command line converters that aim to help with the conversion between imaging research formats and the standard DICOM representation for image analysis results
* [highdicom](https://github.com/MGHComputationalPathology/highdicom): high-level DICOM abstractions for the Python programming language
* [DCMTK](https://dicom.offis.de/dcmtk.php.en): C++ library that provides API abstractions for reading and writing SR-TID1500 documents

Tools referenced above can be used to 1\) extract qualitative evaluations and quantitative measurements fro the SR-TID1500 document; 2\) generate standard-compliant SR-TID1500 objects.

{% hint style="info" %}
SR-TID1500-specific metadata attributes are available in the table views maintained by IDC. See details [here](../data/organization-of-data-1/organization-of-data.md).
{% endhint %}

_**TODO: what to do with the results once you generated them**_

