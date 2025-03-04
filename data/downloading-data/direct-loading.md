# Directly Loading DICOM Objects from Google Cloud in Python

The [official Python SDK for Google Cloud Storage][1]
(installable from pip and PyPI as `google-cloud-storage`) provides a
"file-like" interface allowing other Python libraries to work with blobs as if
they were "normal" files on the local filesystem. This allows some of the DICOM
packages in the Python ecosystem to work directly with the IDC data on Google
Cloud Storage without having to first download files to a local drive.

We are not currently aware of a convenient way to do this with blobs in AWS S3
buckets. Please let us know if you find one!

### Reading Files With Pydicom

[Pydicom][2]'s [dcmread][3] function can accept a "file-like" object, meaning
you can read a file straight from a blob if you know its path.
See [this page](../organization-of-data/files-and-metadata.md#storage-buckets)
for information on finding the paths of the blobs for DICOM objects in IDC.
The `dcmread` function also has some other options that allow you to control
what is read. For example you can choose to read only the metadata and not
the pixel data, or read only certain attributes.

```python
from pydicom import dcmread
from google.cloud import storage


# Create a client and bucket object representing the IDC public data bucket
client = storage.Client()
bucket = client.bucket("idc-open-data")

# This is the path (within the above bucket) to a CT image in the IDC
blob = bucket.blob(
    "f44633af-5e76-4e01-a7fe-63764fc7e8c2/e36b336b-3550-48c9-8457-c853eab14e25.dcm"
)

# Read the whole file directly from the blob
dcm = dcmread(blob.open("rb"))

# Read metadata only (no pixel data)
dcm = dcmread(blob.open("rb"), stop_before_pixels=True)

# Read only specific attributes, identified by their tag
# (here the Manufacturer and ManufacturerModelName # attributes)
dcm = dcmread(blob.open("rb"), specific_tags=[0x0008_0070, 0x0008_1090])
```

Reading only metadata or only specific attributes will *usually* reduce the
amount of data that needs to be pulled down and therefore make the loading
process faster.

This works because running the [open][4] method on a Blob object returns
a [BlobReader][5] object, which has a "file-like" interface (specifically
the ``seek``, ``read``, and ``tell`` methods). There are further parameters
of the `open()` method that may improve performance, for example the
`chunk_size`, which you may wish to explore if performance is important to
you.

### Frame Level Access With Highdicom

[Highdicom][6] is a higher-level library providing several features to work
with images and image-derived DICOM objects. As of the release 0.25.1, its
various reading methods including [imread][7], [segread][8], [annread][9],
and [srread][10] can read any file-like object, including Google Cloud blobs.

A particularly useful feature when working with blobs is ["lazy" frame retrieval][13]
for images and segmentations. This feature allows you to download the metadata,
use it to determine which frames are of interest, and request only frames of
interest as and when they are needed. This is particularly useful for
large multiframe files such as those found in slide microscopy or multi-segment
binary or fractional segmentations as it can significantly reduce the amount
of data that needs to be downloaded to access a subset of the frames.

In this first example, we use lazy frame retrieval to load only a specific
spatial patch from a large whole slide image from the IDC.

```python
import numpy as np
import highdicom as hd
import matplotlib.pyplot as plt
from google.cloud import storage


# Create a storage client and use it to access the IDC's public data package
client = storage.Client()
bucket = client.bucket("idc-open-data")

# This is the path (within the above bucket) to a whole slide image from the
# IDC collection called "CCDI MCI"
blob = bucket.blob(
    "763fe058-7d25-4ba7-9b29-fd3d6c41dc4b/210f0529-c767-4795-9acf-bad2f4877427.dcm"
)

# Read directly from the blob object using lazy frame retrieval
im = hd.imread(
    blob.open(mode="rb"),
    lazy_frame_retrieval=True
)

# Grab an arbitrary region of tile full pixel matrix
region = im.get_total_pixel_matrix(
    row_start=15000,
    row_end=15512,
    column_start=17000,
    column_end=17512,
    dtype=np.uint8
)

# Show the region
plt.imshow(region)
plt.show()
```

Running this code should produce an output that looks like this:

<p align="center">
  <img src="../../.gitbook/assets/slide_screenshot.png" alt="Screenshot of slide region" width="524" height="454">
</p>

As a further example, we use lazy frame retrieval to load only a specific set
of segments from a large multi-organ segmentation of a CT image in the IDC
stored in binary format (meaning each segment is stored using a separate set of
frames).


```python
import highdicom as hd
from google.cloud import storage


# Create a storage client and use it to access the IDC's public data package
client = storage.Client()
bucket = client.bucket("idc-open-data")

# This is the path (within the above bucket) to a segmentation of a CT series
# containing a large number of different organs
blob = bucket.blob(
    "3f38511f-fd09-4e2f-89ba-bc0845fe0005/c8ea3be0-15d7-4a04-842d-00b183f53b56.dcm"
)

# Open the blob with "segread" using the "lazy frame retrieval" option
seg = hd.seg.segread(
    blob.open(mode="rb"),
    lazy_frame_retrieval=True
)

# Find the segment number corresponding to the liver segment
selected_segment_numbers = seg.get_segment_numbers(segment_label="Liver")

# Read in the selected segments lazily
volume = seg.get_volume(
    segment_numbers=selected_segment_numbers,
    combine_segments=True,
)
```

See [this][11] page for more information on highdicom's `Image` class, and
[this][12] page for the `Segmentation` class.

### The Importance of Offset Tables

Achieving good performance for these frame-level retrievals requires the
presence of a "Basic Offset Table" or "Extended Offset Table" in the file.
These tables specify the starting positions of each frame within the file.
Without an offset table being present, libraries such as highdicom have to
parse through the pixel data to find markers that tell it where frame
boundaries are, which involves pulling down significantly more data and is
therefore very slow. This mostly eliminates the potential speed benefits of
frame-level retrieval. Unfortunately there is no simple way to know whether
a file has an offset table without downloading the pixel data and checking it.
If you find that an image takes a long time to load initially, it is
probably because highdicom is constucting the offset table.

Most IDC images do include an offset table, but some of the older pathology
slide images do not. [This page][14] contains some notes about whether
individual collections include offset table because it wasn't included in
the file.


[1]: https://cloud.google.com/python/docs/reference/storage/latest/
[2]: https://pydicom.github.io/pydicom/stable/index.html
[3]: https://pydicom.github.io/pydicom/stable/reference/generated/pydicom.filereader.dcmread.html#pydicom.filereader.dcmread
[4]: https://cloud.google.com/python/docs/reference/storage/latest/google.cloud.storage.blob.Blob#google_cloud_storage_blob_Blob_open
[5]: https://cloud.google.com/python/docs/reference/storage/latest/google.cloud.storage.fileio.BlobReader
[6]: https://highdicom.readthedocs.io
[7]: https://highdicom.readthedocs.io/en/latest/package.html#highdicom.imread
[8]: https://highdicom.readthedocs.io/en/latest/package.html#highdicom.seg.segread
[9]: https://highdicom.readthedocs.io/en/latest/package.html#highdicom.ann.annread
[10]: https://highdicom.readthedocs.io/en/latest/package.html#highdicom.sr.srread
[11]: https://highdicom.readthedocs.io/en/latest/image.html
[12]: https://highdicom.readthedocs.io/en/latest/seg.html
[13]: https://highdicom.readthedocs.io/en/latest/image.html#lazy
[14]: https://github.com/ImagingDataCommons/idc-wsi-conversion?tab=readme-ov-file#overview
