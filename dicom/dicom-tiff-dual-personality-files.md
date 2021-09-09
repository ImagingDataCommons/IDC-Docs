# DICOM-TIFF dual personality files

DICOM and TIFF are two different image file formats that share many similar characteristics, and are capable of encoding exactly the same pixel data, whether uncompressed, or compressed with common lossy schemes \(including JPEG and JPEG 2000\). This allow the pixel data to be losslessly transformed from one format to the other and back.

The DICOM file format was also deliberately designed to allow the two formats \(TIFF and DICOM\) to peacefully co-exist in the same file, sharing the same pixel data without expanding the file size significantly. This is achieved by leaving some unused space at the front of the DICOM file \("preamble"\), which allows for the presence of a TIFF format recognition code \("magic number"\) and a pointer to its Image File Directory \(IFD\), which in turn contains pointers into the shared DICOM Pixel Data element.

The dual-personality mechanism supports both traditional strip-based TIFF organization, such as might be used to encode a single frame image, as well as the tile-based format, which is commonly used for Whole Slide Images \(WSI\), and which is encoded in DICOM with each tile as a frame of a "multi-frame" image.

Unlike TIFF files, which allow multiple different sized images to be encoded in the same file, DICOM does not, so there are limits to this approach. For example, though an entire WSI pyramid can be encoded in a TIFF file, the DICOM WSI definition requires each pyramid layer to be in a separate file, and all frames \(tiles\) within the same file to be the same size.

Most of the structural metadata that describes the organization and encoding of the pixel data is similar in DICOM and TIFF. It is copied into the tags \(data elements\) encoded in the respective format "headers". Biomedical-specific information, such as patient, specimen and anatomical identifiers and descriptions, as well as acquisition technique, is generally only encoded in the DICOM data elements, their being no corresponding standard TIFF tags for it. Limited spatial information \(such as physical pixel size\) can be encoded in TIFF tags, but more complex multi-dimensional spatial location is standardized only in the DICOM data elements.

The dictionary of TIFF tags can be extended with application-specific entries. This has been done for various non-medical and medical applications \(e.g., GeoTIFF, DNG, DEFF\). Other tools have used alternative mechanisms, such as defining text string \(Leica/Aperio SVS\) or structured metadata in other formats \(such as XML for OME\) buried within a TIFF string tag \(e.g, ImageDescription\). This approach can be used with DICOM-TIFF dual-personality files as well, since DICOM does not restrict the content of the TIFF tags; it does require updating or crafting of the textual metadata to actually reflect the characteristics of the encoded pixel data.

It is hoped that the dual-personality approach may serve to mitigate the impact of limited support of one format or the other in different clinical and research tools for acquisition, analysis, storage, indexing, distribution, viewing and annotation.

{% hint style="info" %}
For further information and an example open source implementation, see 

Clunie, D. A. [Dual-Personality DICOM-TIFF for whole slide images: A migration technique for legacy software](http://doi.org/10.4103/jpi.jpi_93_18). J. Pathol. Inform. **10,** 12 \(2019\). 
{% endhint %}



