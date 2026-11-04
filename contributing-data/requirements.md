# Requirements

IDC stakeholders decide which contributions are accepted. They are the IDC team leads, project managers and federal leads. Every contribution is reviewed individually, and acceptance is not guaranteed.

## Public access and license

IDC only hosts public data. Your data must be available to anyone without a data use or data transfer agreement.

[CC BY 4.0](https://creativecommons.org/licenses/by/4.0/) is strongly preferred. Non-commercial licenses are strongly discouraged and are considered only under exceptional circumstances. See [Licensing and attribution](../data/licensing.md) for how licenses apply to IDC data.

Why CC BY:

* **It lets everyone use the data.** IDC's goal is to accelerate scientific discovery and the search for cancer cures. Academic and commercial researchers both do that work, and a non-commercial (NC) license excludes the commercial ones.
* **NC licenses leave model training in doubt.** It is not settled whether a model trained on NC data is itself bound by the NC clause. There is no established precedent, and it has been argued that trained model weights may be considered a derivative work of the training data ([Franceschelli et al., 2024](https://doi.org/10.48550/arXiv.2407.13493)).

## Evidence of value

Tell IDC why the data is worth hosting. Examples:

* the data was collected under a competitive research grant
* the data is described in a peer-reviewed publication
* for manual annotations: the qualifications of the experts who made them
* for ML results: published or otherwise established performance of the model, and documented quality assurance

## De-identification

{% hint style="warning" %}
IDC does not de-identify data, and does not check the results of de-identification. Making sure the data is de-identified is your responsibility.
{% endhint %}

You have two options:

* **Use an entity IDC already recognizes as qualified to de-identify your type of data.** IDC has already reviewed how such an entity de-identifies data, so it does not review that again for each dataset.
* **De-identify the data yourself, or use an entity IDC has not reviewed before.** In this case you submit documentation describing how the data was de-identified. IDC reviews the documentation, but not the data, and gives you feedback. Acceptance is not guaranteed.

How closely IDC reviews the documentation depends on the type of data:

* **Digital pathology slides in vendor formats** are easier. IDC converts them to DICOM and controls which metadata is carried over.
* **DICOM radiology** is riskier, because identifying information can remain in many places in the DICOM headers and in the pixel data. TCIA is currently the only entity IDC recognizes as qualified to de-identify DICOM radiology.

## DICOM format

All data in IDC is in DICOM. If your data is in another format, IDC considers it only when conversion to DICOM is feasible. IDC then either converts it or guides you through the conversion (see [Preparing data in DICOM](preparing-data-in-dicom.md)).

Ready to get in touch? See [Contribution process](contribution-process.md).
