---
layout: post
author: "subin"
title:  "Identifying cell populations with scRNASeq"
subtitle: "an overview of different exprimental protocols and the most popular methods for facilitating the computational analysis."
type: "Review"
category: "paper"
tags: ['scRNA-seq']
---
> Andrews, Tallulah S., and Martin Hemberg. ["Identifying cell populations with scRNASeq."](https://doi.org/10.1016/j.mam.2017.07.002) Molecular aspects of medicine 59 (2018): 114-122.

<br/>
- [Introduction](#1-intro)<br/>
- [**Experimental design considerations for scRNA-seq**](#2-experimental-design-considerations-for-scrna-seq)<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[Experimental protocols](#21-experimental-protocols)<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[Managing technical noise](#22-managing-technical-noise)<br/>

- [**Strategies for dealing with high dimensionality**](#3-strategies-for-dealing-with-high-dimensionality)<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[Dimensionality reduction](#31-dimensionality-reduction)<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[Principal componet analysis](#principal-component-analysis-pca)<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[T-distributed stochastic neighbor embedding](#tdistributed-stochastic-neighbor-embedding-tsne)<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[Diffusion maps](#diffusion-maps-dm)<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[Feature selection](#32-feature-selection)<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[Michaelis-Menten modelling of dropouts](#michaelis-menten-modelling-of-dropouts-m3drop)<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[Highly variable genes](#highly-variable-genes-hvg)<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[Spike-in based methods](#spikein-based-methods)<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[Correlated expression](#correlated-expression)<br/>

- [**Unsupervised clustering methods for identification of cell populations**](#4-unsupervised-clustering-methods-for-identification-of-cell-populations)<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[K-means](#kmeans)<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[Hierarchical clustering](#hierarchical-clustering)<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[Density-based clustering](#densitybased-clustering)<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[Graph clustering](#graph-clustering)<br/>

- [**Biological characterization of clusters**](#5-biological-characterization-of-clusters)<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[Computational approaches](#51-computational-approaches)<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[Experimental approaches](#52-experimental-approaches)<br/>

- [Closing remarks](#6-closing-remarks)


# 1. Intro
# 2. Experimental design considerations for scRNA-seq
## 2.1. Experimental protocols
## 2.2. Managing technical noise
# 3. Strategies for dealing with high dimensionality
## 3.1. Dimensionality reduction
### Principal component analysis (PCA) 
### T-distributed stochastic neighbor embedding (tSNE)
### Diffusion maps (DM)
## 3.2. Feature selection
### Michaelis-Menten modelling of dropouts (M3Drop)
### Highly variable genes (HVG) 
### Spike-in based methods
### Correlated expression 
# 4. Unsupervised clustering methods for identification of cell populations
### K-means
### Hierarchical clustering
### Density-based clustering 
### Graph clustering
# 5. Biological characterization of clusters
## 5.1. Computational approaches
## 5.2. Experimental approaches
# 6. Closing remarks



