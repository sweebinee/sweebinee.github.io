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
- Make seurat object<br/>
```R
SCP.data <- Read10X("/SCP_data_download_dir_location")
SCP <- CreateSeuratObject(counts = SCP.data, project = "SCP1038")

> SCP
#An object of class Seurat 
#22595 features across 79293 samples within 1 assay 
#Active assay: RNA (22595 features, 0 variable features)
```
:inerrobang: 
<details>
<summary> Error in '[.data.frame'(feature.names, , gene.column) : </summary>
<div markdown="1">
`Read10X` 불러들여오는데 다음과 같이 error가 난다면 genes.tsv 파일을 열어서 column이 몇개인지 확인해보자. 한 column밖에 없다면 `gene.column=1` 옵션을 추가해주자.
```R
SCP.data <- Read10X("/SCP_data_download_dir_location")
Error in `[.data.frame`(feature.names, , gene.column) : 
  undefined columns selected
```
```bash
$ head genes.tsv
0610007P14Rik
0610009B22Rik
0610009L18Rik
0610009O20Rik
0610010F05Rik
0610012D04Rik
0610012G03Rik
0610025J13Rik
0610030E20Rik
0610031O16Rik
```
</div>
</details>
<br/>


