---
layout: post
author: "subin"
title:  "RNAvelocyto ver.2"
subtitle: "Seurat to Velocyto using only R"
type: "Tool"
category: "singleCell"
tags: ['RNAvelocyto','RNA velocity','Seurat to RNAvelocity','scRNAseq','R']
disqus: true
---
지난번에 python과 R을 동시에 사용해서 RNA velocity analysis하는 법을 포스팅했었는데,  이번에는 `SeuratWrapper` libarary 사용해서 <u>python 없이 R만으로</u> 동일한 분석 하는 법을 정리해보려고 한다.
<br/>
<div class="bs-callout bs-callout-success">
<div markdown="1">
**이전 포스팅** 
- [RNAvelocyto: Estimations of RNA velocities of single cells by distinguishing unspliced and spliced mRNAs](https://sweebinee.github.io/blog/study/single-cell/singlecell/2021-03-22/RNA-velocyto)
</div></div>
**Velocyto** 사용해서 loom file 제작하는 것까지는 이전 포스팅의 방법과 동일하다. 이후에 Loom file을 R로 불러오고 seurat data의 umap데이터를 합쳐 projection하는 방법에 대해 작성해보고자한다. <br/>(*만약에 제 방법에 문제가 있거나 더 좋은 아이디어가 있다면 댓글로 알려주세요!!*)
<br/><br/>

- [Preprocessing velocity data](#preprocessing-velocity-data)
- [Running RNA Velocity](#running-rna-velocity)
- [Results](#results)
<br/><br/><br/>


:file_folder:**Files to prepare before starting analysis** 
- `Velocyto`돌려서 얻은 **loom file**
- **preprocessed**(filtering, dimension reduction, clustering, visualization) **Seurat data** : umap정보 가져와서 그 위에 velocity결과 얹어서 보려고 함.
<br/>

:wrench:**Prerequisites to install**
- [Seurat](https://satijalab.org/seurat/articles/install.html): install.packages('Seurat')
- [velocyto.R](http://velocyto.org/): library(devtools); install_github("velocyto-team/velocyto.R")
- [SeuratWrappers](https://github.com/satijalab/seurat-wrappers): remotes::install_github('satijalab/seurat-wrappers')


## Preprocessing velocity data
---
만든 loom file을 R로 불러들여 Seurat object로 변환하고 preprocessing과정을 거친다. 

```R
library(Seurat)
library(velocyto.R)
library(SeuratWrappers)
```
<br/>
```R
#Load loom files
sample1_ldat <- ReadVelocity(file = "s1.loom")
sample2_ldat <- ReadVelocity(file = "s2.loom")
..
#Load seurat data
seurat.obj <- readRDS("sample_seurat.rds")

#Get filtered cell ids from seurat data (optional)
sample1_cells <- sapply(colnames(seurat.obj)[grepl("(^(\\w*)-1$)",colnames(seurat.obj))], function(x) paste0("sample1:",substr(x,1,16),"x")) 
sample2_cells <- sapply(colnames(seurat.obj)[grepl("(^(\\w*)-2$)",colnames(seurat.obj))], function(x) paste0("sample2:",substr(x,1,16),"x")) 
```
:warning: 여기서 주의해야할 점은 cell id로 filtering을 진행하기 위해서는 seurat object의 cell id와 loom file로 얻은 데이터의 cell id가 동일해야 한다는 것이다. 내 경우 seurat object의 cell id와 loom file의 cell id의 패턴이 달라서 이를 변환해서 맞는 cell id만 골라 낼 수 있게 한과정을 더 추가해줬다. (굳이 변환해주지 않아도 둘이 동일하다면 skip해도 좋다)

| Seurat cell ids  |loom file cell ids|  
|:----------------:|:----------------:| 
|AAACCCAGTCCGATCG**-1**|**sample1:**AAACCCAGTCCGATCG**x**|
|AAACCCAGTGTTGCCG**-1**|**sample1:**AAACCCAGTGTTGCCG**x**|
|...|...|
|AAACCCATCAGGGTAG**-2**|**sample2:**AAACCCATCAGGGTAG**x**|
|AAACCCATCCGTAATG**-2**|**sampel2:**AAACCCATCCGTAATG**x**|

```R
#convert loom to seurat and cell filtering
sample1_seurat <- as.Seurat(sample1_ldat)[, sample1_cells]
sample2_seurat <- as.Seurat(sample2_ldat)[, sample2_cells]

#merge the sample files 
merged_seurat <- merge(sample1_seurat, sample2_seurat)

#preprocessing
merged_seurat <- SCTransform(object = merged_seurat, assay = "spliced")
merged_seurat <- RunPCA(object = merged_seurat, verbose = FALSE)
merged_seurat <- FindNeighbors(object = merged_seurat, dims = 1:20)
merged_seurat <- FindClusters(object = merged_seurat)
merged_seurat <- RunUMAP(object = merged_seurat, dims = 1:20) #나중에 UMI count로 먼저 계산했던 UMAP으로 덮어씌울 예정
```


## Running RNA velocity
---
`SeuratWrapper`로 RNA velocity계산하고 미리 UMI count로 계산했던 UMAP 결과를 velocity seurat object에 추가해 visualization해보자. (*이게 맞는 방법인진 확실하지 않음: integrated seurat object와 샘플별 velocity loom file을 단순히 merge해서 계산한 velocity 값을 이런식으로 얹어서 봐도 되는지..*)
```R
# Calculate RNA velocity
## 서버사용하면 ncores옵션 반드시 사용하기 안그러면 속터진다.
merged_seurat <- RunVelocity(object = merged_seurat, deltaT = 1, kCells = 25, fit.quantile = 0.02, ncores = 20) 

#add umap coordinate information calculated using UMI counts
numap <- as.matrix(Embeddings(seurat.obj, reduction = "umap"))
rownames(numap) <- rownames(merged_seurat@reductions[["umap"]]@cell.embeddings[,1:2])
merged_seurat@reductions[["numap"]] <- CreateDimReducObject(embeddings = numap, key = "UMAP_", assay = DefaultAssay(merged_seurat))

#visualization
#pdf("velocity_analysis.pdf")
show.velocity.on.embedding.cor(emb = Embeddings(object = merged_seurat, reduction = "numap"), vel = Tool(object = merged_seurat, 
    slot = "RunVelocity"), n = 200, scale = "sqrt", cell.colors = ac(x = cell.colors, alpha = 0.5), 
    cex = 0.8, arrow.scale = 3, show.grid.flow = TRUE, min.grid.cell.mass = 0.5, grid.n = 40, arrow.lwd = 1, 
    do.par = FALSE, cell.border.alpha = 0.1, n.cores=20)
#dev.off()

```

## Results


<br/><br/><br/><br/><br/><br/><br/><br/>
##### Reference list
- [Estimating RNA Velocity using Seurat](http://htmlpreview.github.io/?https://github.com/satijalab/seurat-wrappers/blob/master/docs/velocity.html)
