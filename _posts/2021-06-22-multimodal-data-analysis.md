---
title: 'Analysis of Single Cell Multi-modal Data: Part I'
date: 2021-06-22
permalink: /posts/2021/06/22/multimodal-data/
tags:
  - single cell
  - multimodal
---

## Introduction

Recent advances in technology has enabled measuring information of multiple layers from a single cell, resulting in so-called single cell multi-omic data, such as [SHARE-seq](https://www.sciencedirect.com/science/article/abs/pii/S0092867420312538) or [Multiome](https://www.10xgenomics.com/products/single-cell-multiome-atac-plus-gene-expression), where the chromatin accessibility and gene expression from the exact same cells are quantified. Given this kind of data, an interesting question is how to computational analyze them. In particular, how to perform dimensionality reduction, how to define the clusters and visualize the data, given the fact that each modality captures different information and they have very different property in terms of the readout.

Two methods were developed for this task, namely [Seurat V4](https://www.cell.com/cell/fulltext/S0092-8674(21)00583-3) and [MOFA+](https://genomebiology.biomedcentral.com/articles/10.1186/s13059-020-02015-1). In this blog series, I will use them to analyze a single cell multi-omic data generated using 10x multiome protocol. The raw data can be found [here](https://support.10xgenomics.com/single-cell-multiome-atac-gex/datasets) and I will use the processed R object from [MOFA+ tutorial](https://raw.githack.com/bioFAM/MOFA2_tutorials/master/R_tutorials/10x_scRNA_scATAC.html).

## WNN analysis and multiple kernel learning
Let's first go through the method to understand the basic idea. We begin with Seurat V4 because I have used the previous version of Seurat a lot in my projects and I am more familiar with it.

The core problem when processing multi-mode data is how to determine the similarity of cells, i.e., how to combine the distances estimated from multiple data types. Seurat V4 tried to solve this problem using weighted nearest neighbor (WNN) analysis. Specifically, given K data types and any two cells i and j, it estimates the similarity as follows:

<a href="https://www.codecogs.com/eqnedit.php?latex=\bg_white&space;\fn_cm&space;\LARGE&space;\theta(i,&space;j)&space;=&space;\sum_{k=1}^{K}&space;w_{k}(i)&space;\cdot&space;\theta_{k}(i,j)" target="_blank"><img src="https://latex.codecogs.com/gif.latex?\bg_white&space;\fn_cm&space;\LARGE&space;\theta(i,&space;j)&space;=&space;\sum_{k=1}^{K}&space;w_{k}(i)&space;\cdot&space;\theta_{k}(i,j)" title="\LARGE \theta(i, j) = \sum_{k=1}^{K} w_{k}(i) \cdot \theta_{k}(i,j)" /></a>

This formula reminds me of another paper, [SIMLR](https://www.nature.com/articles/nmeth.4207), which uses multiple kernel learning to learn a cell-cell similarity matrix, as shown below.

<img src="{{ site.url }}/assets/blog/multi_modal_analysis/SIMLR.webp" class="img-responsive">

The difference between WNN and SIMLR is how to calculate the weight parameter. In SIMLR, the weights are automatically learned from the data by solving an optimization problem and they are identical for all the cells. However, in WNN, each cell has a set of weight parameters and they are estimated in a supervised way. The assumption is that if a cell can be more accurately predicted by its neighbors using a modality than others, then this modality better captures the underlying cell-cell relationship for this cell and it deserves a higher weight in the final combination of similarities.

