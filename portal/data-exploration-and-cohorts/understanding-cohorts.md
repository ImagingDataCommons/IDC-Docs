# Understanding cohorts

Creating a cohort helps you quickly return to a subset of interest from the vast collection of data available in the Imaging Data Commons. A cohort is composed of your selections in the Search Scope and Search Configuration panels.

## Creating a cohort

Do the following to create a cohort.

1. [Select filters](./#defining-search-scope-and-configuration) on the Search Scope and Search Configuration panels.
2.  Click the **Save As New Cohort** button in the top-right of the portal.

    The Save Cohort dialog box appears, showing your selected filters.
3. Enter a name for the cohort. Optionally, enter a description.
4. Click **Save Cohort**. The cohort details page appears.

![Save Cohort dialog box](../../.gitbook/assets/save-cohort-confirmation-v2.png)

The cohort details page shows the cohort name, description (if available), and filter definition.

![Greyed out filter options on the Search Configuration panel](../../.gitbook/assets/screen-shot-2021-03-02-at-9.15.10-am.png)

You can open any study or series associated with the cohort using the IDC Viewer. For more information on image visualization, see [Visualizing images](../visualization.md).

![Cohort details page](../../.gitbook/assets/cohort\_details-page.png)

## Viewing the cohorts list

Click the **Cohorts** button in the header to view a list of all the cohorts you created under your account. Each row in the cohorts list includes the corresponding cohort ID, name, the number of cases, studies, and series in the cohort, and the version of the IDC data against which the cohort was created. Pressing the **plus** icon on any row in the cohort list opens a second related row that provides information about the cohort's collections and filters.

![](../../.gitbook/assets/screen-shot-2021-06-07-at-9.12.08-am.png)

Cohort manifests can be exported directly from the **Cohorts** page. Click the checkboxes corresponding to the desired cohorts, then click the **Export Manifest** button which opens the Export Cohort Manifest dialog box, discussed in the next section. Complete this dialog box to export separate manifests for all selected cohorts. Note that only a single cohort manifest can be exported to a file download at one time. Also manifests corresponding to the cohorts with inactive data versions cannot be downloaded. When multiple manifests are exported to BigQuery they are copied to separate BigQuery tables. One or more cohorts can be deleted by clicking the relevant checkboxes and then clicking the **Delete** button.

## Cohort Versions

To indicate cohorts created with previous data versions in the cohort list the **Data Version** cells of such cohorts will have a grey background. The cohort manifest remains accessible for all cohorts after the active data version changes. However cohorts created with older data versions can no longer be opened within the portal. Clicking on the button in the **Version Compare** cell in the cohort list opens a pop-up window that compares the number of cases, studies, and series in the current cohort with those of a new cohort that would be created by applying the cohort's filters to the current data set.

![](../../.gitbook/assets/screen-shot-2021-06-07-at-2.02.08-pm.png)

Clicking on the **Load New Version** button will open the explorer page, applying these cohort filters to the current data version. The user can then save this new cohort or modify the filters as desired.

![](<../../.gitbook/assets/screen-shot-2021-06-09-at-8.46.36-am (1) (1) (1) (1) (1) (1).png>)
