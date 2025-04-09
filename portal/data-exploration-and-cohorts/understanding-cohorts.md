# Data selection and download

IDC Portal offers lots of flexibility in selecting items to download. In all cases, download of data from IDC  Portal is a two step process:

1. Select items and export a manifest corresponding to your selection.&#x20;
2. Use command-line python tool or 3D Slicer IDC browser extension to download the files for your selection, as discussed in [Downloading data](../../data/downloading-data/) .

{% hint style="success" %}
"IDC manifest" is a text file that contains URLs to the files in cloud buckets that correspond to your selection. It will contain one line for each DICOM series, as IDC files are organized in series-level folders in the cloud storage.
{% endhint %}

## Downloading content using Cart

You will see "Cart" icon in the search results collections/cases/studies/series tables. Any of the items in these tables can be added to the cart for subsequent downloading of the corresponding files.&#x20;

Get the manifest for the cart content using "Manifest" button in the Cart panel.

<figure><img src="https://github.com/ImagingDataCommons/IDC-Docs/releases/download/v20/cart.gif" alt=""><figcaption></figcaption></figure>

## Downloading all of the files for the current search configuration

Clicking "Manifest" button in the "Cohort Filters" panel will given you the manifest for all of the studies that match your current selection criteria.&#x20;

<figure><img src="../../.gitbook/assets/image (46).png" alt=""><figcaption></figcaption></figure>

## Download individual studies or series

Studies table contains a button for downloading manifest that will contain references to the files in the given study. To download a single series, no manifest is needed. You will see the command line to run to do the download.

<figure><img src="../../.gitbook/assets/image (47).png" alt=""><figcaption></figcaption></figure>

## Downloading images in the viewers

If you would like to download the entire study, or the specific image you see in the image viewer, you can use the download button in the viewer interface.&#x20;

<figure><img src="../../.gitbook/assets/image (50).png" alt=""><figcaption><p>Button to toggle download instructions in IDC radiology (OHIF v3) viewer</p></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (49).png" alt=""><figcaption><p>Button to toggle download instructions in IDC microscopy (Slim) viewer</p></figcaption></figure>
