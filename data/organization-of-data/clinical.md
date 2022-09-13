# Clinical data

As of Version 11 IDC now provides a public [BigQuery dataset](https://console.cloud.google.com/bigquery?p=bigquery-public-data&d=idc_clinical_current)  with clinical data associated with several of its imaging collections. The clinical data tables associated with a particular version are in the dataset `bigquery-public-data.idc_<idc_version_number>_clinical`. In addition the dataset `bigquery-public-data.idc_current_clinical` has an identically named view for each table in the BQ clinical dataset corresponding to the current IDC release.
 
There are currently 189 tables with clinical data representing 78 different collections. Most of this data was curated from Excel and CSV files downloaded from [The Cancer Imaging Archive (TCIA) wiki](https://wiki.cancerimagingarchive.net/). For most collections data is placed in a single table named `<collection_id>_clinical`, where `<collection_id>` is the name of the collection in a standardized format (i.e. the `idc_webapp_collection_id` column in the `dicom_all` view in the [idc_current dataset](https://console.cloud.google.com/bigquery?p=bigquery-public-data&d=idc_clinical&page=dataset)). Collections from the ACRIN project have different types of clinical data spread across CSV files, and so this data is represented by several BigQuery tables. The clinical data for collections in the [CPTAC program](https://proteomics.cancer.gov/programs/cptac) program is not curated from TCIA but instead is copied from a [BigQuery table](https://console.cloud.google.com/bigquery?p=isb-cgc-bq&d=cptac&t=clinical_gdc_current&page=table) in the ISB-CGC project, which in turn was sourced from the [Genomics Data Commons (GDC) api](https://gdc.cancer.gov/developers/gdc-application-programming-interface-api). Similarly clinical data for collections in the [TCGA program](https://www.cancer.gov/about-nci/organization/ccg/research/structural-genomics/tcga) is copied from the table `tcga_clinical_rel9` in `idc_current` dataset, which was also created using the [Genomics Data Commons (GDC) api](https://gdc.cancer.gov/developers/gdc-application-programming-interface-api). Every clinical data table contains two fields we have introduced, `dicom_patient_id` and `source_batch`. `dicom_patient_id` is identical to the `PatientID` field in the DICOM files that correspond to the given patient. The `dicom_patient_id` value is determined by inspecting the patient column in the clinical data file. In some of the collection's clinical data, the patients were separated into different 'batches' i.e. different source files, or different sheets in the same Excel file. The `source_batch` field is an integer indicating the 'batch' for the given patient. For most collections, in which all patients data is found in the same location, the `source_batch` value is zero.   

Most of the clinical tables are legible by themselves. Tables from the ACRIN collection are an exception as the column names and some of the column values are coded. To provide for clarity and ease of use of all clinical data, we have created two metadata tables, [`table_metadata`](https://console.cloud.google.com/bigquery?p=bigquery-public-data&d=idc_clinical_current&t=table_metadata&page=table) and [`column_metadata`](https://console.cloud.google.com/bigquery?p=bigquery-public-data&d=idc_clinical_current&t=column_metadata&page=table) that provide information about the structure and provenance of all data in this dataset. `table_metadata` has table-level metadata about each clinical collection, while `column_metadata` has column-level metadata.      

Structure of `table_metadata`:

* `collection_id` (STRING, NULLABLE) - the collection_id of the collection in the given table. The collection id is in a format used internally by the IDC Web App (with only lowercase letters, numbers and '_' allowed). It is equivalent to the `idc_webapp_id` field in the `dicom_all` view in the `idc_current` dataset. 

* `table_name` (STRING,NULLABLE) - name of the table

* `table_description` (STRING,NULLABLE) - description of the type of data found in the table. Usually this is set to 'clinical data', unless a description is provided in the source files 

* `idc_version_table_added` (STRING, NULLABLE) - the IDC data version for which this table was first added

* `idc_table_added_datetime` (STRING,NULLABLE) - the date/time this particular table was first generated 

* `post_process_src` (STRING, NULLABLE) - except for cptac_clinical the tables are curated from ZIP, Excel, and CSV files downloaded from the TCIA wiki. These files do not have a consistent structure and were not meant to be machine readable or to translate directly into BigQuery. A semi-manual curation process results in either a CSV of JSON file that can be directly written into a BigQuery table. _post_process_src_ is the name of the JSON or CSV file that results from this process and is used to create the BigQuery table. This field is not used for the cptac_clinical table   

* `post_process_src_add_md5` (STRING, NULLABLE) - the md5 hash of post_process_src when the table was first added

* `idc_version_table_prior` (STRING, NULLABLE) - the idc version the second most recent time the table was updated

* `post_process_src_prior_md5` (STRING, NULLABLE) - the md5 hash of post_process_src the second most recent time the table was updated

* `idc_version_table_updated` (STRING, NULLABLE) - the idc version when the table was last updated

* `table_update_datetime` (STRING, NULLABLE) - date and time an update of the table was last recorded

* `post_process_src_updated_md5` (STRING, NULLABLE) - the md5 hash of post_process_source when the table was last updated

* `number_batches` (INTEGER, NULLABLE) - records the number of batches. Within the source data patients are sometimes grouped into different 'batches' (i.e. training vs test, responder vs non-responder etc.) and the batches are placed in different locations (i.e. different files or different sheets in the same Excel file)

* `source_info` (RECORD, REPEATED) - an array of records with information about the table sources. These sources are either files downloaded from the TCIA wiki or another BigQuery table (as is the case for CPTAC). There is a source_info record for each source 'batch' described above

* `source_info.srcs` (STRING, REPEATED) - a source file downloaded from the TCIA wiki may be a ZIP file, and CSV file, or an Excel file. Sometimes the ZIP files contain other ZIP files that must be opened to extract the clinical data. In the _source_info.src_ array the first string is the file that is downloaded from TCIA for this particular source batch. The final string is the CSV or Excel file that contains the clinical data. Any intermediate strings are the names of ZIP files 'in between' the downloaded file and the clinical file. For the cptac_clinical table, this field contains the source BigQuery table (isb-cgc-bq.CPTAC.gdc_current)   

* `source_info.added_md5` (STRING, NULLABLE) - md5 hash of the downloaded file from TCIA when the table was first added

* `source_info.prior_md5` (STRING, NULLABLE) - md5 hash of the downloaded file from TCIA the second most recent time the table was updated

* `source_info.update_md5` (STRING, NULLABLE) - md5 hash of the downloaded file from TCIA the most recent time the table was updated

* `source_info.table_last_modified` (STRING, NULLABLE) - cptac_clinical only. The date and time the source BigQuery table was most recently modified, as recorded when last copied

* `source_info.table_size` (STRING, NULLABLE) - cptac_clinical only. The size of the source BigQuery table as recorded when last copied


Structure of column_metadata:

* `collection_id` (STRING,NULLABLE) - the collection_id of the collection in the given table. The collection id is in a format used internally by the IDC web app (with only lowercase letters, numbers and _ allowed). It is equivalent to _idc_webapp_id_ in the dicom_all view in the idc_current dataset. Except for cptac_clinical there is only one collection per table. For cptac collection_id it is a comma delimited list of all cptac collections (as determined by the field type it is stored as a string not an array)


* `case_col` (BOOLEAN, NULLABLE) - true if the BigQuery column contains the patient or case id, i.e. if this column is used to determine the value of the _dicom_patient_id_ column 

* `table_name` (STRING, NULLABLE) - table name

* `column_name` (STRING, NULLABLE) - the actual column field name in the table. This nomenclature is derived from the data structure of the ACRIN programs which use a data dictionary and encoded attribute names. For ACRIN collections the _variable_name_ is that sourced from the provided data dictionary. For other collections it is a name constructed by 'normalizing' the variable_label (see next) in a format that can be used as a BigQuery field name  

* `column_label` (STRING, NULLABLE) - a 'free form' label for the column that does not need to conform to the BigQuery column format requirements. For ACRIN collections this is again provided by a data dictionary that accompanies the collection. For other collections it is the name or label of the clinical attribute as inferred from the source document during the curation process

* `data_type` (STRING, NULLABLE) - the type of data in this column. Again for ACRIN collections this is provided in the data dictionary. For other collections it is inferred by analyzing the data during curation

* `original_column_headers` (STRING, REPEATED) - the name(s) or label(s) in the source document that were used to construct the _variable_label_ field. In most cases there is one column label in the source document that perscribes the _variable_label_. In some cases, multiple columns are concantenated and reformated to form the _variable_label_  

* `values` (RECORD, REPEATED) - a structure that is again borrowed from the ACRIN data model. This is an array that contains observerd attribute values for this given column. For ACRIN collections these values are reported in the data dictionary. For other collections these values are determined by analyzing the source data. For simplicity this field is left blank when the number of unique values is greater than 20

* `values.option_code` (STRING, NULLABLE) -  a value for this data column that is present at least once in the source data, and is now found in this BigQuery column 
   

* `values.option_description` (STRING, NULLABLE) -  a description of the _option_code_ (ACRIN only)  

* `files` (STRING, REPEATED) - names of the files that contain the source data for each batch. These are the Excel or CSV files directly downloaded from TCIA, or the files extracted from downloaded ZIP files

* `sheet_names` (STRING, REPEATED) - for Excel-sourced files, the sheet names containing this column's values for each batch


* `batch` (INTEGER, REPEATED) - source batches that contain this particular column. Some columns or attributes may be missing from some batches


* `column_numbers` (STRING, REPEATED) - for each source batch, the column in the original source corresponding to this column in the BigQuery table

