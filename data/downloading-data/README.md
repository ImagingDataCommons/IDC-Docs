# Downloading data

{% hint style="info" %}
If you have questions or feedback about the download tools provided by IDC, please reach out via our [forum](https://discourse.canceridc.dev/) - we are very interested in hearing your feedback and suggestions!
{% endhint %}

Depending on whether you would like to download data interactively or programmatically, we provide two recommended tools to help you.

### Interactive download: 3D Slicer SlicerIDCBrowser extension

[3D Slicer](https://www.slicer.org/) is a free open source, cross-platform, extensible desktop application developed to support a variety of medical imaging research use cases.

IDC maintains [SlicerIDCBrowser](https://github.com/ImagingDataCommons/SlicerIDCBrowser), an extension of 3D Slicer, developed to support direct access to IDC data from your desktop. You will need to [install](https://download.slicer.org/) a recent 3D Slicer 5.7.0 preview application (installers are available for Windows, Mac and Linux), and next use 3D Slicer ExtensionManager to install SlicerIDCBrowser extension. Take a look at the quick demo video in [this post](https://discourse.canceridc.dev/t/sliceridcbrowser-extension-released/515) if you have never used 3D Slicer ExtensionManager before.

Once installed, you can use SlicerIDCBrowser in one of the two modes:

1. **As an interface to explore IDC data**: you can select individual collections, cases and DICOM studies and download items of interest directly into 3D Slicer for subsequent visualization and analysis.
2. **As download tool**: download IDC content based on the manifest you created using IDC Portal, or identifiers of the individual cases, DICOM studies or series.&#x20;

<figure><img src="../../.gitbook/assets/image (24).png" alt=""><figcaption><p>Copy identifiers for the studies/series of interest from the IDC Portal</p></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (26).png" alt=""><figcaption><p>Insert the identifiers in the appropriate fields, or download content defined by the s5cmd manifest</p></figcaption></figure>

### Programmatic download: idc-index python package

{% hint style="warning" %}
`idc-index` package is under development, and its API may change in the future releases!
{% endhint %}

[`idc-index`](https://github.com/ImagingDataCommons/idc-index) is a python package designed to simplify access to IDC data.&#x20;

Once you installed the package with pip install idc-index, you can use it to explore, search, select and download corresponding files as shown in the examples below.

You can also take a look at a short tutorial on using `idc-index` [here](https://github.com/ImagingDataCommons/IDC-Tutorials/blob/master/notebooks/labs/idc\_rsna2023.ipynb).

```shell-session
pip install idc-index
```

```python
from idc_index import index

client = index.IDCClient()

# get identifiers of all collections available in IDC
all_collection_ids = client.get_collections()

# download files for the specific collection, patient, study or series
client.download_from_selection(collection_id="rider_pilot", \
                               downloadDir="/some/dir")
                               
client.download_from_selection(patientId="rider_pilot", \
                               downloadDir="/some/dir")

client.download_from_selection(studyInstanceUID= \
     "1.3.6.1.4.1.14519.5.2.1.6279.6001.175012972118199124641098335511", \
     downloadDir="/some/dir")
                               
client.download_from_selection(seriesInstanceUID=\
     "1.3.6.1.4.1.14519.5.2.1.6279.6001.141365756818074696859567662357", \
     downloadDir="/some/dir")
                               

```

`idc-index` includes a varioety of other helper functions, such as download from the manifest created using IDC portal, automatic generation of the viewer URLs, information about disk space needed for a given collection, and more. We are very interested in your feedback to define the additional functionality to add to this package! Please reach out via [IDC Forum](https://discourse.canceridc.dev/) if you have any suggestions.
