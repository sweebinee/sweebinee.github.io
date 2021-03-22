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
> La Manno, Gioele, et al. "[RNA velocity of single cells.](https://doi.org/10.1038/s41586-018-0414-6)" Nature 560.7719 (2018): 494-498. 
>
>website: http://velocyto.org/

pyhon과 R, 두 가지 언어로 실행할 수 있다.  
SeuratWrapper를 통해서 seurat object로 비슷한 분석이 가능한데, 미리 계산한 RNA velocity 정보를 seurat으로 불러들여서 재분석 하고 visualization까지 하는 방법인듯..[^1]<br/>
예시는 한 데이터만 갖고 하는 방법이고 여기서는 여러개의 dataset의 velocity 계산하고 seurat으로 다시 엮어보자.

*내가 원래 원했던 것은..*<br/>
*이미 integrated seurat data갖고 있고 여기에 velocity data 추가할 수 없을까? 했는데.. 잘 모르겠음.* 

- [Installation](#installation)<br/>
- [Tutorial](#tutorial)<br/>


## Installation
- python >= 3.6.0 (3.5이하는 지원 안함)
- [anaconda]()로 설치하는 것 추천 (dependency-managing issue)
- 
```bash
conda install numpy scipy cython numba matplotlib scikit-learn h5py click
pip install velocyto
```

## Tutorial


[^1]: [Estimating RNA Velocity using Seurat](https://github.com/satijalab/seurat-wrappers/blob/master/docs/velocity.md)