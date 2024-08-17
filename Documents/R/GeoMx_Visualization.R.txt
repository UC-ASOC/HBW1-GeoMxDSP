##### ##### ##### ##### ##### ##### ##### ##### ##### #####
# Hands-on Bioinformatics Workshop
# Spatial Transcriptomics Data Analysis - Nanostring GeoMx DSP
# Data: https://nanostring.com/products/geomx-digital-spatial-profiler/spatial-organ-atlas/human-brain/
# Reference: https://bioconductor.org/packages/release/bioc/vignettes/SpatialOmicsOverlay/inst/doc/SpatialOmicsOverlay.html
#
# Modified by Heewon Seo (Heewon.Seo@UCalgary.ca)
# Updated on July 28, 2024
##### ##### ##### ##### ##### ##### ##### ##### ##### #####

##### ##### ##### ##### ##### ##### ##### ##### ##### #####
# Directory settings
baseDir <- "/home/rstudio/analysis/"
outDir <- file.path(baseDir, "Results")
dir.create(outDir, showWarnings = FALSE)

##### ##### ##### ##### ##### ##### ##### ##### ##### #####
# Load libraries
library(SpatialOmicsOverlay)
library(GeomxTools)

##### ##### ##### ##### ##### ##### ##### ##### ##### #####
# -- * -- Step 1 -- * --
# Load XML (coordinates) from OME.TIFF and Lab Worksheet (annotation) into a SpatialOverlay (SO) object, e.g., soSet.RDS
tiffFile <- file.path(baseDir, "Images", "hu_brain_004b.ome.tiff")
labWorksheet <- file.path(baseDir, "LabWorksheet", "01SEPT2021_HWTA_20210929T1939_LabWorksheet.txt")
lwAnnot <- readLabWorksheet(lw = labWorksheet, slideName = "hu_brain_004b")

soObj <- readSpatialOverlay(ometiff = tiffFile, annots = labWorksheet, 
                              slideName = gsub(".ome.tiff", "", basename(tiffFile)), 
                              image = FALSE, # will load the image later
                              saveFile = FALSE, outline = FALSE)
soObj

# Check the stats
slideName(soObj)

sampNames(soObj)

meta(overlay(soObj))

head(coords(soObj))

##### ##### ##### ##### ##### ##### ##### ##### ##### #####
# -- * -- Step 2 -- * --
# Generating a Figure without image
# Check the GeoMx DSP instrument output: https://ASOC.ucalgary.ca/SOA_Brain/ROI/Hippocampus_84.png
pdf(file.path(outDir, "12_SpatialOverlay.pdf"))
plotSpatialOverlay(overlay = soObj, hiRes = FALSE, legend = FALSE, scaleBar = FALSE)
dev.off()

##### ##### ##### ##### ##### ##### ##### ##### ##### #####
# -- * -- Step 3 -- * --
# Import the HPCA gene expression and visualize the expression level
geomxObj <- readRDS(file.path(outDir, "08_GeoMxSet_AnalysisReady.RDS"))

soObj <- addPlottingFactor(overlay = soObj, annots = lwAnnot, plottingFactor = "segment")
soObj <- addPlottingFactor(overlay = soObj, annots = geomxObj, plottingFactor = "HPCA")
soObj <- addPlottingFactor(overlay = soObj, annots = 1:length(sampNames(soObj)), plottingFactor = "ROILabel")
soObj

head(plotFactors(soObj))

pdf(file.path(outDir, "13_HPCAexpr.pdf"))
plotSpatialOverlay(overlay = soObj, hiRes = FALSE, colorBy = "HPCA", scaleBar = FALSE) +
	viridis::scale_color_viridis() +
	ggplot2::labs(title = "HPCA Expression in Hippocampus")
dev.off()

##### ##### ##### ##### ##### ##### ##### ##### ##### #####
# -- * -- Step 4 -- * --
# Customizing the Figure
## (1) Adding a scale bar
pdf(file.path(outDir, "14_HPCAexpr_scaleBar.pdf"))
plotSpatialOverlay(overlay = soObj, hiRes = FALSE, colorBy = "HPCA", scaleBarWidth = 0.3, scaleBarColor = "black", corner = "bottomleft") +
	viridis::scale_color_viridis()+
	ggplot2::labs(title = "HPCA Expression in Hippocampus")
dev.off()

## (2) Adding the tissue image
soObj <- addImageOmeTiff(overlay = soObj, ometiff = tiffFile, res = 6)

pdf(file.path(outDir, "14_HPCAexpr_tissue.pdf"))
plotSpatialOverlay(overlay = soObj, hiRes = FALSE, colorBy = "segment", scaleBar = FALSE)
dev.off()

##### ##### ##### ##### ##### ##### ##### ##### ##### #####
# -- * -- Step 5 -- * --
# Image manipulation
## (1) Flipping X-axis
pdf(file.path(outDir, "15_flipX.pdf"))
plotSpatialOverlay(overlay = flipX(soObj), hiRes = FALSE, colorBy = "segment", scaleBar = FALSE)
dev.off()

## (2) Flipping Y-axis
pdf(file.path(outDir, "15_flipY.pdf"))
plotSpatialOverlay(overlay = flipY(soObj), hiRes = FALSE, colorBy = "segment", scaleBar = FALSE)
dev.off()

## (3) Flipping X- and Y-aixs
pdf(file.path(outDir, "15_flipXY.pdf"))
plotSpatialOverlay(overlay = flipY(flipX(soObj)), hiRes = FALSE, colorBy = "segment", scaleBar = FALSE)
dev.off()

## (4) Cropping tissue
soObj <- cropTissue(overlay = soObj, buffer = 0.01)

pdf(file.path(outDir, "15_cropTissue.pdf"))
plotSpatialOverlay(overlay = soObj, colorBy = "ROILabel", legend = FALSE, scaleBar = FALSE)
dev.off()

## (5) Cropping sample(s)
soObjSubset <- cropSamples(overlay = soObj, sampleIDs = lwAnnot$Sample_ID) # all samples in this tutorial

pdf(file.path(outDir, "15_cropSamples.pdf"))
plotSpatialOverlay(overlay = soObjSubset, hiRes = FALSE, colorBy = "HPCA", scaleBar = FALSE) +
	viridis::scale_color_viridis()
dev.off()

##### ##### ##### ##### ##### ##### ##### ##### ##### #####
# -- * -- Step 6 -- * --
# Image recoloring
## (1) Current settings
fourChObj <- add4ChannelImage(overlay = soObj)
fluor(fourChObj)

pdf(file.path(outDir, "16_segmentCols.pdf"))
gp <- plotSpatialOverlay(overlay = fourChObj, hiRes = FALSE, colorBy = "segment", scaleBar = FALSE)
legend <- fluorLegend(fourChObj, nrow = 2, textSize = 4, boxColor = "black", alpha = 0.8)
cowplot::ggdraw() +
	cowplot::draw_plot(gp) +
	cowplot::draw_plot(legend, scale = 0.2, x = -0.3, y = -0.3)
dev.off()

## (2) Updating colors
fourChObj <- changeImageColoring(overlay = fourChObj, color = "cyan", dye = "Alexa 488")
fourChObj <- changeImageColoring(overlay = fourChObj, color = "gold", dye = "Alexa 594")
fourChObj <- changeImageColoring(overlay = fourChObj, color = "magenta", dye = "Alexa 647")
fluor(fourChObj)

pdf(file.path(outDir, "16_segmentCols_fixed.pdf"))
gp <- plotSpatialOverlay(overlay = fourChObj, hiRes = FALSE, colorBy = "segment", scaleBar = FALSE)
legend <- fluorLegend(fourChObj, nrow = 2, textSize = 4, boxColor = "black", alpha = 0.8)
cowplot::ggdraw() +
	cowplot::draw_plot(gp) +
	cowplot::draw_plot(legend, scale = 0.2, x = -0.3, y = -0.3)
dev.off()

q("no")