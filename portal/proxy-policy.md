# Proxy policy

The mission of the Imaging Data Commons \(IDC\) is to support cancer imaging research in the cloud. One consequence of this is that researchers who wish to download imaging data out of the cloud must pay the egress charges they incur using the Google “Requester-Pays” billing model. While researchers must always provide a billable Google Cloud project ID when reading from storage buckets, there are no charges for operations that take place in Google VMs operating in US regions.

Of course, one crucial aspect of imaging research that requires downloads is viewing image data. To support this use case, IDC image viewers provide free viewing \(i.e. downloading\) of the IDC image collection, up to a generous daily quota. This quota is enforced by a proxy service that sits in front of an IDC-private Google Cloud Healthcare DICOMWeb service.

It is the policy of the IDC that this proxy service is only to be used to support our IDC viewers, and should not be used for other purposes. Our DICOMWeb endpoint is not provided as a way to extract the entire IDC collection out of the cloud for free, as an end-run around the requester-pays payment structure. Thus, it is not acceptable to “IP hop” in an attempt to circumvent individual daily quotas, since there is also a global daily cap as well to prevent full egress of the imaging collection. Note that if this global cap is hit, all other users of the site would be unable to use the viewers for the rest of the day \(using the UTC clock\). Thus, IP hopping against the proxy that causes the global quota to be hit will be considered a denial-of-service attack.

If you, as a researcher, are using the viewer to explore images as a part of your research \(e.g. to fine-tune your cohort selections\) and hit your daily quota, please feel free to reach out to us at [support@canceridc.dev](mailto:support@canceridc.dev) to discuss the restriction. Also, note that we are actively investigating waiving the quotas for cloud VMs that are operating in the same cloud region as the proxy service, which would not incur egress charges. If this capability would be useful for your research, please contact us.

Thus, if any of these conditions are met:

1. You repeatedly hit the daily quota
2. There is evidence that multiple IPs are being used to circumvent the quota \(and thus are creating a condition where the global cap might be hit\)
3. You are consistently downloading to a level just under the quota for an extended stretch of time
4. You are demonstrating other access behaviors or patterns that are inconsistent with individual researchers using the viewer as intended

then the IDC will block the offending IP addresses from using the proxy service. If you are denied from using our proxy service, and feel that you have been using it in accordance with our policy, or were unaware of our policy and have violated it unintentionally, please contact us at [support@canceridc.dev](mailto:support@canceridc.dev) to lift the restriction.

