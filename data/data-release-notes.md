# Data release notes

{% hint style="info" %}
Data hosted by IDC is ingested from several sources, including [The Cancer Imaging Archive (TCIA)](https://www.cancerimagingarchive.net/), [Genomics Data Commons (GDC)](https://gdc.cancer.gov/), [Clinical Proteomic Tumor Analysis Consortium (CPTAC)](https://gdc.cancer.gov/about-gdc/contributed-genomic-data-cancer-research/clinical-proteomic-tumor-analysis-consortium-cptac) and [Human Tumor Atlas Network (HTAN)](https://humantumoratlas.org/).

Please refer to the license and terms of use, which are defined in the `license_url` and `source_doi` or `source_doi` of the IDC BigQuery [`dicom_all` table](https://console.cloud.google.com/bigquery?p=bigquery-public-data\&d=idc\_current\&t=dicom\_all\&page=table). You can filter the data by license type in the [IDC Portal](https://imaging.datacommons.cancer.gov/).
{% endhint %}

## IDC releases summary view

<figure><img src="https://docs.google.com/spreadsheets/d/e/2PACX-1vRXGj6pT16xHtmyqsSSd1sVqbvUThLyDCZkHtpFROX-TEW4-sxdFRar6sr3NPxNPheWcgD_Cf0qmhmx/pubchart?oid=395334291&#x26;format=image" alt=""><figcaption></figcaption></figure>

## v16 - September 2023

New radiology collections

1. [PDMR-Texture-Analysis](https://dev-portal.canceridc.dev/explore/filters/?collection\_id=PDMR\&collection\_id=pdmr\_texture\_analysis)

New pathology collections

1. [RMS-Mutation-Prediction](https://dev-portal.canc=ccdieridc.dev/explore/filters/?collection\_id=CCDI\&collection\_id=rms\_mutation\_prediction)

Revised radiology collections

1. [Breast-MRI-NACT-Pilot](https://dev-portal.canceridc.dev/explore/filters/?collection\_id=Community\&collection\_id=breast\_mri\_nact\_pilot) (TCIA description: (Repair of DICOM tag(0008,0005) to value "ISO\_IR 100" in 79 series)
2. [CPTAC-CRCC](https://dev-portal.canceridc.dev/explore/filters/?collection\_id=CPTAC\&collection\_id=cptac\_crcc) (Revised because results from CPTAC-CRCC-Tumor-Annotations were added)
3. [CPTAC-UCEC](https://dev-portal.canceridc.dev/explore/filters/?collection\_id=CPTAC\&collection\_id=cptac\_ucec) (Revised because results from CPTAC-UCEC-Tumor-Annotations were added)
4. [CPTAC-PDA](https://dev-portal.canceridc.dev/explore/filters/?collection\_id=CPTAC\&collection\_id=cptac\_pda) (Revised because results from CPTAC-PDA-Tumor-Annotations were added)

New analysis results

1. [CPTAC-CRCC-Tumor-Annotations ](https://wiki.cancerimagingarchive.net/pages/viewpage.action?pageId=157288300)
2. [CPTAC-UCEC-Tumor-Annotations](https://wiki.cancerimagingarchive.net/pages/viewpage.action?pageId=157288358)
3. [CPTAC-PDA-Tumor-Annotations](https://wiki.cancerimagingarchive.net/pages/viewpage.action?pageId=157288334)

New clinical metadata tables

1. [htan\_hms\_demographics](https://portal.imaging.datacommons.cancer.gov/explore/filters/?collection\_id=htan\_hms)
2. [htan\_hms\_diagnosis](https://portal.imaging.datacommons.cancer.gov/explore/filters/?collection\_id=htan\_hms)
3. [htan\_hms\_exposure](https://portal.imaging.datacommons.cancer.gov/explore/filters/?collection\_id=htan\_hms)
4. [htan\_hms\_familyhistory](https://portal.imaging.datacommons.cancer.gov/explore/filters/?collection\_id=htan\_hms)
5. [htan\_hms\_followup](https://portal.imaging.datacommons.cancer.gov/explore/filters/?collection\_id=htan\_hms)
6. [htan\_hms\_moleculartheraphy](https://portal.imaging.datacommons.cancer.gov/explore/filters/?collection\_id=htan\_hms)
7. [htan\_oshu\_demographics](https://portal.imaging.datacommons.cancer.gov/explore/filters/?collection\_id=htan\_oshu)
8. [htan\_oshu\_diagnosis](https://portal.imaging.datacommons.cancer.gov/explore/filters/?collection\_id=htan\_oshu)
9. [htan\_oshu\_exposure](https://portal.imaging.datacommons.cancer.gov/explore/filters/?collection\_id=htan\_oshu)
10. [htan\_oshu\_familyhistory](https://portal.imaging.datacommons.cancer.gov/explore/filters/?collection\_id=htan\_oshu)
11. [htan\_oshu\_followup](https://portal.imaging.datacommons.cancer.gov/explore/filters/?collection\_id=htan\_oshu)
12. [htan\_oshu\_moleculartheraphy](https://portal.imaging.datacommons.cancer.gov/explore/filters/?collection\_id=htan\_oshu)
13. [htan\_wustl\_demographics](https://portal.imaging.datacommons.cancer.gov/explore/filters/?collection\_id=htan\_wustl)
14. [htan\_wustl\_diagnosis](https://portal.imaging.datacommons.cancer.gov/explore/filters/?collection\_id=htan\_wustl)
15. [htan\_wustl\_exposure](https://portal.imaging.datacommons.cancer.gov/explore/filters/?collection\_id=htan\_wustl)
16. [htan\_wustl\_familyhistory](https://portal.imaging.datacommons.cancer.gov/explore/filters/?collection\_id=htan\_wustl)
17. [htan\_wustl\_followup](https://portal.imaging.datacommons.cancer.gov/explore/filters/?collection\_id=htan\_wustl)
18. [htan\_wustl\_moleculartheraphy](https://portal.imaging.datacommons.cancer.gov/explore/filters/?collection\_id=htan\_wustl)
19. [rms\_mutation\_prediction\_demographics](https://portal.imaging.datacommons.cancer.gov/explore/filters/?collection\_id=rms\_mutation\_prediction)
20. [rms\_mutation\_prediction\_diagnosis](https://portal.imaging.datacommons.cancer.gov/explore/filters/?collection\_id=rms\_mutation\_prediction)
21. [rms\_mutation\_prediction\_sample](https://portal.imaging.datacommons.cancer.gov/explore/filters/?collection\_id=rms\_mutation\_prediction)


## v15 - July 2023

New radiology collections

1. [Adrenal-ACC-Ki67-Seg](https://portal.imaging.datacommons.cancer.gov/explore/filters/?collection\_id=Community\&collection\_id=adrenal\_acc\_ki67\_seg)
2. [CC-Tumor-Heterogeneity](https://portal.imaging.datacommons.cancer.gov/explore/filters/?collection\_id=Community\&collection\_id=cc\_tumor\_heterogeneity)
3. [Colorectal-Liver-Metastases](https://portal.imaging.datacommons.cancer.gov/explore/filters/?collection\_id=Community\&collection\_id=cc\_tumor\_heterogeneity)
4. [NLM-Visible-Human-Project](https://portal.imaging.datacommons.cancer.gov/explore/filters/?collection\_id=nlm\_visible\_human\_project)
5. [Prostate-Anatomical-Edge-Cases](https://portal.imaging.datacommons.cancer.gov/explore/filters/?collection\_id=Community\&collection\_id=prostate\_anatomical\_edge\_cases)
6. [RIDER Pilot](https://portal.imaging.datacommons.cancer.gov/explore/filters/?collection\_id=RIDER\&collection\_id=rider\_pilot)

New pathology collections

1. [HTAN-VANDERBILT](https://portal.imaging.datacommons.cancer.gov/explore/filters/?collection\_id=HTAN\&collection\_id=htan\_vanderbilt)
2. [ICDC-Glioma](https://portal.imaging.datacommons.cancer.gov/explore/filters/?Modality\_op=OR\&Modality=SM\&collection\_id=icdc\_glioma) (ICDC-Glioma radiology added in a previous version)

Revised radiology collections

1. [CPTAC-CCRCC](https://portal.imaging.datacommons.cancer.gov/explore/filters/?collection\_id=CPTAC\&collection\_id=cptac\_ccrcc) (TCIA description: “Radiology modality data cleanup to remove extraneous scans.”)
2. [CPTAC-CM](https://portal.imaging.datacommons.cancer.gov/explore/filters/?collection\_id=CPTAC\&collection\_id=cptac\_cm) (“TCIA description: Radiology modality data cleanup to remove extraneous scans.”)
3. [CPTAC-LSCC](https://portal.imaging.datacommons.cancer.gov/explore/filters/?collection\_id=CPTAC\&collection\_id=cptac\_lscc) (TCIA description: “Radiology modality data cleanup to remove extraneous scans.”)
4. [CPTAC-LUAD](https://portal.imaging.datacommons.cancer.gov/explore/filters/?collection\_id=CPTAC\&collection\_id=cptac\_luad) (TCIA description: “Radiology modality data cleanup to remove extraneous scans.”)
5. [CPTAC-PDA](https://portal.imaging.datacommons.cancer.gov/explore/filters/?collection\_id=CPTAC\&collection\_id=cptac\_pda) (TCIA description: TCIA description: “Radiology modality data cleanup to remove extraneous scans.”)
6. [CPTAC-SAR](https://portal.imaging.datacommons.cancer.gov/explore/filters/?collection\_id=CPTAC\&collection\_id=cptac\_sar) (TCIA description: “Radiology modality data cleanup to remove extraneous scans.”)
7. [CPTAC-UCEC](https://portal.imaging.datacommons.cancer.gov/explore/filters/?collection\_id=CPTAC\&collection\_id=cptac\_ucec) (TCIA description: “Radiology modality data cleanup to remove extraneous scans.”)
8. [CT Lymph Nodes](https://portal.imaging.datacommons.cancer.gov/explore/filters/?collection\_id=Community\&collection\_id=ct\_lymph\_nodes) (TCIA description: “Added DICOM version of MED\_ABD\_LYMPH\_MASKS.zip segmentations that were previously available”)
9. [RIDER Lung CT](https://portal.imaging.datacommons.cancer.gov/explore/filters/?collection\_id=RIDER\&collection\_id=rider\_lung\_ct) (Revised because QIBA-VolCT-1B analysis results were added)
10. [NLST](https://portal.imaging.datacommons.cancer.gov/explore/filters/?collection\_id=RIDER\&collection\_id=nlst) (Revised because analysis results from nnU-Net-BPR-Annotations were revised)
11. [NSCLC-Radiomics](https://portal.imaging.datacommons.cancer.gov/explore/filters/?collection\_id=Community\&collection\_id=nsclc\_radiomics) (Revised because analysis results from nnU-Net-BPR-Annotations were revised)

Revised pathology collections

1. [CPTAC-GBM](https://portal.imaging.datacommons.cancer.gov/explore/filters/?collection\_id=Community\&collection\_id=cptac\_gbm) (11 pathology-only patients removed at request of data owner)
2. [CPTAC-SAR](https://portal.imaging.datacommons.cancer.gov/explore/filters/?collection\_id=CPTAC\&collection\_id=cptac\_sar) (1 pathology-only patient removed at request of data owner)

New analysis results

1. [QIBA-VolCT-1B](https://portal.imaging.datacommons.cancer.gov/explore/filters/?collection\_id=qiba\_ct\_1c) (Analysis of NLST and NSCLC-Radiomics)

Revised analysis results

1. [nnU-Net-BPR-Annotations](https://portal.imaging.datacommons.cancer.gov/explore/filters/?analysis\_results\_id=nnU-Net-BPR-annotations) (Annotations of NLST and NSCLC-Radiomics radiology)

New clinical metadata tables

1. [adrenal\_acc\_ki67\_seg\_clinical](https://portal.imaging.datacommons.cancer.gov/explore/filters/?collection\_id=Community\&collection\_id=adrenal\_acc\_ki67\_seg)
2. [cc\_tumor\_heterogeneity\_clinical](https://portal.imaging.datacommons.cancer.gov/explore/filters/?collection\_id=Community\&collection\_id=cc\_tumor\_heterogeneity)
3. [colorectal\_liver\_metastases\_clinical](https://portal.imaging.datacommons.cancer.gov/explore/filters/?collection\_id=Community\&collection\_id=colorectal\_liver\_metastases)
4. [duke\_breast\_cancer\_mri\_clinical](https://portal.imaging.datacommons.cancer.gov/explore/filters/?collection\_id=Community\&collection\_id=duke\_breast\_cancer\_mri)
5. [nlst\_clinical](https://portal.imaging.datacommons.cancer.gov/explore/filters/?collection\_id=NCI\_Trials\&collection\_id=nlst)
6. [nlst\_ctab](https://portal.imaging.datacommons.cancer.gov/explore/filters/?collection\_id=NCI\_Trials\&collection\_id=nlst)
7. [nlst\_ctabc](https://portal.imaging.datacommons.cancer.gov/explore/filters/?collection\_id=NCI\_Trials\&collection\_id=nlst)
8. [nlst\_prsn](https://portal.imaging.datacommons.cancer.gov/explore/filters/?collection\_id=NCI\_Trials\&collection\_id=nlst)
9. [nlst\_screen](https://portal.imaging.datacommons.cancer.gov/explore/filters/?collection\_id=NCI\_Trials\&collection\_id=nlst)

## v14 - May 2023

This release does not introduce any new data, but changes the bucket organization and introduces replication of IDC files in Amazon AWS storage buckets, as described in [this section](organization-of-data/files-and-metadata.md).

## v13 - Mar 2023

New analysis results collection:

1. [nnU-Net-BPR-annotations](https://portal.imaging.datacommons.cancer.gov/explore/filters/?analysis\_results\_id=nnU-Net-BPR-annotations)

New clinical data collections:

1. [PROSTATEx](https://portal.imaging.datacommons.cancer.gov/explore/filters/?collection\_id=prostatex)

## v12 - Nov 2022

New collections:

1. [CT-vs-PET-Ventilation-Imaging](https://portal.imaging.datacommons.cancer.gov/explore/filters/?collection\_id=ct\_vs\_pet\_ventilation\_imaging)
2. [CTpred-Sunitinib-panNET](https://portal.imaging.datacommons.cancer.gov/explore/filters/?collection\_id=ctpred\_sunitinib\_pannet)

Updated collections:

1. [CMB-CRC](https://portal.imaging.datacommons.cancer.gov/explore/filters/?access=Public\&collection\_id=CMB\&collection\_id=cmb\_crc)
2. [CMB-LCA](https://portal.imaging.datacommons.cancer.gov/explore/filters/?access=Public\&collection\_id=CMB\&collection\_id=cmb\_lca)
3. [CMB-MEL](https://portal.imaging.datacommons.cancer.gov/explore/filters/?access=Public\&collection\_id=CMB\&collection\_id=cmb\_mel)
4. [CMB-PCA](https://portal.imaging.datacommons.cancer.gov/explore/filters/?access=Public\&collection\_id=CMB\&collection\_id=cmb\_pca)
5. [Pancreatic-CT-CBCT-SEG](https://portal.imaging.datacommons.cancer.gov/explore/filters/?collection\_id=pancreatic\_ct\_cbct\_seg)

Other:

Metadata corresponding to "limited" access collections are removed.

New clinical data collections:

1. [CTpred-Sunitinib-panNET](https://portal.imaging.datacommons.cancer.gov/explore/filters/?collection\_id=ctpred\_sunitinib\_pannet)

Other clinical data updates:

Limited access collections are removed. Clinical metadata for the COVID-19-NY-SUB and ACRIN 6698/I-SPY2 Breast DWI collections now includes information ingested from data dictionaries associated with these collections. In v11 the string value 'NA' was being changed to null during the ETL process for some columns/collections. This is now fixed in v12 and the value 'NA' is preserved.

## v11 - Sept 2022

This release introduces clinical data ingested for a subset of collections, and now available via a dedicated BigQuery dataset.

New collections:

1. [CMB-CRC](https://portal.imaging.datacommons.cancer.gov/explore/filters/?access=Public\&collection\_id=CMB\&collection\_id=cmb\_crc)
2. [CMB-GEC](https://portal.imaging.datacommons.cancer.gov/explore/filters/?access=Public\&collection\_id=CMB\&collection\_id=cmb\_gec)
3. [CMB-LCA](https://portal.imaging.datacommons.cancer.gov/explore/filters/?access=Public\&collection\_id=CMB\&collection\_id=cmb\_lca)
4. [CMB-MEL](https://portal.imaging.datacommons.cancer.gov/explore/filters/?access=Public\&collection\_id=CMB\&collection\_id=cmb\_mel)
5. [CMB-MML](https://portal.imaging.datacommons.cancer.gov/explore/filters/?access=Public\&collection\_id=CMB\&collection\_id=cmb\_mml)
6. [CMB-PCA](https://portal.imaging.datacommons.cancer.gov/explore/filters/?access=Public\&collection\_id=CMB\&collection\_id=cmb\_pca)
7. [GBM-DSC-MRI-DRO](https://portal.imaging.datacommons.cancer.gov/explore/filters/?access=Public\&collection\_id=QIN\&collection\_id=gbm\_dsc\_mri\_dro)
8. [HCC-TACE-Seg](https://portal.imaging.datacommons.cancer.gov/explore/filters/?access=Public\&collection\_id=Community\&collection\_id=hcc\_tace\_seg)
9. [PDMR-521955-158-R4](https://portal.imaging.datacommons.cancer.gov/explore/filters/?access=Public\&collection\_id=PDMR\&collection\_id=pdmr\_521955\_158\_r4)

## v10 - Aug 2022

In this release we introduce a new HTAN program including currently three collections release by the [Human Tumor Atlas Network](https://humantumoratlas.org/).

New collections:

1. [ACRIN-6698](https://dx.doi.org/10.7937/tcia.kk02-6d95)
2. [HTAN-HMS](https://humantumoratlas.org/hta7/)
3. [HTAN-OHSU](https://humantumoratlas.org/hta9/)
4. [HTAN-WUSTL](https://humantumoratlas.org/hta12/)
5. [ISPY2](https://dx.doi.org/10.7937/TCIA.D8Z0-9T85)
6. [UPENN-GBM](https://dx.doi.org/10.7937/TCIA.709X-DN49)

Updated collections:

CPTAC, TCGA and NLST collections have been reconverted due to a technical issue identified with a subset of images included in v9.

1. [CPTAC-AML](https://dx.doi.org/10.7937/tcia.2019.b6foe619)
2. [CPTAC-BRCA](https://dx.doi.org/10.7937/TCIA.CAEM-YS80) \*
3. [CPTAC-CCRCC](https://dx.doi.org/10.7937/K9/TCIA.2018.OBLAMN27)
4. [CPTAC-CM](https://dx.doi.org/10.7937/K9/TCIA.2018.ODU24GZE)
5. [CPTAC-COAD](https://dx.doi.org/10.7937/TCIA.YZWQ-ZZ63)
6. [CPTAC-GBM](https://dx.doi.org/10.7937/K9/TCIA.2018.3RJE41Q1)
7. [CPTAC-HNSCC](https://dx.doi.org/10.7937/K9/TCIA.2018.UW45NH81)
8. [CPTAC-LSCC](https://dx.doi.org/10.7937/K9/TCIA.2018.6EMUB5L2)
9. [CPTAC-LUAD](https://dx.doi.org/10.7937/K9/TCIA.2018.PAT12TBS)
10. [CPTAC-OV](https://dx.doi.org/10.7937/TCIA.ZS4A-JD58)
11. [CPTAC-PDA](https://dx.doi.org/10.7937/K9/TCIA.2018.SC20FO18)
12. [CPTAC-SAR](https://dx.doi.org/10.7937/TCIA.2019.9bt23r95)
13. [CPTAC-UCEC](https://dx.doi.org/10.7937/K9/TCIA.2018.3R3JUISW)
14. [Duke-Breast-Cancer-MRI](https://dx.doi.org/10.7937/TCIA.e3sv-re93)
15. [NLST](https://dx.doi.org/10.7937/TCIA.hmq8-j677)
16. [TCGA-ACC](https://www.cancer.gov/about-nci/organization/ccg/research/structural-genomics/tcga/studied-cancers/adrenocortical)
17. [TCGA-BLCA](https://dx.doi.org/10.7937/K9/TCIA.2016.8LNG8XDR)
18. [TCGA-BRCA](https://dx.doi.org/10.7937/K9/TCIA.2016.AB2NAZRP)
19. [TCGA-BRCA](https://dx.doi.org/10.7937/TCIA.2019.wgllssg1)
20. [TCGA-CESC](https://dx.doi.org/10.7937/K9/TCIA.2016.SQ4M8YP4)
21. [TCGA-CHOL](https://www.cancer.gov/about-nci/organization/ccg/research/structural-genomics/tcga/studied-cancers/cholangiocarcinoma)
22. [TCGA-COAD](https://dx.doi.org/10.7937/K9/TCIA.2016.HJJHBOXZ)
23. TCGA-DLBC
24. [TCGA-ESCA](https://dx.doi.org/10.7937/K9/TCIA.2016.VPTNRGFY)
25. [TCGA-GBM](https://dx.doi.org/10.7937/K9/TCIA.2016.RNYFUYE9)
26. [TCGA-GBM](https://dx.doi.org/10.7937/TCIA.2018.ow6ce3ml)
27. [TCGA-HNSC](https://dx.doi.org/10.7937/K9/TCIA.2016.LXKQ47MS)
28. [TCGA-KICH](https://dx.doi.org/10.7937/K9/TCIA.2016.YU3RBCZN)
29. [TCGA-KIRC](https://dx.doi.org/10.7937/K9/TCIA.2016.V6PBVTDR)
30. [TCGA-KIRP](https://dx.doi.org/10.7937/K9/TCIA.2016.ACWOGBEF) \*
31. [TCGA-LGG](https://dx.doi.org/10.7937/K9/TCIA.2016.L4LTD3TK)
32. [TCGA-LGG](https://dx.doi.org/10.7937/TCIA.2018.ow6ce3ml)
33. [TCGA-LIHC](https://dx.doi.org/10.7937/K9/TCIA.2016.IMMQW8UQ)
34. [TCGA-LUAD](https://dx.doi.org/10.7937/K9/TCIA.2016.JGNIHEP5)
35. [TCGA-LUSC](https://dx.doi.org/10.7937/K9/TCIA.2016.TYGKKFMQ)
36. [TCGA-MESO](https://www.cancer.gov/about-nci/organization/ccg/research/structural-genomics/tcga/studied-cancers/mesothelioma)
37. [TCGA-OV](https://dx.doi.org/10.7937/K9/TCIA.2016.NDO1MDFQ)
38. [TCGA-PAAD](https://www.cancer.gov/about-nci/organization/ccg/research/structural-genomics/tcga/studied-cancers/pancreatic)
39. [TCGA-PCPG](https://www.cancer.gov/about-nci/organization/ccg/research/structural-genomics/tcga/studied-cancers/paraganglioma)
40. [TCGA-PRAD](https://dx.doi.org/10.7937/K9/TCIA.2016.YXOGLM4Y)
41. [TCGA-READ](https://dx.doi.org/10.7937/K9/TCIA.2016.F7PPNPNU)
42. [TCGA-SARC](https://dx.doi.org/10.7937/K9/TCIA.2016.CX6YLSUX)
43. [TCGA-SKCM](https://www.cancer.gov/about-nci/organization/ccg/research/structural-genomics/tcga/studied-cancers/melanoma-skin)
44. [TCGA-STAD](https://dx.doi.org/10.7937/K9/TCIA.2016.GDHL9KIM)
45. [TCGA-TGCT](https://www.cancer.gov/about-nci/organization/ccg/research/structural-genomics/tcga/studied-cancers/testicular-germ-cell)
46. [TCGA-THCA](https://dx.doi.org/10.7937/K9/TCIA.2016.9ZFRVF1B)
47. [TCGA-THYM](https://www.cancer.gov/about-nci/organization/ccg/research/structural-genomics/tcga/studied-cancers/thymoma)
48. [TCGA-UCEC](https://dx.doi.org/10.7937/K9/TCIA.2016.GKJ0ZWAC)
49. [TCGA-UCS](https://www.cancer.gov/about-nci/organization/ccg/research/structural-genomics/tcga/studied-cancers/uterine-carcinosarcoma)
50. [TCGA-UVM](https://www.cancer.gov/about-nci/organization/ccg/research/structural-genomics/tcga/studied-cancers/melanoma-eye)

Note that the TCGA-KIRP and TCGA-BRCA collections (marked with the asterisk in the list above) are currently missing SM high resolution layer files/instances due to a [known limitation](https://issuetracker.google.com/issues/238920874) of Google Healthcare that makes it not possible to ingest datasets that exceed some internal limits. Specifically, the following patient/studies are affected:

* TCGA-KIRP: `PatientID` TCGA-5P-A9KA, `StudyInstanceUID` 2.25.191236165605958868867890945341011875563
* TCGA-BRCA: `PatientID` TCGA-OL-A66H, `StudyInstanceUID` 2.25.82800314486527687800038836287574075736 The affected files will be included in IDC when the infrastructure limitation is addressed.

Collection access level change:

1. [Vestibular-Schwannoma-SEG](https://doi.org/10.7937/TCIA.9YTJ-5Q73) is now available as public access collection

## v9 - May 2022

This data release introduces the concept of differential license to IDC: some of the collections maintained by IDC contain items that have different licenses. As an example, radiology component of the TCGA-GBM collection is covered by the TCIA limited access license, and is not available in IDC, while the digital pathology component is covered by CC-BY. With this release, we complete sharing in full of the digital pathology component of the datasets released by the CPTAC and TCGA programs.

New collections:

1. [ACRIN-Contralateral-Breast-MR](https://dx.doi.org/10.7937/Q1EE-J082)
2. [StageII-Colorectal-CT](https://dx.doi.org/10.7937/p5k5-tg43)

Updated collections:

1. [B-mode-and-CEUS-Liver](https://dx.doi.org/10.7937/TCIA.2021.v4z7-tc39)
2. [CPTAC-GBM](https://dx.doi.org/10.7937/K9/TCIA.2018.3RJE41Q1)
3. [CPTAC-HNSCC](https://dx.doi.org/10.7937/K9/TCIA.2018.UW45NH81)
4. [Pediatric-CT-SEG](https://dx.doi.org/10.7937/TCIA.X0H0-1706)
5. [TCGA-GBM](https://dx.doi.org/10.7937/K9/TCIA.2016.RNYFUYE9)
6. [TCGA-HNSC](https://dx.doi.org/10.7937/K9/TCIA.2016.LXKQ47MS)
7. [TCGA-LGG](https://dx.doi.org/10.7937/K9/TCIA.2016.L4LTD3TK)

## v8 - April 2022

The main highlight of this release is the addition of the NLST and TCGA Slide Microscopy imaging data. New TCGA content includes introduction of new (to IDC) TCGA collections that have only slide microscopy component, and addition of the slide microscopy component to those IDC collections that were available earlier and included only the radiology component.

New collections

1. [TCGA-ACC](https://www.cancer.gov/about-nci/organization/ccg/research/structural-genomics/tcga/studied-cancers/adrenocortical)
2. [TCGA-CHOL](https://www.cancer.gov/about-nci/organization/ccg/research/structural-genomics/tcga/studied-cancers/cholangiocarcinoma)
3. TCGA-DLBC (TCGA-DLBC collection does not have a description page)
4. [TCGA-MESO](https://www.cancer.gov/about-nci/organization/ccg/research/structural-genomics/tcga/studied-cancers/mesothelioma)
5. [TCGA-PAAD](https://www.cancer.gov/about-nci/organization/ccg/research/structural-genomics/tcga/studied-cancers/pancreatic)
6. [TCGA-PCPG](https://www.cancer.gov/about-nci/organization/ccg/research/structural-genomics/tcga/studied-cancers/paraganglioma)
7. [TCGA-SKCM](https://www.cancer.gov/about-nci/organization/ccg/research/structural-genomics/tcga/studied-cancers/melanoma-skin)
8. [TCGA-TGCT](https://www.cancer.gov/about-nci/organization/ccg/research/structural-genomics/tcga/studied-cancers/testicular-germ-cell)
9. [TCGA-THYM](https://www.cancer.gov/about-nci/organization/ccg/research/structural-genomics/tcga/studied-cancers/thymoma)
10. [TCGA-UCS](https://www.cancer.gov/about-nci/organization/ccg/research/structural-genomics/tcga/studied-cancers/uterine-carcinosarcoma)
11. [TCGA-UVM](https://www.cancer.gov/about-nci/organization/ccg/research/structural-genomics/tcga/studied-cancers/melanoma-eye)

Updated collections

1. [NLST](https://dx.doi.org/10.7937/TCIA.hmq8-j677)
2. [TCGA-BLCA](https://dx.doi.org/10.7937/K9/TCIA.2016.8LNG8XDR)
3. [TCGA-BRCA](https://dx.doi.org/10.7937/K9/TCIA.2016.AB2NAZRP)
4. [TCGA-BRCA](https://dx.doi.org/10.7937/TCIA.2019.wgllssg1)
5. [TCGA-CESC](https://dx.doi.org/10.7937/K9/TCIA.2016.SQ4M8YP4)
6. [TCGA-COAD](https://dx.doi.org/10.7937/K9/TCIA.2016.HJJHBOXZ)
7. [TCGA-ESCA](https://dx.doi.org/10.7937/K9/TCIA.2016.VPTNRGFY)
8. [TCGA-KICH](https://dx.doi.org/10.7937/K9/TCIA.2016.YU3RBCZN)
9. [TCGA-KIRC](https://dx.doi.org/10.7937/K9/TCIA.2016.V6PBVTDR)
10. [TCGA-KIRP](https://dx.doi.org/10.7937/K9/TCIA.2016.ACWOGBEF)
11. [TCGA-LIHC](https://dx.doi.org/10.7937/K9/TCIA.2016.IMMQW8UQ)
12. [TCGA-LUAD](https://dx.doi.org/10.7937/K9/TCIA.2016.JGNIHEP5)
13. [TCGA-LUSC](https://dx.doi.org/10.7937/K9/TCIA.2016.TYGKKFMQ)
14. [TCGA-OV](https://dx.doi.org/10.7937/K9/TCIA.2016.NDO1MDFQ)
15. [TCGA-PRAD](https://dx.doi.org/10.7937/K9/TCIA.2016.YXOGLM4Y)
16. [TCGA-READ](https://dx.doi.org/10.7937/K9/TCIA.2016.F7PPNPNU)
17. [TCGA-SARC](https://dx.doi.org/10.7937/K9/TCIA.2016.CX6YLSUX)
18. [TCGA-STAD](https://dx.doi.org/10.7937/K9/TCIA.2016.GDHL9KIM)
19. [TCGA-THCA](https://dx.doi.org/10.7937/K9/TCIA.2016.9ZFRVF1B)
20. [TCGA-UCEC](https://dx.doi.org/10.7937/K9/TCIA.2016.GKJ0ZWAC)

## v7 - February 2022

The main highlight of this release is the addition of the Slide Microscopy imaging component to the remaining CPTAC collections.

New collections

1. [APOLLO-5-ESCA](https://dx.doi.org/10.7937/n69a-7a26)
2. [APOLLO-5-LUAD](https://dx.doi.org/10.7937/BDM9-4623)
3. [APOLLO-5-PAAD](https://dx.doi.org/10.7937/tcia.1yeg-5740)
4. [APOLLO-5-THYM](https://dx.doi.org/10.7937/tcia.0pg4-nh82)
5. [CPTAC-AML](https://dx.doi.org/10.7937/tcia.2019.b6foe619)
6. [CPTAC-BRCA](https://dx.doi.org/10.7937/TCIA.CAEM-YS80)
7. [CPTAC-COAD](https://dx.doi.org/10.7937/TCIA.YZWQ-ZZ63)
8. [CPTAC-OV](https://dx.doi.org/10.7937/TCIA.ZS4A-JD58)
9. [Pancreatic-CT-CBCT-SEG](https://dx.doi.org/10.7937/TCIA.ESHQ-4D90)
10. [Pediatric-CT-SEG](https://dx.doi.org/10.7937/TCIA.X0H0-1706)

Updated collections

1. [CPTAC-CCRCC](https://dx.doi.org/10.7937/K9/TCIA.2018.OBLAMN27)
2. [CPTAC-CM](https://dx.doi.org/10.7937/K9/TCIA.2018.ODU24GZE)
3. [CPTAC-LSCC](https://dx.doi.org/10.7937/K9/TCIA.2018.6EMUB5L2)
4. [CPTAC-LUAD](https://dx.doi.org/10.7937/K9/TCIA.2018.PAT12TBS)
5. [CPTAC-PDA](https://dx.doi.org/10.7937/K9/TCIA.2018.SC20FO18)
6. [CPTAC-SAR](https://dx.doi.org/10.7937/TCIA.2019.9bt23r95)
7. [CPTAC-UCEC](https://dx.doi.org/10.7937/K9/TCIA.2018.3R3JUISW)

## v6 - January 2022

The following collections became limited access due to the [change in policy by TCIA](https://discourse.canceridc.dev/t/tcia-collections-of-brain-head-or-head-neck-cancers-are-changing-to-limited-access/241), which is the original source of those collections.

Original collections:

1. [AAPM-RT-MAC](https://doi.org/10.7937/tcia.2019.bcfjqfqb)
2. [ACRIN-DSC-MR-Brain](https://doi.org/10.7937/tcia.2019.zr1pjf4i)
3. [ACRIN-FMISO-Brain](https://doi.org/10.7937/K9/TCIA.2018.vohlekok)
4. [ACRIN-HNSCC-FDG-PET-CT](https://doi.org/10.7937/K9/TCIA.2016.JQEJZZNG)
5. [Anti-PD-1\_MELANOMA](https://doi.org/10.7937/tcia.2019.1ae0qtcu)
6. [Brain-Tumor-Progression](https://doi.org/10.7937/K9/TCIA.2018.15quzvnb)
7. [CPTAC-GBM](https://doi.org/10.7937/K9/TCIA.2018.3RJE41Q1)
8. [CPTAC-HNSCC](https://doi.org/10.7937/K9/TCIA.2018.UW45NH81)
9. [HEAD-NECK-RADIOMICS-HN1](https://doi.org/10.7937/tcia.2019.8kap372n)
10. [HNSCC](https://doi.org/10.7937/k9/tcia.2020.a8sh-7363)
11. [HNSCC-3DCT-RT](https://doi.org/10.7937/K9/TCIA.2018.13upr2xf)
12. [Head-Neck Cetuximab](https://doi.org/10.7937/K9/TCIA.2015.7AKGJUPZ)
13. [Head-Neck-PET-CT](https://doi.org/10.7937/K9/TCIA.2017.8oje5q00)
14. [IvyGAP](https://doi.org/10.7937/K9/TCIA.2016.XLwaN6nL)
15. [LGG-1p19qDeletion](https://doi.org/10.7937/K9/TCIA.2017.dwehtz9v)
16. [MRI-DIR](https://doi.org/10.7937/K9/TCIA.2018.3f08iejt)
17. [OPC-Radiomics](https://doi.org/10.7937/tcia.2019.8dho2gls)
18. [QIN GBM Treatment Response](https://doi.org/10.7937/K9/TCIA.2016.nQF4gpn2)
19. [QIN-BRAIN-DSC-MRI](https://doi.org/10.7937/K9/TCIA.2016.5DI84Js8)
20. [QIN-HEADNECK](https://doi.org/10.7937/K9/TCIA.2015.K0F5CGLI)
21. [REMBRANDT](https://doi.org/10.7937/K9/TCIA.2015.588OZUZB)
22. [RIDER NEURO MRI](https://doi.org/10.7937/K9/TCIA.2015.VOSN3HN1)
23. [TCGA-GBM](https://doi.org/10.7937/K9/TCIA.2016.RNYFUYE9)
24. [TCGA-HNSC](https://doi.org/10.7937/K9/TCIA.2016.LXKQ47MS)
25. [TCGA-LGG](https://doi.org/10.7937/K9/TCIA.2016.L4LTD3TK)
26. [Vestibular-Schwannoma-SEG](https://doi.org/10.7937/TCIA.9YTJ-5Q73)

Analysis results collections:

1. [DICOM-SEG Conversions for TCGA-LGG and TCGA-GBM Segmentation Datasets](https://doi.org/10.7937/TCIA.2018.ow6ce3ml)
2. [Outcome Prediction in Patients with Glioblastoma by Using Imaging, Clinical, and Genomic Biomarkers: Focus on the Nonenhancing Component of the Tumor](https://doi.org/10.7937/K9/TCIA.2014.FAB7YRPZ)

## v5 - December 2021

New collections:

* [COVID-19-NY-SBU](https://dx.doi.org/10.7937/TCIA.BBAG-2923)
* [B-mode-and-CEUS-Liver](https://dx.doi.org/10.7937/TCIA.2021.v4z7-tc39)
* [APOLLO-5-LSCC](https://dx.doi.org/10.7937/TCIA.QQ0G-EB24)
* [CMMD](https://dx.doi.org/10.7937/tcia.eqde-4b16)
* [ACRIN-HNSCC-FDG-PET-CT](https://dx.doi.org/10.7937/K9/TCIA.2016.JQEJZZNG)
* [Duke-Breast-Cancer-MRI](https://dx.doi.org/10.7937/TCIA.e3sv-re93)

New analysis results collections:

* Outcome Prediction in Patients with Glioblastoma by Using Imaging, Clinical, and Genomic Biomarkers: Focus on the Nonenhancing Component of the Tumor ([GBM-MR-NER-Outcomes](https://dx.doi.org/10.7937/K9/TCIA.2014.FAB7YRPZ))
* DICOM-SEG Conversions for TCGA-LGG and TCGA-GBM Segmentation Datasets ([DICOM-Glioma-SEG](https://dx.doi.org/10.7937/TCIA.2018.ow6ce3ml))

Updated collections:

* [TCGA-GBM](https://dx.doi.org/10.7937/K9/TCIA.2014.FAB7YRPZ)
* [TCGA-LGG](https://dx.doi.org/10.7937/TCIA.2018.ow6ce3ml)
* [QIN-HEADNECK](https://dx.doi.org/10.7937/K9/TCIA.2015.K0F5CGLI)
* [Breast-Cancer-Screening-DBT](https://dx.doi.org/10.7937/e4wt-cd02)
* [NSCLC Radiogenomics](https://dx.doi.org/10.7937/K9/TCIA.2017.7hs46erv)
* [QIN-HEADNECK](https://dx.doi.org/10.7937/K9/TCIA.2015.K0F5CGLI)
* [Pseudo-PHI-DICOM-Data](https://dx.doi.org/10.7937/s17z-r072)

## v4 - September 2021

[National Lung Screening Trial (NLST) collection](https://doi.org/10.7937/tcia.hmq8-j677) is added. The data included consists of the following components:

1\) CT images available as any other imaging collection (via IDC Portal, BigQuery metadata tables, and storage buckets);

2\) a subset of clinical data available in the BigQuery tables starting with `nlst_` under the `idc_v4` dataset, as documented in the [Collection-specific BigQuery Tables](organization-of-data/files-and-metadata.md) section.

3\) One instance is missing from patient/study/series:\
`126153/1.2.840.113654.2.55.319335498043274792486636919135185299851/1.2.840.113654.2.55.262421043240525317038356381369289737801`

4\) Three instances are missing from patient/study/series:\
`215303/1.3.6.1.4.1.14519.5.2.1.7009.9004.337968382369511017896638591276/1.3.6.1.4.1.14519.5.2.1.7009.9004.180224303090109944523368212991`

## v3 - August 2021

The following radiology collections were updated to include DICOM Slide Microscopy (SM) images converted from the original vendor-specific representation into [dual personality DICOM-TIFF format](../dicom/dicom-tiff-dual-personality-files.md).

1. [CPTAC-LUAD](https://doi.org/10.7937/K9/TCIA.2018.PAT12TBS)
2. [CPTAC-LSCC](https://doi.org/10.7937/K9/TCIA.2018.6EMUB5L2)

{% hint style="warning" %}
The DICOM Slide Microscopy (SM) images included in the collections above in IDC are not available in TCIA. TCIA only includes images in the vendor-specific SVS format!
{% endhint %}

## v2 - June 2021

Listed below are all of the [original](https://www.cancerimagingarchive.net/collections/) and [analysis results](https://www.cancerimagingarchive.net/tcia-analysis-results/) collections of [The Cancer Imaging Archive](https://www.cancerimagingarchive.net) currently hosted by IDC, with the links to the Digital Object Identifiers (DOIs) of those collections.

New original collections:

1. [IvyGAP](https://doi.org/10.7937/K9/TCIA.2016.XLwaN6nL)
2. [QIN LUNG CT](https://doi.org/10.7937/K9/TCIA.2015.NPGZYZBZ)
3. [LungCT-Diagnosis](https://doi.org/10.7937/K9/TCIA.2015.A6V7JIWX)
4. [HEAD-NECK-RADIOMICS-HN1](https://doi.org/10.7937/tcia.2019.8kap372n)
5. [Prostate Fused-MRI-Pathology](https://doi.org/10.7937/K9/TCIA.2016.TLPMR1AM)
6. [APOLLO](https://wiki.cancerimagingarchive.net/x/N4NyAQ)
7. [LGG-1p19qDeletion](https://doi.org/10.7937/K9/TCIA.2017.dwehtz9v)
8. [Soft-tissue-Sarcoma](https://doi.org/10.7937/K9/TCIA.2015.7GO2GSKS)
9. [NSCLC-Radiomics-Genomics](https://doi.org/10.7937/K9/TCIA.2015.L4FRET6Z)
10. [Brain-Tumor-Progression](https://doi.org/10.7937/K9/TCIA.2018.15quzvnb)
11. [Head-Neck Cetuximab](https://doi.org/10.7937/K9/TCIA.2015.7AKGJUPZ)
12. [CPTAC-GBM](https://doi.org/10.7937/K9/TCIA.2018.3RJE41Q1)
13. [CPTAC-SAR](https://doi.org/10.7937/TCIA.2019.9bt23r95)
14. [CPTAC-LUAD](https://doi.org/10.7937/K9/TCIA.2018.PAT12TBS)
15. [CPTAC-LSCC](https://doi.org/10.7937/K9/TCIA.2018.6EMUB5L2)
16. [Head-Neck-PET-CT](https://doi.org/10.7937/K9/TCIA.2017.8oje5q00)
17. [C4KC-KiTS](https://doi.org/10.7937/TCIA.2019.IX49E8NX)
18. [Breast-MRI-NACT-Pilot](https://doi.org/10.7937/K9/TCIA.2016.QHSYHJKY)
19. [4D-Lung](https://doi.org/10.7937/K9/TCIA.2016.ELN8YGLE)
20. [Mouse-Mammary](https://doi.org/10.7937/K9/TCIA.2015.9P42KSE6)
21. [CT Lymph Nodes](https://doi.org/10.7937/K9/TCIA.2015.AQIIDCNM)
22. [HNSCC](https://doi.org/10.7937/k9/tcia.2020.a8sh-7363)
23. [Breast-Cancer-Screening-DBT](https://doi.org/10.7937/e4wt-cd02)
24. [MRI-DIR](https://doi.org/10.7937/K9/TCIA.2018.3f08iejt)
25. [Lung-PET-CT-Dx](https://doi.org/10.7937/TCIA.2020.NNC2-0461)
26. [NSCLC-RADIOMICS-INTEROBSERVER1](https://doi.org/10.7937/tcia.2019.cwvlpd26)
27. [PDMR-BL0293-F563](https://doi.org/10.7937/tcia.2019.b6u7wmqw)
28. [CT COLONOGRAPHY](https://doi.org/10.7937/K9/TCIA.2015.NWTESAY1)
29. [Phantom FDA](https://doi.org/10.7937/K9/TCIA.2015.ORBJKMUX)
30. [QIN-PROSTATE-Repeatability](https://doi.org/10.7937/K9/TCIA.2018.MR1CKGND)
31. [PROSTATEx](https://doi.org/10.7937/K9TCIA.2017.MURS5CL)
32. [AAPM-RT-MAC](https://doi.org/10.7937/tcia.2019.bcfjqfqb)
33. [ICDC-Glioma](https://doi.org/10.7937/TCIA.SVQT-Q016)
34. [RIDER Breast MRI](https://doi.org/10.7937/K9/TCIA.2015.H1SXNUXL)
35. [Anti-PD-1\_MELANOMA](https://doi.org/10.7937/tcia.2019.1ae0qtcu)
36. [COVID-19-AR](https://doi.org/10.7937/tcia.2020.py71-5978)
37. [PROSTATE-MRI](https://doi.org/10.7937/K9/TCIA.2016.6046GUDv)
38. [NaF PROSTATE](https://doi.org/10.7937/K9/TCIA.2015.ISOQTHKO)
39. [Mouse-Astrocytoma](https://doi.org/10.7937/K9TCIA.2017.SGW7CAQW)
40. [ACRIN-DSC-MR-Brain](https://doi.org/10.7937/tcia.2019.zr1pjf4i)
41. [ACRIN-NSCLC-FDG-PET](https://doi.org/10.7937/tcia.2019.30ilqfcl)
42. [QIN Breast DCE-MRI](https://doi.org/10.7937/K9/TCIA.2014.A2N1IXOX)
43. [RIDER NEURO MRI](https://doi.org/10.7937/K9/TCIA.2015.VOSN3HN1)
44. [MIDRC-RICORD-1A](https://doi.org/10.7937/VTW4-X588)
45. [MIDRC-RICORD-1C](https://doi.org/10.7937/91ah-v663)
46. [REMBRANDT](https://doi.org/10.7937/K9/TCIA.2015.588OZUZB)
47. [NSCLC Radiogenomics](https://doi.org/10.7937/K9/TCIA.2017.7hs46erv)
48. [HNSCC-3DCT-RT](https://doi.org/10.7937/K9/TCIA.2018.13upr2xf)
49. [VICTRE](https://doi.org/10.7937/TCIA.2019.ho23nxaw)
50. [CPTAC-CM](https://doi.org/10.7937/K9/TCIA.2018.ODU24GZE)
51. [CPTAC-PDA](https://doi.org/10.7937/K9/TCIA.2018.SC20FO18)
52. [CPTAC-UCEC](https://doi.org/10.7937/K9/TCIA.2018.3R3JUISW)
53. [CPTAC-CCRCC](https://doi.org/10.7937/K9/TCIA.2018.OBLAMN27)
54. [CPTAC-HNSCC](https://doi.org/10.7937/K9/TCIA.2018.UW45NH81)
55. [OPC-Radiomics](https://doi.org/10.7937/tcia.2019.8dho2gls)
56. [Vestibular-Schwannoma-SEG](https://doi.org/10.7937/TCIA.9YTJ-5Q73)
57. [SPIE-AAPM Lung CT Challenge](https://doi.org/10.7937/K9/TCIA.2015.UZLSU3FL)
58. [Lung Phantom](https://doi.org/10.7937/K9/TCIA.2015.08A1IXOO)
59. [Pseudo-PHI-DICOM-Data](https://doi.org/10.7937/s17z-r072)
60. [Pancreas-CT](https://doi.org/10.7937/K9/TCIA.2016.tNB1kqBU)
61. [QIN GBM Treatment Response](https://doi.org/10.7937/K9/TCIA.2016.nQF4gpn2)
62. [Pelvic-Reference-Data](https://doi.org/10.7937/TCIA.2019.woskq5oo)
63. [Lung-Fused-CT-Pathology](https://doi.org/10.7937/K9/TCIA.2018.SMT36LPN)
64. [Anti-PD-1\_Lung](https://doi.org/10.7937/tcia.2019.zjjwb9ip)
65. [BREAST-DIAGNOSIS](https://doi.org/10.7937/K9/TCIA.2015.SDNRQXXR)
66. [RIDER Lung PET-CT](https://doi.org/10.7937/K9/TCIA.2015.OFIP7TVM)
67. [RIDER Lung CT](https://doi.org/10.7937/K9/TCIA.2015.U1X8A5NR)
68. [PDMR-292921-168-R](https://doi.org/10.7937/TCIA.2020.PCAK-8Z10)
69. [PDMR-833975-119-R](https://doi.org/10.7937/TCIA.0ECK-C338)
70. [PDMR-997537-175-T](https://doi.org/10.7937/TCIA.2020.BRY9-4N29)
71. [LCTSC](https://doi.org/10.7937/K9/TCIA.2017.3r3fvz08)
72. [Prostate-3T](https://doi.org/10.7937/K9/TCIA.2015.QJTV5IL5)
73. [ACRIN-FLT-Breast](https://doi.org/10.7937/K9/TCIA.2017.ol20zmxg)
74. [ACRIN-FMISO-Brain](https://doi.org/10.7937/K9/TCIA.2018.vohlekok)
75. [PDMR-425362-245-T](https://doi.org/10.7937/TCIA.2020.7YRS-7J97)
76. [Prostate-MRI-US-Biopsy](https://doi.org/10.7937/TCIA.2020.A61IOC1A)
77. [MIDRC-RICORD-1B](https://doi.org/10.7937/31V8-4A40)
78. [DRO-Toolkit](https://doi.org/10.7937/t062-8262)

New analysis results collections:

1. [PROSTATEx Zone Segmentations](https://doi.org/10.7937/tcia.nbb4-4655)
2. [High Resolution Prostate Segmentations for the ProstateX-Challenge](https://doi.org/10.7937/TCIA.2019.DEG7ZG1U)
3. [RIDER Lung CT Segmentation Labels from: Decoding tumour phenotype by noninvasive imaging using a quantitative radiomics approach](https://doi.org/10.7937/tcia.2020.jit9grk8)

## v1 - October 2020

Listed below are all of the [original](https://www.cancerimagingarchive.net/collections/) and [analysis results](https://www.cancerimagingarchive.net/tcia-analysis-results/) collections of [The Cancer Imaging Archive](https://www.cancerimagingarchive.net) currently hosted by IDC, with the links to the Digital Object Identifiers (DOIs) of those collections.

Original collections included:

1. [TCGA-PRAD](https://doi.org/10.7937/K9/TCIA.2016.YXOGLM4Y)
2. [TCGA-BLCA](https://doi.org/10.7937/K9/TCIA.2016.8LNG8XDR)
3. [TCGA-UCEC](https://doi.org/10.7937/K9/TCIA.2016.GKJ0ZWAC)
4. [TCGA-HNSC](https://doi.org/10.7937/K9/TCIA.2016.LXKQ47MS)
5. [TCGA-LUSC](https://doi.org/10.7937/K9/TCIA.2016.TYGKKFMQ)
6. [TCGA-KIRP](https://doi.org/10.7937/K9/TCIA.2016.ACWOGBEF)
7. [TCGA-THCA](https://doi.org/10.7937/K9/TCIA.2016.9ZFRVF1B)
8. [TCGA-SARC](https://doi.org/10.7937/K9/TCIA.2016.CX6YLSUX)
9. [TCGA-ESCA](https://doi.org/10.7937/K9/TCIA.2016.VPTNRGFY)
10. [TCGA-CESC](https://doi.org/10.7937/K9/TCIA.2016.SQ4M8YP4)
11. [TCGA-STAD](https://doi.org/10.7937/K9/TCIA.2016.GDHL9KIM)
12. [TCGA-COAD](https://doi.org/10.7937/K9/TCIA.2016.HJJHBOXZ)
13. [TCGA-KICH](https://doi.org/10.7937/K9/TCIA.2016.YU3RBCZN)
14. [TCGA-READ](https://doi.org/10.7937/K9/TCIA.2016.F7PPNPNU)
15. [TCGA-LUAD](https://doi.org/10.7937/K9/TCIA.2016.JGNIHEP5)
16. [TCGA-LIHC](https://doi.org/10.7937/K9/TCIA.2016.IMMQW8UQ)
17. [TCGA-BRCA](https://doi.org/10.7937/K9/TCIA.2016.AB2NAZRP)
18. [TCGA-OV](https://doi.org/10.7937/K9/TCIA.2016.NDO1MDFQ)
19. [TCGA-KIRC](https://doi.org/10.7937/K9/TCIA.2016.V6PBVTDR)
20. [TCGA-LGG](https://doi.org/10.7937/K9/TCIA.2016.L4LTD3TK)
21. [TCGA-GBM](https://doi.org/10.7937/K9/TCIA.2016.RNYFUYE9)
22. [ISPY1 (ACRIN 6657)](https://doi.org/10.7937/K9/TCIA.2016.HdHpgJLK)
23. [QIN-HeadNeck](https://doi.org/10.7937/K9/TCIA.2015.K0F5CGLI)
24. [LIDC-IDRI](https://doi.org/10.7937/K9/TCIA.2015.LO9QL9SX)
25. [NSCLC-Radiomics](https://wiki.cancerimagingarchive.net/display/Public/NSCLC-Radiomics)

Analysis collections included:

1. [Standardized representation of the TCIA LIDC-IDRI annotations using DICOM](https://doi.org/10.7937/TCIA.2018.h7umfurq)
2. [QIN multi-site collection of Lung CT data with Nodule Segmentations](https://doi.org/10.7937/K9/TCIA.2015.1BUVFJR7) (only items corresponding to the LIDC-IDRI original collection are included)
3. [DICOM SR of clinical data and measurement for breast cancer collections to TCIA](https://doi.org/10.7937/TCIA.2019.wgllssg1) (only items corresponding to the ISPY1 original collection are included)
