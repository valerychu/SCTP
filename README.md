# SCTP
Single-Cell and Tissue Phenotype prediction 

## Introduction

The SCTP R package contains the proposed SCTP (Single Cell Tissue Phenotype prediction) method. SCTP provides a valuable approach for analyzing and understanding the cellular malignancy within the tumor microenvironment from an innovative and integrative perspective by combining the essential information from the bulk sample phenotype, single cell composition and cellular special distribution, which would be overlooked in traditional tissue pathological slice. As an automated tissue phenotype prediction model, SCTP facilitates a more profound understanding of tumor microenvironments, enables quantitative characterization of cancer hallmarks, and elucidates the underlying complex molecular and cellular interplay. 

In this tutorial, we provide multiple examples to assist you in applying SCTP to real-world applications. It encompasses instructions for estimating the likelihood of colorectal cancer using a pre-trained model and guides on constructing a new SCTP model using datasets supplied by yourself.

## Installation

### Prerequisites

* python 3.9 and R 4.3.0

### Python environment and packages

Please set up a virtual environment named with "env_SCTP," ensuring it includes the required packages:

* numpy
* pytorch
* pytorch_geometric
* scikit-learn
* scipy

## Spots and cell malignancy prediction using SCTP-CRC model

In this section, we outline the procedure for utilizing the SCTP-CRC pretrained model (function `SCTP_CRC`) to evaluate cell or spot malignancy in your own datasets.

## Loading your dataset

The input data must be formatted as a Seurat object, particularly for spatial transcriptomics data, where examining the image component is highly recommended for visualization of the output. 

### Input as gene expression matrix

For single cell data, in cases where only the counts matrix is available, you could first use the function `Seurat_preprocess` to converted into a Seurat object. This function provide simplified preprocessing procedures and the output is a Seurat object.  

```{r}
counts <- read.csv(
  file="/Users/w435u/Documents/ST_SC/Method_Compare/data/IR/GSE115978_counts.csv",
  header=TRUE,
  row.names = 1
)
```

