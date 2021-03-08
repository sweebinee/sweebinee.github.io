---
layout: post
author: "subin"
title:  "공개 데이터 다운받아서 재분석 하는 방법"
subtitle: "How to reanalyze public single cell sequencing datasets."
type: "Basic"
category: "singleCell"
tags: ['public single-cell RNA seq','scRNAseq database']
disqus: true
---

기본 분석 툴: `Seurat` 사용 가정

## Raw data(.fastq) 제공하는 경우

## Cellranger results(barcodes.tsv, gene.tsv, matrix.mtx) 제공하는 경우
기본적으로 cellrnager 이후 seurat object생성하는 방법과 동일하게 진행한다.
:point_right: [SCP1038](https://singlecell.broadinstitute.org/single_cell/study/SCP1038/the-human-and-mouse-enteric-nervous-system-at-single-cell-resolution#study-download) single cell nucleus RNA seq dataset
- Download Mouse ileum all cells (10X) data and processed data(tSNE & clustering results)<br/>
 `msi.barcodes.tsv`, `msi.genes.tsv`, `gene_sorted-msi.matrix.mtx`, `msi.tsne2.txt`<br/>
Change the file name. cellranger결과로 나온 것과 동일하게 `barcodes.tsv`,`genes.tsv`,`matrix.txt` 로 변경.
```bash
mv msi.barcodes.tsv barcodes.tsv
mv gene_sorted-msi.matrix.mtx matrix.mtx
mv msi.genes.tsv genes.tsv
```
- 







&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; **RNA** assay **counts** slot으로 subset떼와서 <u>scTransform다시 진행.</u> <br/>
:link: [Seurat Issue #2014 'SCTransform on subsets'](https://github.com/satijalab/seurat/issues/2014)

### :poop: subset() 사용할때 원하는 assay정보만 떼오는 기능은 없나? 
괜히 후속 분석할때 불안한데, RNA assay만 subsetting해올 순 없을까?