# DICOM

DICOM standard is one of the key foundation components of the IDC. In the following we summarize the relevant characteristics of the DICOM standard, and provide relevant pointers, to support our decision to use DICOM in IDC both for data representation and as a communication protocol.

{% hint style="info" %}
DICOM is the _comprehensive standard for the management and communication of medical imaging information and imaging-related data_. 
{% endhint %}

DICOM is used universally in clinical operations involving acquisition, communication, visualization and interpretation of radiological and cardiological images, and is used increasingly for all enterprise imaging applications, including medical photography, ophthalmology, dermatology, and more recently, for whole slide imaging for histopathology and cytology. 

DICOM applications, however, extend beyond clinical domains into _clinical trials_ as well as _pre-clinical research_. DICOM approaches modeling the complexity of the real world using a hierarchy of abstract objects described by a set of attributes. Real-world objects, such as a patient, or a tissue specimen image, correspond to DICOM _Information Object Definitions \(IODs\)_. 

DICOM objects, such as images, are composite IODs that include attributes of different real-world entities \(e.g., a CT image would include attributes describing the patient and imaging study, alongside the attribute with the details of CT image acquisition\). The resulting real-world model is comprehensive, incorporating a variety of objects corresponding to imaging modalities, imaging-derived data \(e.g., results of post-processing and annotation of images\), and imaging-related data \(e.g., clinical information describing relevant patient characteristics or specimen handling\). 

The standard and the data model are not limited to human imaging. The standard includes provisions that harmonize data representation for veterinary and pre-clinical small animal imaging, and includes capabilities to characterize, in detail, image acquisition context in pre-clinical applications \(e.g., animal handling, housing or medication, critical for pre- and co-clinical trials, as an example\). 

The DICOM data model is also extensible: there is an established process for modification and refinement of the DICOM standard through participation in working groups that include a range of stakeholders, such as vendors of imaging equipment, pharma, clinical and research users. 

Importantly, the DICOM standard is not limited to the definition of the data model and object storage representation. It is comprehensive in defining various aspects related to communication and management of imaging and image-related data, such as communication protocols and presentation of the data. The DICOMweb communication component of the standard found broad adoption in commercial and open source tools and, importantly, was chosen by Google Healthcare for cloud deployment of imaging data. By utilizing the DICOM standard, we will leverage decades of development and participation, and harmonization with the existing infrastructure.

