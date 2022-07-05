# Clinical data

As of v10 IDC now provides a public [BigQuery dataset](https://console.cloud.google.com/bigquery?p=bigquery-public-data&d=idc_clinical_current&page=dataset&project=idc-dev-etl)  with clinical data associated with several of its imaging collections. There are currently 145 tables with clinical data representing 45 different collections. Most of this data was curated from Excel and CSV files downloaded from [The Cancer Imaging Archive (TCIA) wiki](https://wiki.cancerimagingarchive.net/). For most collections data is placed in a single table named _collection_id_clinical_, where _collection_id_ is the name of the collection in a standardized format (i.e. the _idc_webapp_collection_id_ field in the _dicom_all_ view in the [idc_current dataset](https://console.cloud.google.com/bigquery?p=bigquery-public-data&d=idc_clinical&page=dataset&project=idc-dev-etl)). Collections from the ACRIN project have differtent types of clinical data spread across CSV files, and so this data is represented by several BigQuery tables. The [cptac_clinical](https://console.cloud.google.com/bigquery?p=bigquery-public-data&d=idc_clinical_current&t=cptac_clinical&page=table&project=idc-dev-etl) table was not curated from TCIA but instead was copied from a [BigQuery table](https://console.cloud.google.com/bigquery?p=isb-cgc-bq&d=cptac&t=clinical_gdc_current&page=table&project=idc-dev-etl) in the ISB-CGC project, which in turn is sourced from the [Genomics Data Commons (GDC) api](https://gdc.cancer.gov/developers/gdc-application-programming-interface-api). cptac_clinical is the only table that contains clinical data from several collections (each collection in the CPTAC program). Each clinical data table also contains two fields we have introduced, _dicom_patient_id_ and _source_batch_. _dicom_patient_id_ is identical to the _PatientID_ field in the DICOM files that correspond to the given patient. This is inferred from a patient or case column in the original source data. The _source_batch_ field contains an integer that references metadata (see below) indicating which 'batch' of original source records contained the given record. In some of the collection's clinical data, the patient's were separated into different 'batches' i.e. different source files, or different sheets in the same Excel file.  

Most of the clinical tables are legible by themselves. Tables from the ACRIN collection are an exception as the field names and some of the field values are coded. We provide two meta data tables, [table_metadata](https://console.cloud.google.com/bigquery?p=bigquery-public-data&d=idc_clinical_current&t=table_metadaata&page=table&project=idc-dev-etl) and [column_metadata](https://console.cloud.google.com/bigquery?p=bigquery-public-data&d=idc_clinical_current&t=column_metadata&page=table&project=idc-dev-etl) that provide information about the structure and provenance of the clinical data in this dataset. table_metadata has table-level metadata about each clinical collection, while column_metadata has column-level metadata.      

Structure of table_metadata:

* _collection_id_ (STRING, NULLABLE) - the collection_id of the collection in the given table. The collection id is in a format used internally by the IDC web app (with only lowercase letters, numbers and _ allowed). It is equivalent to _idc_webapp_id_ field in the dicom_all view in the idc_current dataset. Except for cptac_clinical there is only one collection per table. For cptac _collection_id_ it is a comma delimited list of all cptac collections (as dictated by the column field type it is stored as a string not an array) 

* _table_name_ (STRING,NULLABLE) - name of the table

* _table_description_ (STRING,NULLABLE) - table description

* _idc_version_table_added_ (STRING, NULLABLE) - the IDC data version for which this table was first added

* _idc_table_added_datetime_ (STRING,NULLABLE) - the date/time this particular table was first generated 

* _post_process_src_ (STRING, NULLABLE) - except for cptac_clinical the tables are curated from ZIP, Excel, and CSV files downloaded from the TCIA wiki. These files do not have a consistent structure and were not meant to be machine readable or to translate directly into BigQuery. A semi-manual curation process results in either a CSV of JSON file that can be directly written into a BigQuery table. _post_process_src_ is the name of the JSON or CSV file that results from this process and is used to create the BigQuery table. This field is not used for the cptac_clinical table   

* _post_process_src_add_md5_ (STRING, NULLABLE) - the md5 hash of post_process_src when the table was first added

* _idc_version_table_prior_ (STRING, NULLABLE) - the idc version the second most recent time the table was updated

* _post_process_src_prior_md5_ (STRING, NULLABLE) - the md5 hasd of post_process_src the second most recent time the table was updated

* _idc_version_table_updated_ (STRING, NULLABLE) - the idc version when the table was last updated

* _table_update_datetime_ (STRING, NULLABLE) - date and time an update of the table was last recorded

* _post_process_src_updated_md5_ (STRING, NULLABLE) - the md5 hash of post_process_source when the table was last updated

* _number_batches_ (INTEGER, NULLABLE) - records the number of batches. Within the source data patients are sometimes grouped into different 'batches' (i.e. training vs test, responder vs non-responder etc.) and the batches are placed in different locations (i.e. different files or different sheets in the same Excel file)

* _source_info_ (RECORD, REPEATED) - an array of records with information about the table sources. These sources are either files downloaded from the TCIA wiki or another BigQuery table (as is the case for CPTAC). There is a source_info record for each source 'batch' described above

* _source_info.srcs_ (STRING, REPEATED) - a source file downloaded from the TCIA wiki may be a ZIP file, and CSV file, or an Excel file. Sometimes the ZIP files contain other ZIP files that must be opened to extract the clinical data. In the _source_info.src_ array the first string is the file that is downloaded from TCIA for this particular source batch. The final string is the CSV or Excel file that contains the clinical data. Any intermediate strings are the names of ZIP files 'in between' the downloaded file and the clinical file. For the cptac_clinical table, this field contains the source BigQuery table (isb-cgc-bq.CPTAC.gdc_current)   

* _source_info.added_md5_ (STRING, NULLABLE) - md5 hash of the downloaded file from TCIA when the table was first added

* _source_info.prior_md5_ (STRING, NULLABLE) - md5 hash of the downloaded file from TCIA the second most recent time the table was updated

* _source_info.update_md5_ (STRING, NULLABLE) - md5 hash of the downloaded file from TCIA the most recent time the table was updated

* _source_info.table_last_modified_ (STRING, NULLABLE) - cptac_clinical only. The date and time the source BigQuery table was most recently modified, as recorded when last copied

* _source_info.table_size_ (STRING, NULLABLE) - cptac_clinical only. The size of the source BigQuery table as recorded when last copied


Structure of column_metadata:

* _collection_id_ (STRING,NULLABLE) - the collection_id of the collection in the given table. The collection id is in a format used internally by the IDC web app (with only lowercase letters, numbers and _ allowed). It is equivalent to _idc_webapp_id_ in the dicom_all view in the idc_current dataset. Except for cptac_clinical there is only one collection per table. For cptac collection_id it is a comma delimited list of all cptac collections (as determined by the field type it is stored as a string not an array)


* _case_col_ (BOOLEAN, NULLABLE) - true if the BigQuery column contains the patient or case id, i.e. if this column is used to determine the value of the _dicom_patient_id_ column 

* _table_name_ (STRING, NULLABLE) - table name

* _variable_name_ (STRING, NULLABLE) - the actual column field name in the table. This nomenclature is derived from the data structure of the ACRIN programs which use a data dictionary and encoded attribute names. For ACRIN collections the _variable_name_ is that sourced from the provided data dictionary. For other collections it is a name constructed by 'normalizing' the variable_label (see next) in a format that can be used as a BigQuery field name  

* _variable_label_ (STRING, NULLABLE) - a 'free form' label for the column that does not need to conform to the BigQuery column format requirements. For ACRIN collections this is again provided by a data dictionary that accompanies the collection. For other collections it is the name or label of the clinical attribute as inferred from the source document during the curation process

* _data_type_ (STRING, NULLABLE) - the type of data in this column. Again for ACRIN collections this is provided in the data dictionary. For other collections it is inferred by analyzing the data during curation

* _original_column_headers_ (STRING, REPEATED) - the name(s) or label(s) in the source document that were used to construct the _variable_label_ field. In most cases there is one column label in the source document that perscribes the _variable_label_. In some cases, multiple columns are concantenated and reformated to form the _variable_label_  

* _values_ (RECORD, REPEATED) - a structure that is again borrowed from the ACRIN data model. This is an array that contains observerd attribute values for this given column. For ACRIN collections these values are reported in the data dictionary. For other collections these values are determined by analyzing the source data. For simplicity this field is left blank when the number of unique values is greater than 20

* _values.option_code_ (STRING, NULLABLE) -  a value for this data column that is present at least once in the source data, and is now found in this BigQuery column 
   

* _values.option_description_ (STRING, NULLABLE) -  a description of the _option_code_ (ACRIN only)  

* _batch_ (INTEGER, REPEATED) - source batches that contain this particular column. Some columns or attributes may be missing from some batches

* _files_ (STRING, REPEATED) - names of the files that contain the source data for each batch. These are the Excel or CSV files directly downloaded from TCIA, or the files extracted from downloaded ZIP files

* _sheet_names (STRING, REPEATED) - for Excel-sourced files, the sheet names containing this column's values for each batch

* _column_numbers_ (STRING, REPEATED) - for each source batch, the column in the original source corresponding to this column in the BigQuery table

