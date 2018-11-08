# EPrints - Archivematica Integration
Digital Preservation through EPrints-Archivematica Integration - An EPrints export plugin for contents to be preserved with Archivematica

The EPrints-Archivematica integration proposal was first presented at Archivematica Camp and OR2018 in Bozeman.
The OR2018 presentation is available here:

Neugebauer, Tomasz , Simpson, Justin and Bradley, Justin (2018) Digital Preservation through EPrints-Archivematica Integration. In: International Conference on Open Repositories, June 3-7, 2018, Bozeman, Montana, USA
https://spectrum.library.concordia.ca/983933/

# Summary
The following is a summary of the proposed workflow for EPrints-Archivematica integration:
* “Digital Preservation Export” batch script runs periodically that identifies new/updated items to
export and generates the bags described below.

* BagIt hierarchical packaging format is used create one bag per eprint, including: a metadata folder
with Dublin Core metadata (in TEXT format), EPrints XML metadata, all uploaded digital files that
are a part of the eprint in a data folder, all EPrints generated “revision” XML files in a revisions
folder, and a derivatives folder containing any derivative access files that were generated by
EPrints, such as thumbnail images, audio access files, video access files. Bags are moved to
specified shared storage location.

![Eprint Export Folder Structure, before bagging](https://raw.githubusercontent.com/photomedia/EPrintsArchivematica/master/eprint-export-folder-structure.png)

* The following would be the structure of the data folder:
eprintid-XXXXX -> documents -> documentid-XXXXX -> files -> fileid-XXXXX -> folder# -> filename

![Eprint Export Folder Structure - Data Folder Structure](https://raw.githubusercontent.com/photomedia/EPrintsArchivematica/master/eprint-export-folder-strcture-data-folder-structure.png)

* The following would be the structure of the derivatives folder:
fileid-XXXXX -> folder# -> filename

* Archivematica monitors shared storage for new bags and processes new items by: extracting the
Dublin Core Metadata (in TEXT format) and converting it to JSON for indexing, using the eprintID as
an indexed identifier for the AIP, generating digital preservation AIP by running the full
identification, characterization, normalization routines on all files in the bag, and finally moving the
AIP to archival storage.

This integration is currently in the technical specification phase.
