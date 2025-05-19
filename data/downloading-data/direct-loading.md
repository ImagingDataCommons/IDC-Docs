# Directly loading DICOM objects from Google Cloud or AWS in Python

DICOM files in the IDC are stored as "blobs" on the cloud, with one copy housed on Google Cloud Storage (GCS) and another on Amazon Web Services (AWS) S3 storage. By using the right tools, these blobs can be wrapped to appear as "file-like" objects to Python DICOM libraries, enabling intelligent loading of DICOM files directly from cloud storage as if they were local files without having to first download them onto a local drive.
### Reading files with Pydicom

[Pydicom][2] is popular library for working with DICOM files in Python. Its [dcmread][3] function is able to accept any "file-like" object, meaning you can read a file straight from a cloud blob if you know its path. See [this page](../organization-of-data/files-and-metadata.md#storage-buckets) for information on finding the paths of the blobs for DICOM objects in IDC. The `dcmread` function also has some other options that allow you to control what is read. For example you can choose to read only the metadata and not the pixel data, or read only certain attributes. In the following two sections, we demonstrate these abilities using first Google Cloud Storage blobs and then AWS S3 blobs.

##### Mapping IDC DICOM series to bucket URLs

All of the image data available from IDC is replicated between public Google Cloud Storage (GCS) and AWS buckets. pip-installable [idc-index](https://github.com/imagingdatacommons/idc-index) package provides convenience functions to get URLs of the files corresponding to a given DICOM series.

```python
from idc_index import IDCClient


# Create IDCClient for looking up bucket URLs
idc_client = IDCClient()

# Get the list of GCS file URLs in Google bucket from SeriesInstanceUID
gcs_file_urls = idc_client.get_series_file_URLs(
    seriesInstanceUID="1.3.6.1.4.1.14519.5.2.1.131619305319442714547556255525285829796",
    source_bucket_location="gcs",
)

# Get the list of AWS file URLs in AWS bucket from SeriesInstanceUID
aws_file_urls = idc_client.get_series_file_URLs(
    seriesInstanceUID="1.3.6.1.4.1.14519.5.2.1.131619305319442714547556255525285829796",
    source_bucket_location="aws",
)
```

##### From Google Cloud Storage blobs

The [official Python SDK for Google Cloud Storage][1] (installable from pip and PyPI as `google-cloud-storage`) provides a "file-like" interface allowing other Python libraries, such as Pydicom, to work with blobs as if they were "normal" files on the local filesystem.

To read from a GCS blob with Pydicom, first create a storage client and blob object, representing a remote blob object stored on the cloud, then simply use the `.open('rb')` method to create a readable file-like object that can be passed to the `dcmread` function.

```python
from pydicom import dcmread
from pydicom.datadict import keyword_dict
from google.cloud import storage
from idc_index import IDCClient


# Create IDCClient for looking up bucket URLs
idc_client = IDCClient()

# Create a client and bucket object representing the IDC public data bucket
gcs_client = storage.Client.create_anonymous_client()

# This example uses a CT series in the IDC.
# get the list of file URLs in Google bucket from the SeriesInstanceUID
file_urls = idc_client.get_series_file_URLs(
    seriesInstanceUID="1.3.6.1.4.1.14519.5.2.1.131619305319442714547556255525285829796",
    source_bucket_location="gcs",
)

# URLs will look like this:
# s3://idc-open-data/668029cf-41bf-4644-b68a-46b8fa99c3bc/f4fe9671-0a99-4b6d-9641-d441f13620d4.dcm
(_, _, bucket_name, folder_name, file_name) = file_urls[0].split("/")
blob_key = f"{folder_name}/{file_name}"

# These objects represent the bucket and a single image blob within the bucket
bucket = gcs_client.bucket(bucket_name)
blob = bucket.blob(blob_key)

# Read the whole file directly from the blob
with blob.open("rb") as reader:
    dcm = dcmread(reader)

# Read metadata only (no pixel data)
with blob.open("rb") as reader:
    dcm = dcmread(reader, stop_before_pixels=True)

# Read only specific attributes, identified by their tag
# (here the Manufacturer and ManufacturerModelName attributes)
with blob.open("rb") as reader:
    dcm = dcmread(
        reader,
        specific_tags=[keyword_dict['Manufacturer'], keyword_dict['ManufacturerModelName']],
    )
    print(dcm)
```

Reading only metadata or only specific attributes will reduce the amount of data that needs to be pulled down under some circumstances and therefore make the loading process faster. This depends on the size of the attributes being retrieved, the `chunk_size` (a parameter of the `open()` method that controls how much data is pulled in each HTTP request to the server), and the position of the requested element within the file (since it is necessary to seek through the file until the requested attributes are found, but any data after the requested attributes need not be pulled).

This works because running the [open][4] method on a Blob object returns a [BlobReader][5] object, which has a "file-like" interface (specifically the ``seek``, ``read``, and ``tell`` methods).

##### From AWS S3 blobs

The `boto3` package provides a Python API for accessing S3 blobs. It can be installed with `pip install boto3`. In order to access open IDC data without providing AWS credentials, it is necessary to configure your own client object such that it does not require signing. This is demonstrated in the following example, which repeats the above example using the counterpart of the same blob on AWS S3. If you want to read an entire file, we recommend using a temporary buffer like this:

```python
from io import BytesIO
from pydicom import dcmread

import boto3
from botocore import UNSIGNED
from botocore.config import Config
from idc_index import IDCClient


# Create IDCClient for looking up bucket URLs
idc_client = IDCClient()

# This example uses a CT series in the IDC (same as above).
# Get the list of file URLs in AWS bucket from SeriesInstanceUID
file_urls = idc_client.get_series_file_URLs(
    seriesInstanceUID="1.3.6.1.4.1.14519.5.2.1.131619305319442714547556255525285829796",
    source_bucket_location="aws",
)

# URLs will look like this:
# s3://idc-open-data/668029cf-41bf-4644-b68a-46b8fa99c3bc/f4fe9671-0a99-4b6d-9641-d441f13620d4.dcm
(_, _, bucket_name, folder_name, file_name) = file_urls[0].split("/")
blob_key = f"{folder_name}/{file_name}"

# Configure a client to avoid the need for AWS credentials
s3_client = boto3.client('s3', config=Config(signature_version=UNSIGNED))

with BytesIO() as buf:
    # Download entire file contents to an in-memory buffer
    s3_client.download_fileobj("idc-open-data", blob_key, buf)

    # Use pydicom to read from the in-memory buffer
    buf.seek(0)
    dcm = dcmread(buf)
```

Unlike `google-cloud-storage`, `boto3` does not provide a file-like interface to access data in blobs. Instead, the `smart_open` [package][15] is a third-party package that wraps an S3 client to expose a "file-like" interface. It can be installed with `pip install 'smart_open[s3]'`. However, we have found that the buffering behavior of this package (which is intended for streaming) is not well matched to the use case of reading DICOM metadata, resulting in many unnecassary requests while reading the metadata of DICOM files (see [this](https://github.com/piskvorky/smart_open/issues/712) issue). Therefore while the following will work, we recommend using the approach in the above example (downloading the whole file) in most cases even if you only want to read the metadata as it will likely be much faster. The exception to this is when reading only the metadata of very large images where the total amount of pixel data dwarfs the amount of metadata (or using frame-level access to such images, see below).

```python
from pydicom import dcmread

import boto3
from botocore import UNSIGNED
from botocore.config import Config
import smart_open

from idc_index import IDCClient

# Create IDCClient for looking up bucket URLs
idc_client = IDCClient()

# Get the list of file URLs in AWS bucket from SeriesInstanceUID
file_urls = idc_client.get_series_file_URLs(
    seriesInstanceUID="1.3.6.1.4.1.14519.5.2.1.131619305319442714547556255525285829796",
    source_bucket_location="aws"
)

# URL to an IDC CT image on AWS S3
url = file_urls[0]

# Configure a client to avoid the need for AWS credentials
s3_client = boto3.client('s3', config=Config(signature_version=UNSIGNED))

# Read the whole file directly from the blob
with smart_open.open(url, mode="rb", transport_params=dict(client=s3_client)) as reader:
    dcm = dcmread(reader)

# Read metadata only (no pixel data)
with smart_open.open(url, mode="rb", transport_params=dict(client=s3_client)) as reader:
    dcm = dcmread(reader, stop_before_pixels=True)
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
from pydicom import dcmread
from pydicom.datadict import keyword_dict

from idc_index import IDCClient

# Create IDCClient for looking up bucket URLs
idc_client = IDCClient()

# install additional component of idc-index to resolve SM instances to file URLs
idc_client.fetch_index("sm_instance_index")

# given SeriesInstanceUID of an SM series, find the instance that corresponds to the
# highest resolution base layer of the image pyramid
query = """
SELECT SOPInstanceUID, TotalPixelMatrixColumns
FROM sm_instance_index
WHERE SeriesInstanceUID = '1.3.6.1.4.1.5962.99.1.1900325859.924065538.1719887277027.4.0'
ORDER BY TotalPixelMatrixColumns DESC
LIMIT 1
"""
result = idc_client.sql_query(query)

# get URL corresponding to the base layer instance in the Google Storage bucket
base_layer_file_url = idc_client.get_instance_file_URL(sopInstanceUID=result.iloc[0]["SOPInstanceUID"], source_bucket_location="gcs")

# Create a storage client and use it to access the IDC's public data package
gcs_client = storage.Client.create_anonymous_client()

(_,_, bucket_name, folder_name, file_name) = base_layer_file_url.split("/")
blob_key = f"{folder_name}/{file_name}"

bucket = gcs_client.bucket(bucket_name)
base_layer_blob = bucket.blob(blob_key)

# Read directly from the blob object using lazy frame retrieval
with base_layer_blob.open(mode="rb") as reader:
    im = hd.imread(reader, lazy_frame_retrieval=True)

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
from idc_index import IDCClient


# Create IDCClient for looking up bucket URLs
idc_client = IDCClient()

# Get the file URL corresponding to the segmentation of a CT series
# containing a large number of different organs - the same one as used in the
# IDC Portal front page
file_urls = idc_client.get_series_file_URLs(
    seriesInstanceUID="1.2.276.0.7230010.3.1.3.313263360.15787.1706310178.804490",
    source_bucket_location="gcs"
)

(_, _, bucket_name, folder_name, file_name) = file_urls[0].split("/")

# Create a storage client and use it to access the IDC's public data package
gcs_client = storage.Client.create_anonymous_client()
bucket = gcs_client.bucket(bucket_name)

blob_name = f"{folder_name}/{file_name}"
blob = bucket.blob(blob_name)

# Open the blob with "segread" using the "lazy frame retrieval" option
with blob.open(mode="rb") as reader:
    seg = hd.seg.segread(reader, lazy_frame_retrieval=True)

    # Find the segment number corresponding to the liver segment
    selected_segment_numbers = seg.get_segment_numbers(segment_label="Liver")

    # Read in the selected segments lazily
    volume = seg.get_volume(
        segment_numbers=selected_segment_numbers,
        combine_segments=True,
    )

# Print dimensions of the liver segment volume
print(volume.shape)
```

See [this][11] page for more information on highdicom's `Image` class, and [this][12] page for the `Segmentation` class.

### The importance of offset tables for slide microscopy (SM) images

Achieving good performance for the Slide Microscopy frame-level retrievals requires the presence of either a "Basic Offset Table" or "Extended Offset Table" in the file. These tables specify the starting positions of each frame within the file's byte stream. Without an offset table being present, libraries such as highdicom have to parse through the pixel data to find markers that tell it where frame boundaries are, which involves pulling down significantly more data and is therefore very slow. This mostly eliminates the potential speed benefits of frame-level retrieval. Unfortunately there is no simple way to know whether a file has an offset table without downloading the pixel data and checking it. If you find that an image takes a long time to load initially, it is probably because highdicom is constucting the offset table itself because it wasn't included in the file.

Most IDC images do include an offset table, but some of the older pathology slide images do not. [This page][14] contains some notes about whether individual collections include offset tables.

You can also check whether an image file (including pixel data) has an offset table using pydicom like this:

```python
import pydicom


dcm = pydicom.dcmread("...")  # Any method to read from file/cloud storage


print("Has Extended Offset Table:", "ExtendedOffsetTable" in dcm)
print("Has Basic Offset Table:", dcm.Pixeldata[4:8] != b'\x00\x00\x00\x00')

```


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
