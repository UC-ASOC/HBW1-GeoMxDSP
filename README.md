# Hands-on Bioinformatics Workshop (HBW), Series I
ASOC (Applied Spatial Omics Centre) is hosting a **Spatial Transcriptomics Data Analysis Workshop** focusing on **Nanostring GeoMx DSP** data analysis and visualization to profile the whole-transcriptome of your regions of interest at the University of Calgary.

## Workshop Information 
**Registration: [https://ASOC.ucalgary.ca/HBW/](https://ASOC.ucalgary.ca/HBW/)**

The workshop will include lectures and hands-on tutorials to cover data (pre)processing, normalization, and visualization of the Nanostring GeoMx DSP data. Nanostring offers a set of R packages that enables comprehensive QC/analysis of the GeoMx DSP data and the following two vignettes will be covered in the Workshop. In addition, Nanostring generously provided the Spatial Organ Atlas and the Human Brain dataset will be used to generate results. The goal is to provide **comprehensive analysis schemes** for Spatial Transcriptomics data analysis, specifically Nanostring GeoMx DSP data, and other bioinformatic tasks.

1. Vignette: [GeoMxTools](https://www.bioconductor.org/packages/release/workflows/vignettes/GeoMxWorkflows/inst/doc/GeomxTools_RNA-NGS_Analysis.html)
2. Vignette: [SpatialOmicsOverlay](https://bioconductor.org/packages/release/bioc/vignettes/SpatialOmicsOverlay/inst/doc/SpatialOmicsOverlay.html)
3. Material: [Spatial Orgain Atals, Human Brain](https://nanostring.com/products/geomx-digital-spatial-profiler/spatial-organ-atlas/human-brain/)

## Prerequisites
This workshop is for **beginners** and no prior knowledge of Linux/Docker/R is required. Basic Linux commands will not be covered, however, you'll type in a couple of lines of Linux commands to deploy a Docker app. The Instructor will use a MacBook throughout the Workshop and it would be easier to follow the steps if participants brought a MacOS- or Linux-based machine. A Windows machine would still be okay and please make sure to have Docker installed on your machine.

1. **Bring your laptop**
2. [Docker](https://docs.docker.com/get-docker/)

## Materials - Download the following files before the Workshop
Nanostring generously agrees that ASOC can directly distribute and host a subset of Human Brain GeoMx DSP data for this Workshop and participants should download the following **two files before** the workshop on their machine. 

1. Human Brain GeoMx DSP data
  - If you can use git, ```git clone https://github.com/UC-ASOC/HBW1-GeoMxDSP.git```
  - Or, download the repo by clicking ```Code``` (green button at the top right corner) and ```Download ZIP```
2. [Human Brain OME.TIFF file](https://ASOC.ucalgary.ca/HBW/download/hu_brain_004b.ome.tiff)
3. [Docker App](https://hub.docker.com/r/ucasoc/hbw1-geomxdsp) (no need to download)

## Objectives
By the end of this workshop, participants will be able to **understand the GeoMx DSP data structure, generate diagnostic plots for QC, carry out statistical testing between user-defined groups, and visualize the segments on a tissue image**. Participants can preview a set of codes and outputs below. 

1. [Nanostring GeoMx DSP Data Analysis](https://ASOC.ucalgary.ca/HBW/HBW1-GeoMxDSP_Part1.html)
2. [Nanostring GeoMx DSP Data Visualization](https://ASOC.ucalgary.ca/HBW/HBW1-GeoMxDSP_Part2.html)

## Workshop Schedule 
| **Time**	 | **Activity**                                             | **Lead** |
|:-----------|:---------------------------------------------------------|:---------|
| 09:00 AM	 | Pre-workshop (Help to install Docker on your machine)    | Heewon   | 
| 10:00 AM   | Introduction to Spatial Omics                            | Bo Young |
| 11:00 AM   | R for Bioinformatics                                     | Heewon   |
|            |  - Data types and structure                              |          |
|            |  - Statistical functions                                 |          |
|            |  - Visualization functions                               |          |
| 01:00 PM   | Nanostring GeoMx DSP Data Analysis with Human Kidney     | Heewon   |
|            |  - GeoMx DSP data structure                              |          |
|            |  - Quality control/Normalization                         |          |
|            |  - Differential gene expression analysis                 |          |
| 03:00 PM	 | Nanostring GeoMx DSP Data Visualization with Mouse Brain | Heewon   |
|            |  - Plot ROIs (region of interests)                       |          |
|            |  - Overlay the sample image                              |          |
|            |  - Compile Figures in a paper                            |          |
| 04:30 PM   | Q & A                                                    | Bo Young & Heewon   |

## About Nanostring GeoMx Digital Spatial Profiler (DSP)
GeoMx DSP is the most flexible and robust spatial multi-omic platform for the analysis of formalin-fixed paraffin-embedded (FFPE) and fresh frozen (FF) tissue sections. GeoMx is the only spatial biology platform that non-destructively profiles the expression of RNA and protein from distinct tissue compartments and cell populations with an automated and scalable workflow that integrates with standard histology staining. It uses fluorescent imaging reagents to visualize tissue and oligo-tagged reagents for expression analysis. After selecting regions of interest (ROI), DNA oligos are released, and collected in a 96-well plate. Expressions levels are readout using Next-Generation Sequencing (NGS) or nCounter and analyzed using the DSP interactive software.

For more information, please visit [Overview on Nanostring.com](https://nanostring.com/products/geomx-digital-spatial-profiler/geomx-dsp-overview/) and [Spatial Biology Revealed with the GeoMx DSP by NanoString on YouTube](https://www.youtube.com/watch?v=mVhfZq8ppbc).
