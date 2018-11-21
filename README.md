# EPrints - Archivematica Integration

Digital Preservation through EPrints-Archivematica Integration - An EPrints export plugin for contents to be preserved with Archivematica

The EPrints-Archivematica integration proposal was first presented at Archivematica Camp and OR2018 in Bozeman.
The OR2018 presentation is available here:

Neugebauer, Tomasz , Simpson, Justin and Bradley, Justin (2018) Digital Preservation through EPrints-Archivematica Integration. In: International Conference on Open Repositories, June 3-7, 2018, Bozeman, Montana, USA
https://spectrum.library.concordia.ca/983933/

# Summary

The following is a summary of the proposed workflow for EPrints-Archivematica integration:

* “Digital Preservation Export” batch script runs periodically that identifies new/updated items to
export and generates the exports in a directory structure optimized for [Archivematica transfers](https://www.archivematica.org/en/docs/archivematica-1.7/user-manual/transfer/transfer/#transfer-checksums) described below.

* The export plugin will create a transfer for each eprint. Each transfer includes: 
	* An `objects` directory containing the uploaded digital files that are part of the eprint as well as any derivative access files generated by EPrints
	* An `objects/documents` folder containing all uploaded digital files that are a part of the eprint
	* A `objects/derivatives` folder containing any derivative access files that were generated by EPrints, such as thumbnail images, audio access files, video access files
	* A `metadata` folder with Dublin Core metadata (in JSON format), EPrints XML metadata, EPrints-generated "revision" XML files, and an `md5deep`-style checksum manifest for digital files in the `objects` directory
	* A `metadata/revisions` folder containing all EPrints-generated “revision” XML files

Transfers are moved to a specified shared storage location. 

![Eprint Export Folder Structure](https://github.com/photomedia/EPrintsArchivematica/blob/master/eprint-export-folder-structure.png)

* The following would be the structure of the documents folder:

![Eprint Export Folder Structure - Documents](https://github.com/photomedia/EPrintsArchivematica/blob/master/eprint-export-documents-folder-structure.png)

* The following would be the structure of the derivatives folder:

`fileid-XXXXX -> folder# -> filename`

* Archivematica's [Automation Tools](https://github.com/artefactual/automation-tools) monitors shared storage for new bag, creates transfers/ingests in Archivematica according to a user-defined processing configuration, and then stores AIPs in archival storage.

This integration is currently in the technical specification phase.

# Implementation details

## Checksum manifest

The `metadata/checksum.md5` file should follow the specifications detailed in the [Archivematica documentation for creating a transfer with existing checksums](https://www.archivematica.org/en/docs/archivematica-1.8/user-manual/transfer/transfer/#create-a-transfer-with-existing-checksums).

Specifically, in this implementation, each line of the `checksum.md5` manifest should contain the md5 hash value for a file in the `objects` directory, followed by a space, followed by the relative path to the file from the `checksum.md5` file itself.

Example:

`2121dca88ad7f701d3f3e2d041004a56 ../objects/documents/my-doc.pdf`
