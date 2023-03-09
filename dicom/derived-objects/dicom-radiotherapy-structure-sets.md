# DICOM Radiotherapy Structure Sets

DICOM Radiotherapy Structure Sets (RTSS, or RTSTRUCT) define regions of interest by a set of planar contours.&#x20;

RTSS objects can be identified by the `RTSTRUCT` value assigned to the `Modality` attribute, or by `SOPClassUID` = `1.2.840.10008.5.1.4.1.1.481.3`.

If you use the IDC Portal, you can select cases that include RTSTRUCT objects by selecting "Radiotherapy Structure Set" in the "Original" tab, "Modality" section ([filter link](https://portal.imaging.datacommons.cancer.gov/explore/filters/?Modality\_op=OR\&Modality=RTSTRUCT)). Here is [a sample study](https://viewer.imaging.datacommons.cancer.gov/viewer/1.3.6.1.4.1.14519.5.2.1.77129820837198295687264213920427872580) that contains an RTSS series.

As always, you get most of the power in exploring IDC metadata when using SQL interface. As an example, the query below will select a random study that contains a RTSTRUCT series, and return a URL to open that study in the viewer:

```sql
# get the viewer URL for a random study that 
#  contains RTSTRUCT modality
SELECT
  ANY_VALUE(CONCAT("https://viewer.imaging.datacommons.cancer.gov/viewer/", StudyInstanceUID)) as viewer_url
FROM
  `bigquery-public-data.idc_current.dicom_all`
WHERE
  StudyInstanceUID IN (
  # select a random DICOM study that includes an RTSTRUCT object
  SELECT
    StudyInstanceUID
  FROM
    `bigquery-public-data.idc_current.dicom_all`
  WHERE
    SOPClassUID = "1.2.840.10008.5.1.4.1.1.481.3"
  ORDER BY
    RAND()
  LIMIT
    1)
```

### Metadata

RTSTRUCT relies on unstructured text in describing the semantics of the individual regions segmented. This information is stored in the `StructureSetROISequence.ROIName` attribute. The following query will return the list of all distinct values of `ROIName` and their frequency.

```sql
SELECT
  structureSetROISequence.ROIName AS ROIName,
  COUNT(DISTINCT(SeriesInstanceUID)) AS ROISeriesCount
FROM
  `bigquery-public-data.idc_current.dicom_all`
CROSS JOIN
  UNNEST (StructureSetROISequence) AS structureSetROISequence
WHERE
  SOPClassUID = "1.2.840.10008.5.1.4.1.1.481.3"
GROUP BY
  ROIName
ORDER BY
  ROISeriesCount DESC
```

### Conversion into alternative representations

We recommend [Plastimatch convert](https://plastimatch.org/plastimatch.html#plastimatch-convert) tool for converting planar contours of the individual structure sets into volumetric representation.
