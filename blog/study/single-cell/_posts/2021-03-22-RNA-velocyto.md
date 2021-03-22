---
layout: post
author: "subin"
title:  "RNAvelocyto"
subtitle: "Estimations of RNA velocities of single cells by distinguishing unspliced and spliced mRNAs"
type: "Tool"
category: "singleCell"
tags: ['RNAvelocyto','RNA velocity']
disqus: true
---
**RNAvelocyto**는 unspliced와 spliced mRNAs를 구분해서  RNA velocity를 계산해주는 tool이다.

> **Publication**
> 
> La Manno, Gioele, et al. "[RNA velocity of single cells.](https://doi.org/10.1038/s41586-018-0414-6)" Nature 560.7719 (2018): 494-498. 
>
>website: http://velocyto.org/

`pyhon`과 `R`, 두 가지 언어로 실행할 수 있다.<br/>  

SeuratWrapper를 통해서 seurat object로 비슷한 분석이 가능한데, 미리 계산한 RNA velocity 정보를 seurat으로 불러들여서 재분석 하고 visualization까지 하는 방법인듯..[^1]<br/>
예시는 한 데이터만 갖고 하는 방법이고 여기서는 여러개의 dataset의 velocity 계산하고 seurat으로 다시 엮어보자.  

*내가 원래 원했던 것은..*<br/>
*이미 integrated seurat data갖고 있고 여기에 velocity data 추가할 수 없을까? 했는데.. 잘 모르겠음.* 

- [Installation](#installation)<br/>
- [Tutorial](#tutorial)<br/>


## Installation
- python >= 3.6.0 (3.5이하는 지원 안함)
- [anaconda](https://sweebinee.github.io/blog/study/tools/2021-03-22/Anaconda)로 설치하는 것 추천 (dependency-managing issue)
- [samtools](https://sweebinee.github.io/blog/study/tools/2021-03-22/Samtools) >= 1.6 

```bash
conda install numpy scipy cython numba matplotlib scikit-learn h5py click
pip install velocyto
```

## Tutorial
Velocyto는 두가지 구성요소로 이루어져 있음.
-  **Command line interface(CLI)**, spliced/unspliced expression matrices를 만드는 파이프라인을 돌릴때 사용.
-  **A library**, CLI로 만든 expression matrices에서 RNA velocity 측정하는 function을 포함.

### Running CLI[^2]
돌리기 전에 준비물
- genome annotation file<br/> .gtf file을 준비한다.(분석하는 종, 분석에 사용한 reference 버전에 맞춰서)<br/> cellranger pipeline을 사용했다면 그때 사용했던 gene/gene.gtf 파일을 사용하면 된다. 다운로드는 [여기](https://support.10xgenomics.com/single-cell-gene-expression/software/pipelines/latest/advanced/references) 
- expressed repeats annoation (optional)
<br/>
#### Running `velocyto` 
`velocyto run` 으로 기본적인 pipeline을 돌릴 수 있는데, 사람들이 많이 사용하는 scRNA-seq chemistry는 redy-to-use subcommand를 만들어놨다고 한다. 가능한 옵션은 다음과 같다.
- `run10x`: Run on 10X Chromium samples
- `run_smartseq2`: Run on SmartSeq2 samples
- `run_dropest`: Run on DropSeq, InDrops and other techniques
- `run`: Run on any technique (Advanced use)
<br/>

*나는 10X로 생산한 data를 분석할거라 `run10x`로 진행!*

```bash
#Usage: velocyto run10x [OPTIONS] SAMPLEFOLDER GTFFILE
velocyto run10x /scRNAseq/02_Preprocessing/SW480/ /cellranger-5.0.1/refdata-gex-GRCh38-2020-A/genes/genes.gtf
velocyto run10x /scRNAseq/02_Preprocessing/SW620/ /cellranger-5.0.1/refdata-gex-GRCh38-2020-A/genes/genes.gtf
```
만약에 **여러개의 데이터는 통합분석할 예정이라면 샘플별로 따로 `velocyto run`을 진행한 후 나중에 합쳐줘야한다!** arg에 들어가는 `SAMPLEFOLDER`의 하위폴더로 `outs`, `outs/analys` and `outs/filtered_gene_bc_matrices`가 있어야하기 때문! Cellranger에서 `aggr`을 진행하면 폴더구성이 저것과 달라서 aggr결과 폴더를 input으로 주면 에러남.  
<br/> 
`--samtools-threads`와 `--samtools-memory` 옵션으로 parallelization 조정가능. 

### Estimating RNA velocity


[^1]: [Estimating RNA Velocity using Seurat](https://github.com/satijalab/seurat-wrappers/blob/master/docs/velocity.md)
[^2]: [Running Velocyto CLI](http://velocyto.org/velocyto.py/tutorial/index.html)