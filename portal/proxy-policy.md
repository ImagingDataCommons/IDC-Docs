# IDC Proxy Policy


The mission of the Imaging Data Commons (IDC) is to support cancer imaging research in the cloud. In general,
downloading data from the cloud incurs egress charges. Fortunately, starting in November 2021, the IDC partnered
with the Google Public Data Program, which makes it possible to download
[IDC DICOM files](https://console.cloud.google.com/marketplace/product/gcp-public-data-idc/nci-idc-data) for free.

That free access is for files hosted in Google Cloud Storage buckets. Of course, one crucial aspect of
imaging research that requires downloads is viewing image data, and this service is provided through a DICOMWeb API
hosted on Google, which does not provide free downloads. To support this use case, IDC image viewers provide
free viewing (i.e. downloading) of the IDC image collection, up to a generous daily quota. This quota is
enforced by a proxy service that sits in front of an IDC-private Google Cloud Healthcare DICOMWeb service.

It is the policy of the IDC that this proxy service is only to be used to support our IDC viewers, and
should not be used for other purposes. Our DICOMWeb endpoint is not provided as a way to extract the
entire IDC collection out of the cloud for free, nor as an API to provide IDC data for analysis, even to
cloud-based VMs. Thus, it is not acceptable to “IP hop” in an attempt to circumvent individual daily quotas,
since there is also a global daily cap as well to prevent full egress of the imaging collection. Note that
if this global cap is hit, all other users of the site would be unable to use the viewers for the rest of
the day (using the UTC clock). Thus, IP hopping against the proxy that causes the global quota to be hit
will be considered a denial-of-service attack.

If you, as a researcher, are using the viewer to explore images as a part of your research (e.g. to
fine-tune your cohort selections) and hit your daily quota, please feel free to reach out to us at
<support@canceridc.dev> to discuss the restriction. Note that there are resources under active development to
allow a DICOMWeb interface hosted on a VM that is backed by cloud storage buckets.

Thus, if any of these conditions are met:

1. You repeatedly hit the daily quota
2. There is evidence that multiple IPs are being used to circumvent the quota (and thus are creating a condition where the global cap might be hit)
3. You are consistently downloading to a level just under the quota for an extended stretch of time
4. You are extracting a very large number of DICOM studies in a limited time period, inconsistent with a usage pattern of interactive viewing
5. You are demonstrating other access behaviors or patterns that are inconsistent with individual researchers using the viewer as intended

then the IDC will block the offending IP addresses from using the proxy service. If you are denied from
using our proxy service, and feel that you have been using it in accordance with our policy, or were
unaware of our policy and have violated it unintentionally, please contact us at <support@canceridc.dev> to
lift the restriction.
