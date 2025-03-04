# Directly Loading DICOM from Google Cloud in Python

The [official Python SDK for Google Cloud Storage][1]
(installable from pip and PyPI as `google-cloud-storage`) provides a
"file-like" interface allowing other Python libraries to work with blobs as if
it were a "normal" file on the local filesystem. This allows some of the DICOM
packages in the Python ecosystem to work directly with the copy of the IDC data
in Google Cloud storage first download them to a local drive.

We are not currently aware of a convenient way to do this with blobs in AWS S3
buckets. Please let us know if you know of one!

### Reading Images With Pydicom

[Pydicom][2]'s [dcmread][3] function can accept a "file-like" object, meaning
you can read a file straight from a blob. The `dcmread` function also has some
other options that allow you to control what is read. For example you can choose
to read only the metadata and not the frames, or read only certain attributes.

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

# Read only specific attributes (here the Manufacturer and ManufacturerModelName
# attributes)
dcm = dcmread(blob.open("rb"), specific_tags=[0x0008_0070, 0x0008_1090])
```

Reading only metadata, or only specific attributes will *usually* reduce the
amount of data that needs to be pulled down and therefore make the loading
process faster.

This works because running the [open][4] method on a Blob object returns
a [BlobReader][5] object, which has a "file-like" interface (specifically
the ``seek``, ``read``, and ``tell`` methods). There are further parameters
of the `open()` method that may improve performance, for example the
`chunk_size`, which you may wish to explore in performance-critical situations.

See [this page](../organization-of-data/files-and-metadata.md#storage-buckets)
for information on finding the paths of the blobs for DICOM objects in IDC.

### Frame Level Access With Highdicom

[Highdicom][6] is a higher-level library providing several features to work
with images and image-derived DICOM objects. As of the release 0.25.1, its
various reading methods including [imread][7], [segread][8], [annread][9],
and [srread][10] can read any file-like object, including Google Cloud blobs.

Coupling this with :ref:`"lazy" frame retrieval <lazy>` option of `imread` and
`segread`is especially powerful, because it allows frames to be retrieved from
the blobs only as and when they are needed. This is particularly useful for
large multiframe files such as those found in slide microscopy or multi-segment
binary or fractional segmentations.

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

![Screenshot of slide region](../.gitbook/assets/slide_screenshot.png)

As a further example, we use lazy frame retrieval to load only a specific set
of segments from a large multi-organ segmentation of a CT image in the IDC
stored in binary format (meaning each segment is stored using a separate set of
frames).


```python
import highdicom as hd

# Additional libraries (install these separately)
from google.cloud import storage


# Create a storage client and use it to access the IDC's public data package
client = storage.Client()
bucket = client.bucket("idc-open-data")

# This is the path (within the above bucket) to a segmentation of a CT series
# from IDC collection called "CCDI MCI", containing a large number of
# different organs
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
