# Directly loading DICOM objects from Google Cloud or AWS in Python

DICOM files in the IDC are stored as "blobs" on the cloud, with one copy housed on Google Cloud Storage (GCS) and another on Amazon Web Services (AWS) S3 storage. By using the right tools, these blobs can be wrapped to appear as "file-like" objects to Python DICOM libraries, enabling intelligent loading of DICOM files directly from cloud storage as if they were local files without having to first download them onto a local drive.
### Reading files with Pydicom

[Pydicom][2] is popular library for working with DICOM files in Python. Its [dcmread][3] function is able to accept any "file-like" object, meaning you can read a file straight from a cloud blob if you know its path. See [this page](../organization-of-data/files-and-metadata.md#storage-buckets) for information on finding the paths of the blobs for DICOM objects in IDC. The `dcmread` function also has some other options that allow you to control what is read. For example you can choose to read only the metadata and not the pixel data, or read only certain attributes. In the following two sections, we demonstrate these abilities using first Google Cloud Storage blobs and then AWS S3 blobs.

##### From Google Cloud Storage blobs

The [official Python SDK for Google Cloud Storage][1] (installable from pip and PyPI as `google-cloud-storage`) provides a "file-like" interface allowing other Python libraries, such as Pydicom, to work with blobs as if they were "normal" files on the local filesystem.

To read from a GCS blob with Pydicom, first create a storage client and blob object, representing a remote blob object stored on the cloud, then simply use the `.open('rb')` method to create a readable file-like object that can be passed to the `dcmread` function.

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
# (here the Manufacturer and ManufacturerModelName attributes)
dcm = dcmread(blob.open("rb"), specific_tags=[0x0008_0070, 0x0008_1090])
```

Reading only metadata or only specific attributes will *usually* reduce the amount of data that needs to be pulled down and therefore make the loading process faster.

This works because running the [open][4] method on a Blob object returns a [BlobReader][5] object, which has a "file-like" interface (specifically the ``seek``, ``read``, and ``tell`` methods). There are further parameters of the `open()` method that may improve performance, for example the `chunk_size`, which you may wish to explore if performance is important to you.

##### From AWS S3 blobs

The `smart_open` [package][15] wraps an S3 client to expose a "file-like" interface for accessing blobs. It can be installed with `pip install 'smart_open[s3]'`.

In order to be able to access open IDC data without providing AWS credentials, it is necessary to configure your own client object such that it does not require signing. This is demonstrated in the following example, which repeats the above example using the counterpart of the same blob on AWS S3.


```python
from pydicom import dcmread

import boto3
from botocore import UNSIGNED
from botocore.config import Config
import smart_open


# Configure a client to avoid the need for AWS credentials
s3_client = boto3.client('s3', config=Config(signature_version=UNSIGNED))

# URL to an IDC CT image on AWS S3
url = 's3://idc-open-data/f44633af-5e76-4e01-a7fe-63764fc7e8c2/e36b336b-3550-48c9-8457-c853eab14e25.dcm'

# Read the whole file directly from the blob
dcm = dcmread(
    smart_open.open(url, mode="rb", transport_params=dict(client=s3_client)),
)

# Read metadata only (no pixel data)
dcm = dcmread(
    smart_open.open(url, mode="rb", transport_params=dict(client=s3_client)),
    stop_before_pixels=True,
)

# Read only specific attributes, identified by their tag
# (here the Manufacturer and ManufacturerModelName attributes)
dcm = dcmread(
    smart_open.open(url, mode="rb", transport_params=dict(client=s3_client)),
    specific_tags=[0x0008_0070, 0x0008_1090],
)
```

You may want to look into the the other options of `smart_open`'s `open` [method][16] to improve performance (in particular the `buffering` parameter).

In the remainder of the examples, we will use only the GCS access method for brevity. However, you should be able to straightforwardly swap out the opened GCS blob for the opened AWS S3 blob to achieve the same effect with Amazon S3.

### Frame-level access with Highdicom

[Highdicom][6] is a higher-level library providing several features to work with images and image-derived DICOM objects. As of the release 0.25.1, its various reading methods (including [imread][7], [segread][8], [annread][9], and [srread][10]) can read any file-like object, including Google Cloud blobs and anything opened with `smart_open` (including S3 blobs).

A particularly useful feature when working with blobs is ["lazy" frame retrieval][13] for images and segmentations. This downloads only the image metadata when the file is initially loaded, uses it to create a frame-level index, and downloads specific frames as and when they are requested by the user. This is especially useful for large multiframe files (such as those found in slide microscopy or multi-segment binary or fractional segmentations) as it can significantly reduce the amount of data that needs to be downloaded to access a subset of the frames.

In this first example, we use lazy frame retrieval to load only a specific spatial patch from a large whole slide image from the IDC.

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

As a further example, we use lazy frame retrieval to load only a specific set of segments from a large multi-organ segmentation of a CT image in the IDC stored in binary format (in binary segmentations, each segment is stored using a separate set of frames).


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

See [this][11] page for more information on highdicom's `Image` class, and [this][12] page for the `Segmentation` class.

### The importance of offset tables

Achieving good performance for these frame-level retrievals requires the presence of a "Basic Offset Table" or "Extended Offset Table" in the file. These tables specify the starting positions of each frame within the file's byte stream. Without an offset table being present, libraries such as highdicom have to parse through the pixel data to find markers that tell it where frame boundaries are, which involves pulling down significantly more data and is therefore very slow. This mostly eliminates the potential speed benefits of frame-level retrieval. Unfortunately there is no simple way to know whether a file has an offset table without downloading the pixel data and checking it. If you find that an image takes a long time to load initially, it is probably because highdicom is constucting the offset table itself because it wasn't included in the file.

Most IDC images do include an offset table, but some of the older pathology slide images do not. [This page][14] contains some notes about whether individual collections include offset tables.


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
[15]: https://github.com/piskvorky/smart_open
[16]: https://github.com/piskvorky/smart_open/blob/master/help.txt
