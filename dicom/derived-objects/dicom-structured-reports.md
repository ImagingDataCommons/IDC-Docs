# DICOM Structured Reports

DICOM SR uses data elements to encode a higher level abstraction that is a tree of content, where nodes of the tree and their relationships are formalized. SR-TID1500 is one of many standard templates that define constraints on the structure of the tree, and is intended for generic tasks involving image-based measurements. DICOM SR uses standard terminologies and codes to deliver structured content. These codes are used for defining both the concept names and values assigned to those concepts (name-value pairs). Measurements include coded concepts corresponding to the quantity being measured, and a numeric value accompanied by coded units. Coded categorical or qualitative values may also be present. In SR-TID1500, measurements are accompanied by additional context that helps interpret and reuse that measurement, such as finding type, location, method and derivation. Measurements computed from segmentations can reference the segmentation defining the region and the image segmented, using unique identifiers of the respective objects.

{% hint style="warning" %}
At this time, only the measurements that accompany regions of interest defined by segmentations are exposed in the IDC Portal, and in the measurements views maintained by IDC!
{% endhint %}

Open source DCMTK [dsr2html](https://support.dcmtk.org/docs/dsr2html.html) tool can be used to render the content of the DICOM SR tree in a human-readable form (you can see one example of such rendering [here](https://peerj.com/articles/2057/#fig-7)). Reconstructing this content using tools that operate with DICOM content at the level of individual attributes can be tedious. We recommend the tools referenced above that also provide capabilities for reading and writing SR-TID1500 content:

* [highdicom](https://github.com/MGHComputationalPathology/highdicom): high-level DICOM abstractions for the Python programming language
* [dcmqi](https://github.com/QIICR/dcmqi): open source DCMTK-based C++ library and command line converters that aim to help with the conversion between imaging research formats and the standard DICOM representation for image analysis results
* [DCMTK](https://dicom.offis.de/dcmtk.php.en): C++ library that provides API abstractions for reading and writing SR-TID1500 documents

Tools referenced above can be used to 1) extract qualitative evaluations and quantitative measurements fro the SR-TID1500 document; 2) generate standard-compliant SR-TID1500 objects.

{% hint style="info" %}
SR-TID1500-specific metadata attributes are available in the table views maintained by IDC. See details [here](../../data/organization-of-data/organization-of-data-v1.md).
{% endhint %}
