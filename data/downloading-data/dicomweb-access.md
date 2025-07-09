# Accessing data through DICOMweb

IDC users can now also use DICOMweb for intelligent access to IDC data. This is especially useful for efficiently downloading small(er) parts of GB-sized digital pathology images. For example, while pathology whole-slide images (WSIs) are mostly huge, the actual part of an image that shows tissue (of interest in an analysis) can be comparably small.   
As presented [here](https://learn.canceridc.dev/data/organization-of-data/dicom-stores), there are two DICOM stores \- the [IDC-maintained DICOM store](https://learn.canceridc.dev/data/organization-of-data/dicom-stores#idc-maintained-dicom-store-via-proxy) and the [Google-maintained DICOM store](https://learn.canceridc.dev/data/organization-of-data/dicom-stores#dicom-store-maintained-by-google-healthcare) \- available and we recommend that you decide on one of the two options depending on your use case first.

{% hint style="success" %}
Code snippets included in this article are also replicated in [this Google Colab tutorial notebook](https://github.com/ImagingDataCommons/IDC-Tutorials/blob/master/notebooks/advanced_topics/idc_dcmweb_access.ipynb) for your convenience.
{% endhint %}

## Reading slide regions via DICOMweb

There are (at least) the following two Python libraries available that facilitate access to a DICOM store via dicomweb: 

- [wsidicom](https://github.com/imi-bigpicture/wsidicom)   
- [ez-wsi-dicomweb](https://github.com/GoogleCloudPlatform/EZ-WSI-DICOMweb)

Both libraries can be installed using pip:   
```python
pip install wsidicom  
pip install ez-wsi-dicomweb
```

wsidicom is based upon the [dicomweb_client](https://github.com/ImagingDataCommons/dicomweb-client) Python library, while ez-wsi-dicomweb includes its own DICOMweb implementation. 

{% hint style="danger" %}
Note that you can use wsidicom with both, the IDC-maintained and the Google-maintained DICOM store, while ez-wsi-dicomweb only works with the Google-maintained store. 
{% endhint %}

The following code snippets show exemplarily how to use each of the libraries to access a subregion from a DICOM slide.  

For this, let’s say we are interested in the WSI with 

- DICOM StudyInstanceUID = 2.25.317753984268209060558780446660059290395
- DICOM SeriesInstanceUID = 1.3.6.1.4.1.5962.99.1.1042652702.25371455.1637425225246.2.0

### wsidicom

When you work with wsidicom, the first step requires setting up [dicomweb_client](https://github.com/ImagingDataCommons/dicomweb-client)’s `DICOMwebClient`: 

```python
from dicomweb_client.api import DICOMwebClient  
from dicomweb_client.ext.gcp.session_utils import create_session_from_gcp_credentials
```

If you are accessing the Google-maintained DICOM store, you need to authenticate with your Google credentials first and set up an authorized session for the DICOMwebClient. 

```python
from google.colab import auth  
auth.authenticate_user()

# Create authorized session  
session = create_session_from_gcp_credentials()

# Set-up a DICOMwebClient using the dicomweb_client library  
google_dicom_store_url = 'https://healthcare.googleapis.com/v1/projects/nci-idc-data/locations/us-central1/datasets/idc/dicomStores/idc-store-v20/dicomWeb'  
dw_client = DICOMwebClient(  
    url=dicom_store_url,  
    session=session  
)
```

Otherwise, you can skip ahead and just set up your DICOMwebClient using the proxy URL.   

```python
# Set-up a DICOMwebClient using the dicomweb_client library  
idc_dicom_store_url = 'https://proxy.imaging.datacommons.cancer.gov/current/viewer-only-no-downloads-see-tinyurl-dot-com-slash-3j3d9jyp/dicomWeb'

dw_client = DICOMwebClient(url=idc_dicom_store_url)
```

#### Slide access with wsidicom

You now need to wrap the previously set-up `DICOMwebClient` into wsidicom’s `WsiDicomWebClient`. Then you can use the `open_web()` functionality to find, open and investigate your slide of interest:  

```python
import wsidicom  
import matplotlib.pyplot as plt

wsidicom_client = wsidicom.WsiDicomWebClient(dw_client)  
slide = wsidicom.WsiDicom.open_web(wsidicom_client,  
    study_uid='2.25.317753984268209060558780446660059290395',  
    series_uids='1.3.6.1.4.1.5962.99.1.1042652702.25371455.1637425225246.2.0'  
)  
print(slide)
```

`[0]: Pyramid of levels:`  
    `[0]: Level: 0, size: Size(width=46336, height=44288) px, mpp: SizeMm(width=0.2325, height=0.2325) um/px Instances:         [0]: default z: 0.0 default path: 1 ImageData <wsidicom.web.wsidicom_web_image_data.WsiDicomWebImageData object at 0x7b612914ded0>`  
    `[1]: Level: 2, size: Size(width=11584, height=11072) px, mpp: SizeMm(width=0.93, height=0.93) um/px Instances:         [0]: default z: 0.0 default path: 1 ImageData <wsidicom.web.wsidicom_web_image_data.WsiDicomWebImageData object at 0x7b612a282d50>`  
    `[2]: Level: 4, size: Size(width=2896, height=2768) px, mpp: SizeMm(width=3.72, height=3.72) um/px Instances:         [0]: default z: 0.0 default path: 1 ImageData <wsidicom.web.wsidicom_web_image_data.WsiDicomWebImageData object at 0x7b612a29af50>`

To access a certain part of a slide, wsidicom offers the `read_region()` functionality: 

```python
# Access and visualize 500x500px subregion at level 4, starting from pixel (1000,1000)  
region = slide.read_region(location=(1000, 1000), level=4, size=(500, 500))  
plt.imshow(region)  
plt.show()
```

<div align="center"><img src="../../.gitbook/assets/slide_screenshot_dcmweb.png" alt="Screenshot of slide region" height="454" width="524"></div>

### ez-wsi-dicomweb

The following code shows how to set-up an interface for DICOMweb with ez-wsi-dicomweb. You can only use this interface for accessing data from the Google-maintained DICOM store which means, authentication with you Google account is required. 

```python
from ez_wsi_dicomweb import dicomweb_credential_factory  
from ez_wsi_dicomweb import dicom_slide  
from ez_wsi_dicomweb import local_dicom_slide_cache_types  
from ez_wsi_dicomweb import dicom_web_interface  
from ez_wsi_dicomweb import patch_generator  
from ez_wsi_dicomweb import pixel_spacing  
from ez_wsi_dicomweb.ml_toolkit import dicom_path

from google.colab import auth  
auth.authenticate_user()

google_dicom_store_url = 'https://healthcare.googleapis.com/v1/projects/nci-idc-data/locations/us-central1/datasets/idc/dicomStores/idc-store-v20/dicomWeb'  
study_uid = '2.25.317753984268209060558780446660059290395'  
series_uid = '1.3.6.1.4.1.5962.99.1.1042652702.25371455.1637425225246.2.0'

series_path_str = (  
      f'{google_dicom_store_url}'  
      f'/studies/{study_uid}'  
      f'/series/{series_uid}'  
)  
series_path = dicom_path.FromString(series_path_str)  
dcf = dicomweb_credential_factory.CredentialFactory()  
dwi = dicom_web_interface.DicomWebInterface(dcf)
```

The slide, slide level information and slide regions can be accessed as follows. To accelerate image retrieval, ez-wsi-dicomweb can be configured to fetch frames in blocks and cache them for subsequent use. For more information, check out [this notebook](https://github.com/GoogleCloudPlatform/EZ-WSI-DICOMweb/blob/main/ez_wsi_demo.ipynb), section “Enabling EZ-WSI DICOMweb Frame Cache”.   

```python
ds = dicom_slide.DicomSlide(  
    dwi=dwi,  
    path=series_path,  
    enable_client_slide_frame_decompression = True  
)

# More information: https://github.com/GoogleCloudPlatform/EZ-WSI-DICOMweb/blob/main/ez_wsi_demo.ipynb
ds.init_slide_frame_cache(  optimization_hint=local_dicom_slide_cache_types.CacheConfigOptimizationHint.MINIMIZE_LATENCY  
)
```
```python
# Investigate existing levels and their dimensions  
for level in ds.levels:  
    print(f'Level {level.level_index} has pixel dimensions (row, col): {level.height, level.width}')
```

`Level 1 has pixel dimensions (row, col): (44288, 46336)`  
`Level 2 has pixel dimensions (row, col): (11072, 11584)`       
`Level 3 has pixel dimensions (row, col): (2768, 2896)`

```python
# Access and visualize 500x500px subregion at level 3, starting from pixel (1000,1000)  
level = ds.get_level_by_index(3)  
region = ds.get_patch(level=level, x=1000, y=1000, width=500, height=500).image_bytes()  
plt.imshow(region)  
plt.show()
```

<div align="center"><img src="../../.gitbook/assets/slide_screenshot_dcmweb.png" alt="Screenshot of slide region" height="454" width="524"></div>

## Iterating through tiles using DICOMweb

To iterate over and access image tiles you can simply wrap the functionality presented above into your own function that iterates over the coordinates of interest to you. In case you prefer to iterate over the frames as they are stored within the DICOM file, wsidicom does also offer a [`read_tile()`](https://github.com/imi-bigpicture/wsidicom/blob/8372612cbbcce972c70bfc0fd2922655ec886c5c/wsidicom/wsidicom.py#L716) method.  
Iteration over a slide and accessing tiles from an area defined by a tissue mask can be quite easily achieved using ez-wsi-dicomweb’s DICOMPatchGenerator as described in [this notebook](https://github.com/GoogleCloudPlatform/EZ-WSI-DICOMweb/blob/main/ez_wsi_demo.ipynb) in section “Generating patches from a level image”. 

## Recommendations

Both libraries —ez-wsi-dicomweb and wsidicom—can be recommended for reliable DICOMweb access to IDC data. Based on our experience, ez-wsi-dicomweb typically delivers faster performance likely due to its caching capabilities and is specifically designed to efficiently access image patches from a Google DICOM store for AI model training. Wsidicom, on the other hand, is more general-purpose offering extensive functionality for accessing DICOM files (images as well as annotation files) both from local disk or from the cloud via DICOMweb. It is important to note that when running code locally, access times may be slightly longer compared to cloud-based (such as in a Colab notebook) execution.