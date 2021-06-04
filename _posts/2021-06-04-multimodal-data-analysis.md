---
title: 'Visualize multi-modal data using Seurat V4 and MOFA'
date: 2021-06-04
permalink: /posts/2021/06/viz-multimodal-data/
tags:
  - visualization
  - multimodal
---

Recent advances in technology has enabled measuring multiple layers information from a single cell, resulting in so-called single cell multi-omic data, for example [SHARE-seq](https://www.sciencedirect.com/science/article/abs/pii/S0092867420312538), where the chromatin accessibility and gene expression from the exact same cells are quantified. One natural question would be how to analyze this kind of data. In particular, how to define the clusters and visualize them, given the fact that each modality captures different information and they have very different property in terms of the readout.

Two methods were developed for this propose, namely [Seurat V4](https://www.cell.com/cell/fulltext/S0092-8674(21)00583-3) and [MOFA+](https://genomebiology.biomedcentral.com/articles/10.1186/s13059-020-02015-1). In this blog, I would like to use them to analyze a single cell multi-omic data generated using 10x multiome protocol. The raw data can be found [here](https://support.10xgenomics.com/single-cell-multiome-atac-gex/datasets) and we will use the processed R object from [MOFA+ tutorial](https://raw.githack.com/bioFAM/MOFA2_tutorials/master/R_tutorials/10x_scRNA_scATAC.html).

First, let's import the required libraries:
```terminal
library(MOFA2)
library(Seurat)
```



