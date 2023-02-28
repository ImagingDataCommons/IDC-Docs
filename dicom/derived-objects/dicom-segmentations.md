# DICOM Segmentations

DICOM Segmentation object (SEG) can be identified by `SOPClassUID`= `1.2.840.10008.5.1.4.1.1.66.4` Unlike most "original" image objects that you will find in IDC, SEG belongs to the family of _enhanced multiframe_ image objects, which means that it stores all of the frames (slices) in a single object. SEG can contain multiple _segments_, segment being a separate label/entity being segmented, with each segment containing one or more frames (slices). All of the frames for all of the segments are stored in the `PixelData` attribute of the object.

If you use the IDC Portal, you can select cases that include SEG objects by selecting "Segmentations" in the "Original" tab, "Modality" section ([filter link](https://portal.imaging.datacommons.cancer.gov/explore/filters/?Modality\_op=OR\&Modality=SEG)). Here is [a sample study](https://viewer.imaging.datacommons.cancer.gov/viewer/1.3.6.1.4.1.14519.5.2.1.7311.5101.170561193612723093192571245493?seriesInstanceUID=1.3.6.1.4.1.14519.5.2.1.7311.5101.206828891270520544417996275680,1.2.276.0.7230010.3.1.3.1070885483.15960.1599120307.701) that contains a SEG series.

You can further explore segmentations available in IDC via the "Derived" tab of the Portal by filtering those by specific types and anatomic locations. As an example, [this filter](https://portal.imaging.datacommons.cancer.gov/explore/filters/?SegmentedPropertyTypeCodeSequence=27925004) will select cases that contain segmentations of a nodule.

```sql
# get the viewer URL for a random study that 
#  contains SEG modality
SELECT
  ANY_VALUE(CONCAT("https://viewer.imaging.datacommons.cancer.gov/viewer/", StudyInstanceUID)) as viewer_url
FROM
  `bigquery-public-data.idc_current.dicom_all`
WHERE
  StudyInstanceUID IN (
  # select a random DICOM study that includes a SEG object
  SELECT
    StudyInstanceUID
  FROM
    `bigquery-public-data.idc_current.dicom_all`
  WHERE
    SOPClassUID = "1.2.840.10008.5.1.4.1.1.66.4"
  ORDER BY
    RAND()
  LIMIT
    1)
```

### Metadata

Metadata describing the segments is contained in the `SegmentSequence` of the DICOM object, and is also available in the BigQuery table view maintained by IDC in the [`bigquery-public-data.idc_current.segmentations`](https://console.cloud.google.com/bigquery?p=bigquery-public-data\&d=idc\_current\&t=segmentations\&page=table) BigQuery table. That table contains one row per segment, and for each segment includes metadata such as algorithm type and structure segmented.

### Conversion into alternative representations

We recommend you use one of the following tools to interpret the content of the DICOM SEG and convert it into alternative representations:

* [dcmqi](https://github.com/QIICR/dcmqi): open source DCMTK-based C++ library and command line converters that aim to help with the conversion between imaging research formats and the standard DICOM representation for image analysis results
* [highdicom](https://github.com/MGHComputationalPathology/highdicom): high-level DICOM abstractions for the Python programming language
* [DCMTK](https://dicom.offis.de/dcmtk.php.en): C++ library that provides API abstractions for reading and writing SEG objects

Tools referenced above can be used to 1) extract volumetrically reconstructed mask images corresponding to the individual segments stored in DICOM SEG; 2) extract segment-specific metadata describing its content; 3) generate standard-compliant DICOM SEG objects from research formats.

{% hint style="info" %}
SEG-specific metadata attributes are available in the table views maintained by IDC. See details [here](../../data/organization-of-data/organization-of-data-v1.md).
{% endhint %}
