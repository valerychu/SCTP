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
In this scRNA-seq dataset, each row represents a gene and each column represents a cell. The dimensions of this single-cell data are:

```{r}
dim(counts)
```

which indicates there are 23,686 genes and 7,186 cells in total. We use the functions provided from the Seurat package to preprocess this data. To simplify the process, we wrapped the Seurat analysis pipeline into the following function:

```{r warning=FALSE}
sc_dataset <- Seurat_preprocess(counts, verbose = F, type="SC")
```
The output is a Seurat object that contains the required preprocessed counts matrix, as well as other helpful dimensionality reduction results, such as the PCA, t-SNE, and UMAP. 

```{r}
names(sc_dataset)
```
For the diversity of spatial transcriptomic formats, automatic preprocessing is unavailable from this package. You must initially process your data to create a Seurat object, which should include the SCT-normalized counts matrix and the image data.

### Input as a seurat object

Alternatively, you can also provide a Seurat object using your own pipeline, but at least a normalized data (assays$RNA@data) is required. Below we show examples with single-cell RNA-seq data (sc_dataset) and spatial transcriptomic data (st_dataset) respectively.

```{r}
load("/Users/w435u/Documents/ST_SC/DATA_STSC_CAO/Seurat_L1.RData") #st_dataset
```

Check on single cell data for required information.

```{r}
!is.null(sc_dataset@assays$RNA@data)
```

Check on spatial transcriptomic data for required information.

```{r}
!is.null(st_dataset@assays$SCT$data)
```

## Malignancy prediction with SCTP-CRC model


### Prediciton for spatial transcriptomic data

We present an example using spatial transcriptomic data for prediction. Utilizing a preloaded ST Seurat object, we employ the `SCTP_CRC` function to predict the likelihood of tumor presence in each spot.


```{r}
st_dataset <- SCTP_CRC(my_seurat = st_dataset)
```

Same as single-cell data input, the predicted malignancy of each spot is stored in the annotation "malignancy' in the output Seurat object.

```{r}
names(st_dataset@meta.data)
```

You can then visualize by SpatialFeaturePlot for spatial transcriptomic data. Value closer to 1 indicates higher malignancy of the corresponding spots, while value close to 0 indicates normal state. 

```{r}
SpatialFeaturePlot(st_dataset, features = "malignancy")+
  scale_fill_gradientn(colours = col_mal)
```

