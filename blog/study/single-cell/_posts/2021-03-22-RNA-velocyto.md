---
layout: post
author: "subin"
title:  "RNAvelocyto"
subtitle: "Estimations of RNA velocities of single cells by distinguishing unspliced and spliced mRNAs"
type: "Tool"
category: "singleCell"
tags: ['RNAvelocyto','RNA velocity','Seurat to RNAvelocity','scRNAseq','Python','R']
disqus: true
---
- [Introduction](#introduction)
- [Tutorial](#tutorial)
  * [Generating Loom files](#generating-loom-files)
    + [Velocyto](#velocyto)
      + [Installation](#installation)
      + [Usage](#usage)
  * [Extracting Meta-data From a Seurat Object](#extracting-meta-data)
  * [Integrating Loom File and Meta-data](#integrating-loom-file-and-meta-data)
    + [Multiple-Sample Integration](#multi-samples-integration)
  * [Running RNA Velocity](#running-rna-velocity)



# Introduction
RNA velocity는 **시간단위로 각 세포의 미래상태를 예측해주는 high-dimensional vector**로, 한 시점의 snapshot만을 보여주는 기존의 single cell RNA seq 데이터의 특징과 분석의 한계를 극복하기 위한 방법이다.[^1] velocity 계산은 unspliced read(precurosr mRNA)와 spliced read(masture mRNA)의 양 측정을 통해 이루어진다고 한다. 자세한건 velocyto 논문리뷰에서..[^2]

2018년에 velocyto 논문을 통해서 RNA velocity 분석이 소개가 된 이후로 scVelo[^3], VeloSim같은 tool들도 나오고 있는것 같지만 아직까지는 velocyto를 많이 사용하는것 같다. SeuratWrapper를 통해서 seurat object로도 velocity 분석이 가능한데, 미리 정량한 RNA (spliced/unspliced counts, total counts etc.)정보를 seurat으로 불러들여서 재분석(normalize, dimension reduction, clustering)하고 visualization까지 하는 방법인듯..[^4]<br/>

*나는 지금까지 분석해온 seurat object가 있으니 velocity를 계산하고 얹어서 같이 보는 방법을 정리해보려고 한다.*<br/>
:exclamation:주의해야할 점은 Seurat은 R-based이고, 이제부터 진행할 Velocyto는 python-based program 이라는점! 두 언어를 왔다갔다 할거다!!
<div class="bs-callout bs-callout-default">
<div markdown="1">
다음과 같은 tool을 사용할 예정:
- [Seurat](https://satijalab.org/seurat/)<br/>
- [Velocyto](http://velocyto.org/)<br/>
- Samtools(optional)<br/>
- [Anndata](https://anndata.readthedocs.io/en/stable/)<br/>
</div></div>


# Tutorial
## Generating Loom files
---
loom file만들어야한다. 


### :apple: Velocyto
**RNAvelocyto**는 앞에서도 말했지만 unspliced와 spliced mRNAs를 구분해서 RNA velocity를 계산해주는 tool이다.

### Installation

<div class="bs-callout bs-callout-default">
<div markdown="1">
**Dependencies**
- python >= 3.6.0 (3.5이하는 지원 안함)<br/>
- [anaconda](https://sweebinee.github.io/blog/study/tools/2021-03-22/Anaconda)로 설치하는 것 추천 (dependency-managing issue)<br/>
- [samtools](https://sweebinee.github.io/blog/study/tools/2021-03-22/Samtools) >= 1.6 <br/>
</div></div>

```bash
#dependencies
conda install numpy scipy cython numba matplotlib scikit-learn h5py click
#install velocyto
pip install velocyto
```

### Usage
Velocyto는 두가지 구성요소로 이루어져 있음.
-  **Command line interface(CLI)**, spliced/unspliced expression matrices를 만드는 파이프라인을 돌릴때 사용.
-  **A library**, CLI로 만든 expression matrices에서 RNA velocity 측정하는 function을 포함.

#### Running CLI[^5]
<div class="bs-callout bs-callout-default">
<div markdown="1">
**돌리기 전에 준비물** 
- <u>genome annotation file</u><br/> .gtf file을 준비한다.(분석하는 종, 분석에 사용한 reference 버전에 맞춰서)<br/> cellranger pipeline을 사용했다면 그때 사용했던 gene/gene.gtf 파일을 사용하면 된다. 다운로드는 [여기](https://support.10xgenomics.com/single-cell-gene-expression/software/pipelines/latest/advanced/references) 
- <u>expressed repeats annoation</u> (optional)<br/>
</div></div>

#### Running `velocyto`
`velocyto run` 으로 기본적인 pipeline을 돌릴 수 있는데, 사람들이 많이 사용하는 scRNA-seq chemistry는 redy-to-use subcommand를 만들어놨다고 한다. 가능한 옵션은 다음과 같다.
<div class="bs-callout bs-callout-default">
<div markdown="1">
 `run10x`: Run on 10X Chromium samples <br/>
 `run_smartseq2`: Run on SmartSeq2 samples<br/>
 `run_dropest`: Run on DropSeq, InDrops and other techniques<br/>
 `run`: Run on any technique (Advanced use)
</div></div>

*나는 10X로 생산한 data를 분석할거라 `run10x`로 진행!*

돌리기 전에 다음과 같이 설정해준다. (.bash_profile에 넣던지, 아니면 그냥 bash에서 한번 돌려준다.)
```bash
export HDF5_USE_FILE_LOCKING='FALSE'
```
<details>
<summary>안그러면 이런 에러가 난다.</summary>
<div markdown="1">
거의 2시간쯤 후.. 다 돌아가서 결과 loom file writing 하는 와중에 발생한다.[^6]

```bash
2021-03-22 19:27:45,677 - DEBUG - Writing loom file
Traceback (most recent call last):
  File "/storage2/Project/subin/source/Anaconda3/lib/python3.8/site-packages/velocyto/commands/_run.py", line 286, in _run
    ds = loompy.create(filename=outfile, matrix=total, row_attrs=ra, col_attrs=ca, dtype="float32")
TypeError: create() got an unexpected keyword argument 'matrix'

During handling of the above exception, another exception occurred:

Traceback (most recent call last):
  File "/storage2/Project/subin/source/Anaconda3/bin/velocyto", line 8, in <module>
    sys.exit(cli())
  File "/storage2/Project/subin/source/Anaconda3/lib/python3.8/site-packages/click/core.py", line 829, in __call__
    return self.main(*args, **kwargs)
  File "/storage2/Project/subin/source/Anaconda3/lib/python3.8/site-packages/click/core.py", line 782, in main
    rv = self.invoke(ctx)
  File "/storage2/Project/subin/source/Anaconda3/lib/python3.8/site-packages/click/core.py", line 1259, in invoke
    return _process_result(sub_ctx.command.invoke(sub_ctx))
  File "/storage2/Project/subin/source/Anaconda3/lib/python3.8/site-packages/click/core.py", line 1066, in invoke
    return ctx.invoke(self.callback, **ctx.params)
  File "/storage2/Project/subin/source/Anaconda3/lib/python3.8/site-packages/click/core.py", line 610, in invoke
    return callback(*args, **kwargs)
  File "/storage2/Project/subin/source/Anaconda3/lib/python3.8/site-packages/velocyto/commands/run10x.py", line 112, in run10x
    return _run(bamfile=(bamfile, ), gtffile=gtffile, bcfile=bcfile, outputfolder=outputfolder,
  File "/storage2/Project/subin/source/Anaconda3/lib/python3.8/site-packages/velocyto/commands/_run.py", line 297, in _run
    loompy.create(filename=outfile, layers=tmp_layers, row_attrs=ra, col_attrs=ca, file_attrs={"velocyto.__version__": vcy.__version__, "velocyto.logic": logic})
  File "/storage2/Project/subin/source/Anaconda3/lib/python3.8/site-packages/loompy/loompy.py", line 1057, in create
    with new(filename, file_attrs=file_attrs) as ds:
  File "/storage2/Project/subin/source/Anaconda3/lib/python3.8/site-packages/loompy/loompy.py", line 983, in new
    f = h5py.File(name=filename, mode='w')
  File "/storage2/Project/subin/source/Anaconda3/lib/python3.8/site-packages/h5py/_hl/files.py", line 424, in __init__
    fid = make_fid(name, mode, userblock_size,
  File "/storage2/Project/subin/source/Anaconda3/lib/python3.8/site-packages/h5py/_hl/files.py", line 196, in make_fid
    fid = h5f.create(name, h5f.ACC_TRUNC, fapl=fapl, fcpl=fcpl)
  File "h5py/_objects.pyx", line 54, in h5py._objects.with_phil.wrapper
  File "h5py/_objects.pyx", line 55, in h5py._objects.with_phil.wrapper
  File "h5py/h5f.pyx", line 116, in h5py.h5f.create
OSError: Unable to create file (file locking disabled on this file system (use HDF5_USE_FILE_LOCKING environment variable to override), errno = 38, error message = 'Function not implemented')
```
</div>
</details>
<br/>

```bash
#Usage: velocyto run10x [OPTIONS] SAMPLEFOLDER GTFFILE
velocyto run10x /scRNAseq/02_Preprocessing/SW480/ /cellranger-5.0.1/refdata-gex-GRCh38-2020-A/genes/genes.gtf
velocyto run10x /scRNAseq/02_Preprocessing/SW620/ /cellranger-5.0.1/refdata-gex-GRCh38-2020-A/genes/genes.gtf
```

<div class="bs-callout bs-callout-warning">
<div markdown="1">
<h4>:construction: 만약 여러개의 데이터를 통합분석할 예정이라면,</h4>
**샘플별로 따로 `velocyto run`을 진행한 후 나중에 합쳐줘야한다!** arg에 들어가는 `SAMPLEFOLDER`의 하위폴더로 `outs`, `outs/analys` and `outs/filtered_gene_bc_matrices`가 있어야하기 때문! Cellranger에서 `aggr`을 진행하면 폴더구성이 저것과 달라서 aggr결과 폴더를 input으로 주면 에러남.
</div></div>

`--samtools-threads`와 `--samtools-memory` 옵션으로 parallelization 조정가능. 
<br/><br/>

끝나면, `SAMPLEFOLDER/velocyto` folder아래 loom file이 만들어진다. 다음 단계로 넘어가기 전에 filtering과 후속분석에 필요한 meta data를 seurat object에서 뽑아 따로 저장한다.

## Extracting meta-data
---
<div class="bs-callout bs-callout-default">
<div markdown="1">
**뽑아낼 meta-data**
- Filtered cell ids
- UMAP/tSNE coordinates
- Clusters
- Cluster colors(optional)
</div></div>

```R
#in R
library(Seurat)
seurat.obj <- readRDS("preprocessed_seurat_obj.rds")

#Filtered cell ids
write.csv(Cells(seurat.obj), file = "cellID_obs.csv", row.names = FALSE)
#UMAP coordinates
write.csv(Embeddings(seurat.obj, reduction = "umap"), file = "cell_embeddings.csv")
#Clusters
write.csv(seurat.obj@meta.data$seurat_clusters, file = "clusters.csv")
```

## Integrating loom file and meta-data
---
아까 velocyto로 만들었던 loom file에 seurat에서 뽑아온 meta-data를 합쳐준다.
```python
#in python3
import anndata
import scvelo as scv
import pandas as pd
import numpy as np
import matplotlib as plt

#load loom files
sample1 = anndata.read_loom("sample1.loom")
sample2 = anndata.read_loom("sample2.loom")

#load meta-data
sample_obs = pd.read_csv("cellID_obs.csv")
umap_cord = pd.read_csv("cell_embeddings.csv")
cell_clusters = pd.read_csv("clusters.csv")
```
<br/>
if, sample이 여러개가 아니라 sample1뿐이라면..<br/>
다음처럼 cell filtering하고 RNA velocity계산하는 단계로 넘어가면 됨.
```python
sample1 = sample1[np.isin(sample1.obs.index,sample_obs["x"])]
```
### Multi-samples integration
sample이 n개라면, 각 샘플별로 cell filtering해주고 다시 하나로 합쳐서 준비해준다.

*내 데이터는 뒤에 라벨이 붙은 형식으로 생겨서 이렇게 처리해줬다.*

| Cell ID_obs | 
| ---------------- | 
|AAACCCAGTCCGATCG-1|
|AAACCCAGTGTTGCCG-1|
|...|
|AAACCCATCAGGGTAG-2|
|AAACCCATCCGTAATG-2|

```python
cellID_obs_sample1 = sample_obs[sample_obs["x"].str.contains("-1")]
cellID_obs_sample2 = sample_obs[sample_obs["x"].str.contains("-2")]

#cell filtering
sample1 = sample1[np.isin(sample1.obs.index, cellID_obs_sample1)]
sample2 = sample2[np.isin(sample2.obs.index, cellID_obs_sample2)]

#merge the files into one
sample = np.concatenate(sample1,sample2)
```
<br/><br/>
여기까지 했으면 filtered velocity file에 seurat의 umap정보를 얹을 차례. 그전에 두 file에서의 cell id 순서를 동일하게 맞춰준다.
```python
sample_index = pd.DataFrame(sample.obs.index)
sample = sample_index.rename(columns = {0:'Cell ID'})

umap = umap_cord.rename(Columns = {'Unnamed: 0':'Cell ID'})

umap_ordered = sample_index.merge(umap, on = "Cell ID")

umap_ordered = umap_ordered.iloc[:,1:]
sample.obsm['X_umap'] = umap_ordered.values

#add Clusters and their cluster colors
sample.uns['Cluster_colors']
```

## Running RNA velocity
---
```python
scv.pp.filter_and_normalize(sample)
scv.pp.moments(sample)
scv.tl.velocity(sample, mode = "stochastic")
scv.tl.velocity_graph(sample)
scv.pl.velocity_embedding(sample, basis = 'umap')

```


<br/><br/><br/><br/><br/><br/><br/><br/>


Tutorial reference: [Seurat to RNA velocity](https://github.com/basilkhuder/Seurat-to-RNA-Velocity/blob/master/readme.MD)

[^1]: La Manno, Gioele, et al. ["RNA velocity of single cells."](https://doi.org/10.1038/s41586-018-0414-6) Nature 560.7719 (2018): 494-498.
[^2]: 논문 리뷰페이지()
[^3]: Bergen, Volker, et al. ["Generalizing RNA velocity to transient cell states through dynamical modeling."](https://www.nature.com/articles/s41587-020-0591-3) Nature biotechnology 38.12 (2020): 1408-1414.
[^4]: [Estimating RNA Velocity using Seurat](https://github.com/satijalab/seurat-wrappers/blob/master/docs/velocity.md)
[^5]: [Running Velocyto CLI](http://velocyto.org/velocyto.py/tutorial/index.html)
[^6]: [OSError: Unable to create file](https://github.com/qqwweee/keras-yolo3/issues/443)