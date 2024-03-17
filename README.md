# Exploring variations across colorectal cancer metastasis patterns in a spatial context
Susie Avagyan, Perla Molina, and Sonia Bustos Barocio

## Abstract
Colorectal cancer (CRC) poses a global health challenge, ranking among the most diagnosed cancers with high mortality rates. Understanding its genetic complexity and metastatic potential is crucial for prognosis and treatment. Leveraging single-cell and spatial transcriptomics data, our study explores spatial neighborhoods in primary CRC tumors. Integrated analysis reveals differential cellular composition and transcriptional profiles, shedding light on spatial determinants of CRC metastasis, vital for improving patient outcomes.

## Cell2Location Implementation
Cell2Location is a Bayesian model designed to delineate fine-grained cell types by leveraging single-cell RNA-seq and spatial transcriptomics data [1]. Implemented using Python in Google Colab, it offers a powerful tool for understanding cellular composition in complex tissues. We followed the tutorials from https://github.com/BayraktarLab/cell2location?tab=readme-ov-file and https://cell2location.readthedocs.io/en/latest/notebooks/cell2location_tutorial.html.

Reference cell type signatures were generated from single-cell RNA-seq data using Cell2Location's model functions. A regression model was created and trained for 250 epochs, filtering genes and performing quality control before training. This is in the file under the Cell2Location folder: Pt.1 Belgian_Cohort_ExpressionSigs.ipynb.

Cell2Location performs deconvolution based on generated gene expression cell type signatures. This process involved processing CRC 10x Visium samples, identifying shared genes, and subsetting spatial transcriptomics data and reference signatures. The model was trained with parameters controlling tissue-dependent cell abundance and normalization of RNA detection variation. This is in the file under the Cell2Location folder: Pt.2 Belgian_Cohort_Decovolution.ipynb.

Spatial neighborhoods within primary CRC tumors were identified using matrix factorization (NMF) with the 'CoLocatedGroupsSklearnNMF' model. Employing 5 and 6 factors, different cellular compartments were captured. The analysis was conducted separately for tumor regions in lymph node and liver metastasized samples, as well as tumor-stroma interfaces in both metastases. This is in the file under the Cell2Location folder: Pt.3 Belgian_Cohort_Analysis.ipynb.

## CytoSPACE Implementation
CytoSPACE is an optimization method for mapping individual cells from a single-cell RNA sequencing atlas to spatial expression profiles. It resolves the task of assigning single cells/spots as a convex optimization challenge using the Jonker-Volgenant shortest augmenting path algorithm. [2]. 

We ran CytoSPACE with default parameters following guidelines specified in the tool's GitHub page (https://github.com/digitalcytometry/cytospace/blob/main/README.md). The output included estimated cell type fractions for each spot, along with spatially detailed single-cell RNA sequencing data.

The cell type fractions output was merged with sample annotations. We utilized the dplyr and ggplot2 packages to aggregate and plot sample and metastatic group level cell type fractions. The single-cell RNA seq data was processed using the Seurat R package.

Technical replicates were merged for each sample into one Seurat object, and pathology annotations were added to the metadata. Each object was labeled by a separate grouping variable indicating metastatic group. Subsequently, all samples were merged into one Seurat object.

The object was subsetted to each cell type, processed using SCTransform with default parameters, and prepared for differential expression analysis using the PrepSCTFindMarkers function. Differentially expressed genes were identified for each pathological region separately, using the FindMarkers function without recorrecting for UMI counts.

The found markers were plotted to identify significantly differentially expressed genes using a significance threshold of 0.01 and a logFC threshold of 1.

## References
[1] Kleshchevnikov, V., Shmatko, A., Dann, E. et al. Cell2location maps fine-grained cell types in spatial transcriptomics. Nat Biotechnol 40, 661–671 (2022). https://doi.org/10.1038/s41587-021-01139-4
[2] Vahid, M.R., Brown, E.L., Steen, C.B. et al. High-resolution alignment of single-cell and spatial transcriptomes with CytoSPACE. Nat Biotechnol 41, 1543–1548 (2023). https://doi.org/10.1038/s41587-023-01697-9
