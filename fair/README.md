# IDC and the FAIR principles

The FAIR Guiding Principles ([Wilkinson et al., 2016](https://doi.org/10.1038/sdata.2016.18)) describe what makes research data **F**indable, **A**ccessible, **I**nteroperable and **R**eusable, by people and, above all, by software. This page explains how the data in NCI Imaging Data Commons (IDC) meets each of the 15 principles, what evidence supports each claim, and where the gaps are.

It is written for two kinds of readers:

* **If you are evaluating IDC** - for a funder, a data management plan or a journal - the [summary](#summary), the [status table](#status-by-principle) and the plain-language explanations under each principle are all you need. No imaging background is assumed, and terms are defined in the [glossary](#glossary).
* **If you want to check the claims**, each principle has an expandable note for specialists, and [Evidence and reproduction](evidence.md) gives the method, the full tables and the queries to rerun them.

{% hint style="info" %}
This is a self-assessment by the IDC team. It is not a certified FAIR score, and IDC was not benchmarked against other repositories. Counts are as of IDC data release v25 (October 2026), and will change with each release.
{% endhint %}

## Summary <a href="#summary" id="summary"></a>

IDC meets all 15 FAIR principles. In plain terms:

* **Every one of its 1,044,191 image series has a permanent, unique identifier**, and every series is linked to a citable dataset with a DOI.
* **Anyone can download any of it**, from Amazon Web Services or Google Cloud, with no registration, no access request and no fees.
* **All images and annotations are stored in DICOM, the international standard for medical imaging** - including the results of AI analysis - so standard software can open all of them.
* **Every series carries its license**, and more than 95% of series allow commercial reuse.
* **Every series records who contributed it, who de-identified it and who converted it to DICOM.**
* **The complete metadata of every release since the first one is still openly available**, so an analysis can be traced back to exactly the data it used.

The gaps are in the descriptions that came with the data, not in access to it:

* **The body part examined is missing for many scans** - a third of MRI and nearly half of PET series - which makes searching by anatomy unreliable.
* **Clinical (non-imaging) data accompanies 100 of the 179 collections, and is not harmonized across them.**
* **For 14 of the 242 datasets, the license IDC records differs from the license in the dataset's DOI record.** These are [under review](#r1-1).

The first two are places where [IDC welcomes help](#help-wanted).

## Status by principle <a href="#status-by-principle" id="status-by-principle"></a>

Each principle is marked **Met** when it holds for essentially all of IDC, with any exceptions noted, or **Met, with known gaps** when it holds but the gaps affect routine use.

| Principle            | What it asks                                                   | Status               | Key evidence                                                                                                                           |
| -------------------- | -------------------------------------------------------------- | -------------------- | -------------------------------------------------------------------------------------------------------------------------------------- |
| [**F1**](#f1)        | Data has a globally unique, persistent identifier              | Met                  | All 1,044,191 series have a unique DICOM identifier and a version-specific IDC identifier; 242 dataset DOIs                            |
| [**F2**](#f2)        | Data is described with rich metadata                           | Met, with known gaps | 31 series-level fields plus tables for CT, MR, PET, pathology, segmentations and more; body part missing for 33% of MR and 45% of PET series |
| [**F3**](#f3)        | The metadata includes the identifier of the data               | Met                  | Every metadata record carries the series identifier and its download location                                                          |
| [**F4**](#f4)        | Data is indexed in a searchable resource                       | Met                  | Listed in re3data, FAIRsharing and the RRID registry; AWS Registry of Open Data; dataset DOIs registered with DataCite                  |
| [**A1**](#a1)        | Data is retrievable by its identifier over a standard protocol | Met                  | Every series downloadable anonymously from both clouds; DICOMweb retrieval by DICOM identifier without sign-in                         |
| [**A1.1**](#a1-1)    | The protocol is open, free and universally implementable       | Met                  | HTTPS cloud storage interfaces, DICOMweb (part of the DICOM standard), a documented REST API                                           |
| [**A1.2**](#a1-2)    | The protocol allows authentication where necessary             | Met                  | The protocols support it; IDC uses open access because its data is de-identified and public                                            |
| [**A2**](#a2)        | Metadata stays accessible when the data no longer is           | Met                  | Full metadata of every release, v1 to v25, openly available; version history lists 13,208 series no longer in IDC                      |
| [**I1**](#i1)        | A formal, shared language represents the data                  | Met                  | All data in DICOM (ISO 12052): 38 object types, 25 modality codes, only DICOM-defined encodings                                        |
| [**I2**](#i2)        | Vocabularies follow FAIR principles                            | Met, with known gaps | 99.8% of segment codes are SNOMED CT; body part uses DICOM defined terms; clinical data not harmonized                                 |
| [**I3**](#i3)        | References to other data are explicit                          | Met                  | 99.3% of segmentations, contours and pathology annotations name the series they annotate, and every reference resolves                |
| [**R1**](#r1)        | Data has many accurate and relevant attributes                 | Met, with known gaps | Equipment, acquisition parameters, 3D-geometry checks; clinical tables for 100 of 179 collections                                      |
| [**R1.1**](#r1-1)    | Data has a clear, accessible usage license                     | Met, with known gaps | 100% of series labelled; license URL per dataset; SPDX identifiers in DataCite and on this page; labels of 14 of 242 datasets disagree with their DOI records and are under review |
| [**R1.2**](#r1-2)    | Data has detailed provenance                                   | Met                  | Per-series DOI, record of who contributed, de-identified and converted it, version history, and how each segmentation was made         |
| [**R1.3**](#r1-3)    | Data meets domain-relevant community standards                 | Met                  | Native DICOM throughout; data submitted in other formats converted to DICOM                                                            |

## Background

### FAIR in brief

The FAIR Guiding Principles ([Wilkinson et al., 2016](https://doi.org/10.1038/sdata.2016.18)) list 15 properties that make research data findable, accessible, interoperable and reusable. Their distinctive emphasis is on machines: a computer program, not only a person, should be able to find data, tell what it is, retrieve it and combine it with other data, with minimal human help.

FAIR is not the same as open. Restricted data can be FAIR if the rules for access are clear and the access protocol can enforce them. IDC happens to be both FAIR and open.

### IDC in brief

IDC is the National Cancer Institute's cloud-based repository of public cancer imaging data, and part of the NCI [Cancer Research Data Commons](https://datacommons.cancer.gov/). It gathers images and image-derived data from sources such as The Cancer Imaging Archive (TCIA), the Genomic Data Commons and the Human Tumor Atlas Network, and harmonizes all of it into one format (see [Core functions](../core-functions-of-idc.md)). Data is de-identified before it is released.

| Contents of release v25                                                      |      Count |
| ---------------------------------------------------------------------------- | ---------: |
| Collections (original datasets)                                              |        179 |
| Analysis results (annotations and AI outputs derived from images in IDC)     |         26 |
| Patients                                                                     |     86,451 |
| Studies                                                                      |    168,507 |
| Series                                                                       |  1,044,191 |
| Instances (files)                                                            | 58,784,975 |
| Total size                                                                   |    95.3 TB |

### How imaging data is organized

Medical imaging data has a four-level hierarchy, and IDC counts most things at the **series** level:

| Level        | What it is                                                                | Example: one lung cancer screening exam                                                                                                                                                   |
| ------------ | ------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Patient**  | One (de-identified) person                                                | A participant in a lung cancer screening trial                                                                                                                                            |
| **Study**    | One imaging exam, such as a chest CT on a given day                       | The participant's screening CT from one year of the trial                                                                                                                                 |
| **Series**   | One set of images from the exam, or one set of annotations made on them   | A typical exam in IDC has about eight series: the CT images, often reconstructed more than once for different purposes, plus AI segmentations of organs and reports of measurements made from them |
| **Instance** | One file, such as one image slice                                         | One of about 150 cross-sectional slices in a CT series                                                                                                                                    |

In pathology, one scanned slide is one series.

**DICOM** is the international standard for medical images, used by essentially every clinical scanner and hospital image archive. Each DICOM file carries both the pixels and a structured header describing the equipment, the acquisition and the (de-identified) patient - comparable to the information a phone stores with a photo, but far richer and standardized. See [Introduction to DICOM](../dicom/introduction.md).

Not every series is an image. **Nearly half of the series in IDC (47%) are derived objects**: segmentations that outline organs or tumors (SEG), structured reports holding measurements and findings (SR), radiotherapy contours (RTSTRUCT) and pathology annotations (ANN). Storing them in DICOM, next to the images they describe, is central to IDC's interoperability. See [Derived objects](../dicom/derived-objects/README.md).

## Findable

### F1. Data is assigned a globally unique and persistent identifier <a href="#f1" id="f1"></a>

_What it asks:_ every piece of data has an identifier that no other data anywhere shares, and that does not change.

_What IDC provides:_

* Every one of the 1,044,191 series carries a DICOM **Series Instance UID**, and no two are the same.
* Every series also carries an **IDC UUID** (`crdc_series_uuid`) that identifies one specific _version_ of it. When a series is revised, the new version gets a new UUID and the old version stays available under the old one, so a researcher can cite or re-download exactly the version they used. See [Data versioning](../data/data-versioning.md).
* Every series belongs to one of **242 datasets with a DOI**, the same kind of persistent link used for journal articles.

<details>

<summary>For specialists</summary>

Series Instance UIDs are ISO OID-based identifiers defined in DICOM PS3.5. They are globally unique and never reused, but there is no global resolver for them: IDC resolves them through its own interfaces (`idc-index`, the REST API, DICOMweb). Resolution through a global resolver is provided at dataset level, by the DOIs.

The IDC UUID doubles as the storage prefix (`s3://<bucket>/<crdc_series_uuid>/`), so a list of UUIDs is an exact, version-pinned definition of a cohort.

</details>

### F2. Data is described with rich metadata <a href="#f2" id="f2"></a>

_What it asks:_ the description of the data is detailed enough for someone to decide whether it is relevant.

_What IDC provides:_

* The main metadata table describes each series with **31 fields**: patient, study and series descriptions, equipment manufacturer and model, dates, modality, encoding, license and size.
* Specialized tables add the detail that general catalogs usually drop: CT acquisition and reconstruction parameters (21 fields), MR sequence parameters (22), PET radiopharmaceutical and reconstruction parameters (21), and tables for pathology slides, segmentations, radiotherapy structure sets, pathology annotations, contrast agents and 3D volume geometry.
* Beyond these curated tables, **the DICOM attributes of every file are searchable**, without login, in the metadata IDC publishes with each release (see [Files and metadata](../data/organization-of-data/files-and-metadata.md)).

_Known gap:_ the body part examined, which is recorded by the site that acquired the scan, is missing for 11.7% of CT, 33.1% of MR, 45.0% of PET and 94.0% of ultrasound series. It is complete for mammography and radiography. This is the most visible weakness of IDC's metadata, because anatomy is one of the first filters users reach for. See [Help wanted](#help-wanted).

<details>

<summary>For specialists</summary>

Body Part Examined (0018,0015) is Type 3 (optional) in the General Series Module, so these gaps are conformant. It does not apply to slide microscopy, where `sm_index` records the anatomic location the specimen was taken from. What a segmentation outlines is recorded separately, as coded segment properties (see [I2](#i2)), and does not depend on Body Part Examined.

</details>

### F3. Metadata clearly includes the identifier of the data it describes <a href="#f3" id="f3"></a>

_What it asks:_ the description and the data cannot become separated.

_What IDC provides:_ every metadata record carries the identifiers of the series and the location to download it from. Derived objects also name the series they annotate (see [I3](#i3)). In the other direction, each DICOM file carries its own identifiers in its header, so a downloaded file can always be matched back to its metadata and its license.

### F4. Data is registered or indexed in a searchable resource <a href="#f4" id="f4"></a>

_What it asks:_ the data can be found through search tools, not only by people who already know where to look.

_What IDC provides:_

* **Research-data registries.** IDC is listed in [re3data](https://www.re3data.org/repository/r3d100014074), [FAIRsharing](https://fairsharing.org/10.25504/FAIRsharing.0b5a1d) and the SciCrunch RRID registry (RRID:SCR\_019127), and the records cross-reference one another. Funders, journals and data management planning tools use these registries to point researchers to repositories. The re3data record lists IDC's REST, DICOMweb and MCP interfaces, and the FAIRsharing record links IDC to the [DICOM standard](https://fairsharing.org/FAIRsharing.b7z8by).
* **Cloud catalogs.** IDC is listed in the [AWS Registry of Open Data](https://registry.opendata.aws/nci-imaging-data-commons/) and in the [Google Cloud public datasets](https://cloud.google.com/healthcare-api/docs/resources/public-datasets/idc) program.
* **DOI registration.** All 242 dataset DOIs, from TCIA (`10.7937/...`) and Zenodo (`10.5281/...`), are registered with DataCite and publicly findable there, which makes them discoverable through DataCite-indexed search services.
* **IDC's own search tools.** The [IDC Portal](https://portal.imaging.datacommons.cancer.gov/explore/) in a web browser, the [`idc-index`](https://github.com/ImagingDataCommons/idc-index) Python package for SQL search on a laptop, the [REST API](../api/README.md), an [MCP server](../mcp/README.md) for LLM assistants, and BigQuery.

## Accessible

### A1. Data is retrievable by its identifier using a standardized protocol <a href="#a1" id="a1"></a>

_What it asks:_ once you have the identifier, you can get the data with standard tools.

_What IDC provides:_

* **100% of series** have a public download location, and all data is mirrored on **both AWS and Google Cloud**.
* Downloads are **anonymous and free**: no registration, no access request, no cloud account and no egress fees, through the AWS Open Data Sponsorship Program and the Google Cloud Public Datasets Program.
* **Retrieval works directly from identifiers.** The `idc` command-line tool downloads a collection, patient, study or series given its identifier, and IDC's DICOMweb endpoint retrieves by DICOM UID without sign-in.
* Data can be **viewed in a web browser without downloading**, in the open-source OHIF and VolView viewers for radiology and Slim for pathology.

See [Downloading data](../data/downloading-data/README.md).

<details>

<summary>For specialists</summary>

IDC offers two DICOMweb endpoints (see [DICOM stores](../data/organization-of-data/dicom-stores.md)). The IDC-maintained store, reached through a proxy, holds all of the current release and needs no authentication, but has per-IP and daily quotas (see [Proxy policy](../portal/proxy-policy.md)). The store maintained by Google Healthcare holds most of the data and needs a Google account, which anyone can use.

</details>

### A1.1. The protocol is open, free and universally implementable <a href="#a1-1" id="a1-1"></a>

Files are downloaded over HTTPS through the standard cloud storage interfaces, which many free tools implement, such as [`s5cmd`](../data/downloading-data/downloading-data-with-s5cmd.md), the AWS and Google Cloud command-line tools, and the `idc` command of `idc-index`. DICOMweb is part of the DICOM standard (PS3.18) and is implemented by open-source and commercial software alike. The [REST API](../api/README.md) is plain HTTP and is described in OpenAPI.

### A1.2. The protocol allows authentication and authorization where necessary <a href="#a1-2" id="a1-2"></a>

_What it asks:_ this principle does not require data to be closed. It asks that the access method _can_ enforce access rules when data needs them.

_What IDC provides:_ the cloud storage and DICOMweb protocols IDC uses fully support authentication. IDC deliberately uses anonymous access, because its data is de-identified and public. Two secondary routes involve signing in with a free Google account: BigQuery, and the DICOMweb store maintained by Google Healthcare.

### A2. Metadata remains accessible even when the data is no longer available <a href="#a2" id="a2"></a>

_What it asks:_ if data is withdrawn or replaced, its description and identifier should not vanish.

_What IDC provides:_

* **A stated policy.** Each data release is a fixed snapshot, and "all versions of all data, except data removed due to PHI/PII concerns, should continue to be available" (see [Data versioning](../data/data-versioning.md)).
* **The complete metadata of every release, from v1 to v25**, is published in a public bucket and can be read without login (see [Files and metadata](../data/organization-of-data/files-and-metadata.md)). This covers releases whose data has since been revised or removed.
* **A version history** (`prior_versions_index`) lists 71,042 earlier versions of series that the current release no longer serves, including 13,208 series no longer in IDC, each with the releases that served it.
* Every current series records the release it **first appeared** in and the release it was **last revised** in. 49,304 series (4.7%) have been revised since they were first released.
* Dataset DOIs resolve to landing pages hosted by TCIA and Zenodo, which persist independently of IDC.

## Interoperable

### I1. Data uses a formal, accessible, shared language for knowledge representation <a href="#i1" id="i1"></a>

_What it asks:_ the data is stored in a well-defined, published format that other systems understand.

_What IDC provides:_ this is IDC's defining property.

* **All images and all annotations are DICOM**, a freely available international standard, also published as ISO 12052 (see its [FAIRsharing record](https://fairsharing.org/FAIRsharing.b7z8by)).
* IDC contains **38 types of DICOM object** across **25 modality codes**, stored in **16 encodings** (or combinations of encodings within a series), all defined by the DICOM standard.
* In practice, any DICOM-capable software - and there are hundreds of free and commercial packages - can open any IDC file without a custom reader.
* **Scope:** the clinical tables are ordinary tabular data, not DICOM.

<details>

<summary>For specialists</summary>

Readers from the FAIR community often take "knowledge representation language" to mean RDF or OWL. DICOM is a formal information model - object definitions, a data dictionary, coded structured-reporting templates - with standard JSON and XML encodings (PS3.18 Annex F, PS3.19), which is the sense in which it meets I1.

260 series use the retired Explicit VR Big Endian transfer syntax, which some modern toolkits handle poorly. 18 series in the Lung-Fused-CT-Pathology collection use the Modality value `FUSION`, which is not a DICOM defined term; this is how the data was submitted (see [Inherited from source data](#inherited-from-source-data)). The full counts are in [Evidence and reproduction](evidence.md).

</details>

### I2. Metadata uses vocabularies that follow FAIR principles <a href="#i2" id="i2"></a>

_What it asks:_ descriptions use shared lists of coded terms, so "liver" is always the same code rather than "Liver", "LIVER" or "hepatic".

_What IDC provides:_

* **What each segmentation outlines is coded.** Every one of the 9.7 million segments in IDC's segmentations carries a coded property type, and 99.8% of these codes come from SNOMED CT, the most widely used clinical terminology.
* **Body part uses DICOM's list of defined terms**, each of which maps to a SNOMED CT code. 61 of the 76 distinct values in IDC are defined terms, and they account for 99.85% of the series that have a value.
* Structural fields - modality, object type, encoding - use terms defined by DICOM.

_Known gaps:_

* **Body part** is missing for many scans (see [F2](#f2)), and sites choose different levels of detail: a chest CT may be labelled `CHEST` or `LUNG`, which are different defined terms.
* **Clinical data is not harmonized.** Column names and value codes vary by collection. A data dictionary with 10,661 entries explains them, but they are not mapped to a common vocabulary or data model. See [Help wanted](#help-wanted).

<details>

<summary>For specialists</summary>

Body Part Examined values map to SNOMED CT codes through DICOM PS3.16 Annex L. `CHEST` and `THORAX` map to the same code; `LUNG` maps to a different, more specific one. The 15 values outside Annex L appear on 835 series, and include `Left Breast`, `LEG` and protocol strings such as `TH CT CHEST WO C`.

Besides SNOMED CT, segment property types use the retired SNOMED-RT designator `SRT` (9,463 segments, from older conversions), the NCI Thesaurus, DICOM's own codes, and a local scheme, `MRSC`, in two trial collections (ACRIN-6698 and ISPY2). Segment Algorithm Type (0062,0008) takes the enumerated values `AUTOMATIC`, `SEMIAUTOMATIC` and `MANUAL`; 462 series in QIBA CT-1C use `SEMIAUTOMATED`. All of these came with the data from its submitters (see [Inherited from source data](#inherited-from-source-data)).

</details>

### I3. Metadata includes qualified references to other data <a href="#i3" id="i3"></a>

_What it asks:_ derived data says exactly what it was derived from. An outline of a tumor must say which scan it was drawn on.

_What IDC provides:_

| Derived object type                | Series  | Name the series they annotate | Of those, the series exists in IDC |
| ---------------------------------- | ------: | ----------------------------: | ---------------------------------: |
| Segmentations (SEG)                | 193,283 |              191,791 (99.2%) |                     191,791 (100%) |
| Radiotherapy structure sets (RTSTRUCT) | 19,366 |              19,366 (100%) |                      19,366 (100%) |
| Pathology annotations (ANN)        |   7,448 |                 7,448 (100%) |                       7,448 (100%) |
| **Total**                          | **220,097** |        **218,605 (99.3%)** |                 **218,605 (100%)** |

Every recorded reference points to a series that actually exists in IDC. Each of the 26 analysis results also names the collections it analyzed and its own DOI.

_Known gap:_ 1,492 segmentation series do not name the series they annotate. Nearly all of them came this way from their submitters.

<details>

<summary>For specialists</summary>

References were checked at series level, not at instance or frame level. Structured reports (270,687 SR series) were not included in this check.

</details>

## Reusable

### R1. Data is richly described with accurate and relevant attributes <a href="#r1" id="r1"></a>

_What it asks:_ there is enough context to judge whether data is fit for a new purpose.

_What IDC provides:_

* **Imaging context:** equipment manufacturer and model, de-identified dates, patient age and sex, acquisition parameters, contrast agents, and automated checks of whether each CT, MR or PET series forms a regular 3D volume, which matters for AI work.
* **Clinical data:** 235 tables - demographics, diagnosis, staging, therapy, outcomes - accompany **100 of the 179 collections (56%)**, with a data dictionary of 10,661 entries. See [Clinical data](../data/organization-of-data/clinical.md).

_Known gap:_ 79 collections have no clinical tables in IDC. Some may have clinical data at their original source.

### R1.1. Data is released with a clear and accessible usage license <a href="#r1-1" id="r1-1"></a>

_What IDC provides:_

* **100% of series carry a license label**, which can be used as a search filter.
* Each dataset's record in IDC's metadata carries the license name and a link to the license text, and the DataCite records of the dataset DOIs carry the [SPDX](https://spdx.org/licenses/) identifier of the license (all but one: the NLM terms have no SPDX identifier).

| License                                    | SPDX identifier | Series  | Commercial use  |
| ------------------------------------------ | --------------- | ------: | --------------- |
| CC BY 4.0                                  | `CC-BY-4.0`     | 877,215 | allowed         |
| CC BY 3.0                                  | `CC-BY-3.0`     | 132,303 | allowed         |
| CC BY-NC 4.0                               | `CC-BY-NC-4.0`  |  28,783 | **not allowed** |
| CC BY-NC 3.0                               | `CC-BY-NC-3.0`  |   5,851 | **not allowed** |
| NLM Terms and Conditions; May 21, 2019     | none            |      39 | see terms       |

As labelled in v25, about 97% of series allow commercial reuse; however the review below is resolved, the share stays above 95%. Every license requires attribution. See [Licensing and attribution](../data/licensing.md) for how to check the license of your selection and how to cite it.

_Known gap:_ IDC compared its license labels with the license recorded in DataCite for each of the 242 dataset DOIs. They agree for 228. For 14 datasets (29,827 series) they disagree, and all 14 are under review. For three of them, the disagreement decides whether commercial use is allowed:

| Dataset                        | Series | IDC label (v25) | DOI record (DataCite) | Status                                                                                       |
| ------------------------------ | -----: | --------------- | --------------------- | -------------------------------------------------------------------------------------------- |
| ACRIN-6698                     | 18,747 | CC BY 4.0       | CC BY-NC 4.0          | TCIA's collection page and the DOI record disagree; reported to TCIA (ticket TH-60573)       |
| Phantom FDA                    |  4,433 | CC BY-NC 3.0    | CC BY 4.0             | CC BY 4.0 is correct; TCIA has corrected it, and IDC's metadata will follow in a future release |
| NSCLC-Radiomics-Interobserver1 |     64 | CC BY-NC 3.0    | CC BY 3.0             | Under review                                                                                 |

For the other 11, the two differ only in the version of the license (3.0 or 4.0), or the DOI record lists more than one license.

{% hint style="warning" %}
Until the review is complete, assume that the more restrictive of the two licenses applies to the datasets in this table, except Phantom FDA, whose license is CC BY 4.0.
{% endhint %}

<details>

<summary>For specialists</summary>

The comparison took the SPDX identifiers from the `rightsList` of each DOI's DataCite record (retrieved through the DataCite REST API in November 2026) and compared them with the SPDX identifier of the license IDC records for that DOI. The 11 datasets whose labels differ in version or list more than one license:

| Dataset                          | DOI                             | Series | IDC label (v25) | DOI record (DataCite)                              |
| -------------------------------- | ------------------------------- | -----: | --------------- | -------------------------------------------------- |
| NLST, slide microscopy           | `10.5281/zenodo.12689650`       |  1,259 | CC BY 3.0       | CC BY 4.0                                          |
| CPTAC-LSCC, slide microscopy     | `10.5281/zenodo.12666807`       |  1,081 | CC BY 4.0       | CC BY 3.0                                          |
| RIDER Lung CT                    | `10.7937/k9/tcia.2015.u1x8a5nr` |    936 | CC BY 4.0       | CC BY 3.0                                          |
| CPTAC-CCRCC, slide microscopy    | `10.5281/zenodo.12666768`       |    782 | CC BY 4.0       | CC BY 3.0                                          |
| LDCT-and-Projection-data         | `10.7937/9npb-2637`             |    698 | CC BY 4.0       | NIH Controlled Data Access Policy; CC BY 3.0       |
| CPTAC-PDA, slide microscopy      | `10.5281/zenodo.12666859`       |    557 | CC BY 4.0       | CC BY 3.0                                          |
| CPTAC-GBM, slide microscopy      | `10.5281/zenodo.12666788`       |    462 | CC BY 4.0       | CC BY 3.0                                          |
| CPTAC-HNSCC, slide microscopy    | `10.5281/zenodo.12666803`       |    390 | CC BY 4.0       | CC BY 3.0                                          |
| QIN-Breast-02                    | `10.7937/tcia.2019.4cfm06rr`    |    235 | CC BY 4.0       | CC BY 4.0; CC BY 3.0                               |
| CPTAC-AML, slide microscopy      | `10.5281/zenodo.12666740`       |    122 | CC BY 4.0       | CC BY 3.0                                          |
| MRI-DIR                          | `10.7937/k9/tcia.2018.3f08iejt` |     61 | CC BY 4.0       | CC BY 4.0; CC BY 3.0                               |

The seven Zenodo records are IDC's own: they describe IDC's DICOM conversions of pathology slides, and their provenance record names IDC as contributor and converter, so correcting them falls to IDC. The others are TCIA records. The NLM Visible Human dataset is not a disagreement: both records name the NLM terms, which have no SPDX identifier.

</details>

### R1.2. Data is associated with detailed provenance <a href="#r1-2" id="r1-2"></a>

_What it asks:_ you can tell who produced the data, how, and how it has changed.

_What IDC provides:_

* Every series links to the **DOI of the dataset** it belongs to, which describes the project that produced it.
* Every series has a **provenance record** naming who contributed it to IDC, who provided the source material, who de-identified it and who converted it to DICOM (in `collections_index` for original data, and in `analysis_results_index` for analysis results).
* Every series records **when it entered IDC and when it was last revised**, and earlier versions are retained (see [A2](#a2)).
* **Segmentations record how they were made:** 85.3% automatic (AI), 7.6% manual, 5.5% semi-automatic, and 1.6% mixed or non-standard. Users who need expert-drawn labels can filter on this.
* Because every series resolves to a DOI, **citations can be generated automatically**: `idc-index` and the MCP server produce them in APA, BibTeX and other formats. The TCGA-BRCA collection, for example, yields five dataset citations, plus the IDC paper.

### R1.3. Data meets domain-relevant community standards <a href="#r1-3" id="r1-3"></a>

_What IDC provides:_ DICOM is the standard of clinical imaging, so IDC data fits directly into hospital, research and commercial imaging software. IDC also **converts data that arrives in other formats into DICOM**: pathology slides in vendor-specific formats became DICOM slide microscopy images, the NLM Visible Human images became DICOM CT, MR and photographs, and prostate segmentations delivered as STL surface files became DICOM 3D-model objects. See [Preparing data in DICOM](../contributing-data/preparing-data-in-dicom.md).

_Known gap:_ clinical data does not follow a common community data model (see [I2](#i2)).

## Known gaps

### Help wanted <a href="#help-wanted" id="help-wanted"></a>

These gaps need effort beyond what IDC can take on alone. If you can help, see [Contributing data](../contributing-data/README.md) or contact [support@canceridc.dev](mailto:support@canceridc.dev).

* **Missing body part.** Assigning body part reliably to scans that lack it would take a robust AI tool, substantial compute and probably expert review, and IDC does not plan to do it at present. IDC welcomes body-part or anatomic-region labels for its images, contributed as derived data.
* **Clinical data harmonization.** IDC is about images. By making the clinical data that accompanies them easy to reach, we hope to find partners interested in normalizing it - for example, mapping core elements such as age at diagnosis, stage, histology and vital status to common data elements.

### Inherited from source data <a href="#inherited-from-source-data" id="inherited-from-source-data"></a>

IDC is deliberately conservative about changing DICOM data as its source delivered it. Most radiology data comes from TCIA, which de-identifies and curates it, and harmonizing that data is the responsibility of the submitter. The DICOM data that IDC creates by conversion, IDC fixes itself. The [provenance record](#r1-2) of each series says who contributed, de-identified and converted it, so you can tell which is which.

Known issues of this kind:

* 1,492 segmentation series do not name the series they annotate ([I3](#i3)).
* Some values fall outside DICOM's lists: 835 series with non-standard body part values, 462 segmentation series with the algorithm type `SEMIAUTOMATED`, and 18 series with the modality `FUSION` ([I1](#i1), [I2](#i2)).
* Some segmentations use retired or local coding schemes ([I2](#i2)).

### In progress <a href="#in-progress" id="in-progress"></a>

* **Dataset landing pages** are limited or missing for many datasets, and some are owned by the submitter. IDC is populating collection pages of its own.
* **Registry records.** re3data cannot yet list DICOM as IDC's standard, because DICOM is not among the standards re3data allows; we are contacting its maintainers.
* **License review.** The 14 datasets whose IDC license label disagrees with their DOI record are under review: with TCIA for its datasets, and by IDC for the records of its own DICOM conversions. Corrections will reach IDC's metadata in future releases, starting with Phantom FDA ([R1.1](#r1-1)).

## What this means for data users

* **Check the license before commercial use.** Filter on the license of each series rather than inferring it from the collection - see [Licensing and attribution](../data/licensing.md). For the datasets whose license is [under review](#r1-1), assume the more restrictive license until the review is complete.
* **Do not search by body part alone.** Combine it with collection descriptions, and for segmentations use the coded anatomy in `seg_index`.
* **Record the IDC release and the series UUIDs, or the download manifest.** Earlier versions remain available, so this makes an analysis exactly reproducible.
* **Cite both the dataset DOIs and the IDC paper** - see [Licensing and attribution](../data/licensing.md).
* **Check for clinical data early.** 79 collections have no clinical tables in IDC, which matters for outcome studies.
* **Know whether annotations come from people or from AI.** 85% of segmentation series are automatic; filter on the algorithm type if you need expert labels.

## Limitations of this assessment

* **Self-assessment.** The IDC team assessed its own resource. This is not a certified FAIR score, and IDC was not benchmarked against other repositories.
* **Metadata only.** The findings come from IDC's published metadata; files were not downloaded and validated for DICOM conformance.
* **Scope of the reference check.** [I3](#i3) covers segmentations, radiotherapy structure sets and pathology annotations, at series level.
* **Not evaluated:** portal usability, service uptime, documentation quality, and machine-readable metadata on dataset landing pages.
* **One release.** The figures describe v25, and will change with future releases.
* **Some gaps are legitimate.** Body part is optional in DICOM, and does not apply to pathology slides.

## Glossary <a href="#glossary" id="glossary"></a>

| Term                     | Meaning                                                                                                                         |
| ------------------------ | ------------------------------------------------------------------------------------------------------------------------------- |
| **Analysis result**      | A dataset of annotations or AI outputs created from images already in IDC, published with its own DOI                          |
| **ANN**                  | DICOM microscopy annotation, such as outlines of cell nuclei on a pathology slide                                               |
| **BigQuery**             | Google's cloud SQL database service; IDC publishes its metadata there                                                          |
| **CC BY / CC BY-NC**     | Creative Commons licenses. CC BY allows any reuse with attribution; CC BY-NC does not allow commercial use                      |
| **Collection**           | An original dataset in IDC, usually from one study or project                                                                   |
| **Controlled vocabulary**| An agreed list of terms with codes, so the same concept is always recorded the same way                                         |
| **De-identified**        | With personal identifiers removed or replaced, and dates shifted, to minimize the risk that patients can be identified          |
| **DICOM**                | Digital Imaging and Communications in Medicine: the international standard for medical images and related data                 |
| **DICOMweb**             | The web (HTTP) interface defined by DICOM for searching and retrieving images                                                   |
| **DOI**                  | Digital Object Identifier: a permanent link to a dataset or publication                                                         |
| **Egress fee**           | A charge cloud providers normally make for downloading data out of their cloud; waived for IDC                                  |
| **`idc-index`**          | IDC's Python package for searching and downloading IDC data                                                                     |
| **Instance**             | A single DICOM file, such as one image slice                                                                                    |
| **MCP**                  | Model Context Protocol: an open protocol through which LLM assistants use external tools and data                               |
| **Modality**             | The kind of equipment or object: CT, MR (MRI), PT (PET), SM (slide microscopy), SEG, SR and so on                               |
| **OHIF / VolView / Slim**| Free, open-source web viewers for radiology (OHIF, VolView) and pathology (Slim)                                               |
| **Provenance**           | The record of where data came from and how it was produced and changed                                                          |
| **RTSTRUCT**             | DICOM radiotherapy structure set: contours of tumors and organs used in radiation planning                                      |
| **SEG**                  | DICOM segmentation: a pixel mask outlining structures such as organs or tumors                                                  |
| **Series**               | One acquisition, or one set of annotations, within an exam; IDC's main counting unit                                            |
| **SNOMED CT**            | A large, widely used clinical terminology providing codes for anatomy, findings and more                                        |
| **SPDX identifier**      | A short standard identifier for a license, such as `CC-BY-4.0`, that software can recognize                                     |
| **SR**                   | DICOM structured report: measurements, findings or clinical data in coded form                                                  |
| **Study**                | One imaging exam or visit                                                                                                       |
| **Transfer syntax**      | DICOM's term for how the data in a file is encoded and compressed                                                               |
| **UID / UUID**           | Unique identifiers. DICOM UIDs identify studies, series and instances; IDC's UUIDs identify specific versions of series         |
