# Viewing collections, studies, and series

All collections in IDC as well as their total number of cases and number of cases in this cohort appear in the Collections panel. The panel shows the collection name, total number of cases, and total number of cases for this cohort. You can customize your display of this data by choosing how many entries to show by page and move to previous and next pages.

Click one or more collections to select them. The selected row or rows are highlighted. The available cases for the selected collection(s) appear in the Selected Cases panel. Click the up or down arrow to sort the list alphabetically or numerically, as appropriate for the column.

![Collections panel](<../../.gitbook/assets/collections-panelv2 (2) (2) (2) (2) (2) (4) (4) (4) (2) (4) (4) (1) (1) (1) (4).png>)

{% hint style="info" %}
You must select a collection _before_ you can view data in the Selected Cases, Selected Studies, and Selected Series panels.
{% endhint %}

## Selecting a case per collection

All cases available for the selected collection appear in the Selected Cases panel. The panel shows the collection name, case ID, total number of studies, and total number of series for each case. You can customize your display of this data by choosing how many entries to show by page and move to previous and next pages.

![Selected Cases panel](../../.gitbook/assets/selected\_cases-panel.png)

Click one or more cases to select them. The selected row or rows are highlighted. The available studies for the selected case(s) appear in the Selected Studies panel. Click the up or down arrow to sort the list alphabetically or numerically, as appropriate for the column.

## Viewing studies **p**er case

All studies available for the selected case appear in the Selected Studies panel. The panel shows the project name, case ID, study ID, and study description for each study. You can customize your display of this data by choosing how many entries to show by page and move to previous and next pages.

![Selected Studies panel](../../.gitbook/assets/selected\_studies-panel.png)

Click one or more studies to select them. The selected row or rows are highlighted. The available series for the selected case(s) appear in the Selected Series panel. Click the up or down arrow to sort the list alphabetically or numerically, as appropriate for the column.

Click the icon in the View column for a study row to view study objects in the IDC Viewer. Depending on the type of image, it will be opened using either[ Open Health Imaging Foundation (OHIF) Viewer](https://docs.ohif.org/) (for radiology data) or [SliM Viewer](https://github.com/MGHComputationalPathology/slim) (for digital pathology data).

## Viewing a series per study per case

All series available for the selected study appear in the Selected Series panel. The panel shows the study ID, series number, modality, body part examined, and series description for each study. You can customize your display of this data by choosing how many entries to show by page and move to previous and next pages. Click the up or down arrow to sort the list alphabetically or numerically, as appropriate for the column.

![Selected Series panel](../../.gitbook/assets/selected\_series-panel.png)

Click the icon in the View column for a study row to view the specific series in IDC Viewer.

{% hint style="warning" %}
Some objects can only be opened by the OHIF viewer at their related study level and not at the series level. For these objects, the icon in the View column shows that viewing is disabled.
{% endhint %}

## Viewing data in BigQuery

After you export a cohort manifest to BigQuery, you can view IDC data in BigQuery as a BQ table. An example query follows that returns all studies for the collection QIN\_HEADNECK.

```sql
SELECT PatientID, StudyInstanceUID
FROM `canceridc-data.idc_current.dicom_all`
WHERE collection_id = 'qin_headneck'
GROUP BY PatientID, StudyInstanceUID
```
