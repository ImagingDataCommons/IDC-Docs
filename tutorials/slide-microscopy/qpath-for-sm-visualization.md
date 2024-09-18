# Using QuPath for visualization

[QuPath](https://qupath.github.io/) is a popular open-source desktop application for visualizing and annotating slide microscopy images. It is integrated with both OpenSlide and BioFormats libraries, and as of the current QuPath 0.5.1 version supports direct loading of DICOM Slide Microscopy images. In this tutorial you will learn how to use DICOM SM images from IDC with QuPath.

### Load a brightfield (RGB) DICOM slide

First you will need to download a sample SM image from IDC to your desktop.  To identify a sample image, you can navigate to the IDC Portal, copy `SeriesInstanceUID` value for a sample SM series you want to download. Given that UID, you can download the corresponding files using `idc-index` python package (see details in the documentation section describing data d[ownload instructions](../../data/downloading-data/)).&#x20;

In this tutorial, we will use the series identified by SeriesInstanceUID from the  [TCGA-ACC](https://portal.imaging.datacommons.cancer.gov/explore/filters/?Modality\_op=OR\&Modality=SM\&collection\_id=tcga\_acc) collection `1.3.6.1.4.1.5962.99.1.3140643155.174517037.1639523215699.2.0`, which you can download as follows:

```sh
idc download 1.3.6.1.4.1.5962.99.1.3140643155.174517037.1639523215699.2.0
```

Next, open QuPath and select `"File > Open"`.

<figure><img src="../../.gitbook/assets/image (28).png" alt=""><figcaption></figcaption></figure>

Choose just one of the `.dcm` files that belong to the desired dataset, then click `Open`. The remaining files will be automatically detected and should not be selected.

When prompted for an image type, select `Brightfield H&E` (or whatever is appropriate for the dataset being opened), then click `Apply`. This is a QuPath feature intended to aid in analysis, and is further described in the [QuPath documentation](https://qupath.readthedocs.io/en/stable/docs/starting/first\_steps.html#setting-the-image-type).

<figure><img src="../../.gitbook/assets/image (29).png" alt=""><figcaption></figcaption></figure>

The image should now display, and can be navigated by zooming/panning as described in the [QuPath documentation](https://qupath.readthedocs.io/en/stable/docs/starting/first\_steps.html#zooming-in-out).

Zooming and panning in real time:



{% embed url="https://docs.google.com/presentation/d/1thFGmSQJo6Eog7_cRv2HjRRilelmfwXXIzk0SNfl4JM/edit#slide=id.g3023d7b7326_0_1" %}

The `Image` tab on the left side of the screen shows dimension information, and lists any associated images. In this case, a thumbnail image is present under `Associated Images` at the bottom of the `Image` tab. Double-clicking on `Series 1 (THUMBNAIL)` will open the thumbnail image in a separate window:

<figure><img src="../../.gitbook/assets/image (31).png" alt=""><figcaption></figcaption></figure>

### Open a fluorescence DICOM dataset

For this part, we will use a slide from the HTAN-OHSU collection identified by SeriesInstanceUID `1.3.6.1.4.1.5962.99.1.1999932010.1115442694.1655562373738.4.0`. As before, you can download it as follows:

```sh
idc download 1.3.6.1.4.1.5962.99.1.1999932010.1115442694.1655562373738.4.0
```

As in the brightfield case, open QuPath and select `File > Open`.

<figure><img src="../../.gitbook/assets/image (32).png" alt=""><figcaption></figcaption></figure>

Choose just one of the `.dcm` files in the dataset, as the other files will be automatically detected. It does not matter which file is selected. When prompted, set the image type to `Fluorescence`, or as appropriate for the dataset:

<figure><img src="../../.gitbook/assets/image (33).png" alt=""><figcaption></figcaption></figure>

The image should then display, and can be navigated by zooming/panning as described in the [QuPath documentation](https://qupath.readthedocs.io/en/stable/docs/starting/first\_steps.html#zooming-in-out).



{% embed url="https://docs.google.com/presentation/d/1cxpkIpp4jqybUnyggdFsfcnBFdHoK4WwiuP-mBL1Q28/edit?usp=sharing" %}

The `Image` tab indicates the number of channels (12 in this case). By default, all channels will be displayed at once. This can be changed by selecting `View > Brightness/Contrast` or the "half-circles" icon in the toolbar:

<figure><img src="../../.gitbook/assets/image (34).png" alt=""><figcaption></figcaption></figure>

Unchecking the `Show` box will hide the channel's data, and update the image.

