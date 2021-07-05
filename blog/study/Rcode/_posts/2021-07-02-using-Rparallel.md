---
layout: post
author: "subin"
title:  "Processing with multi cores in R"
subtitle: "Rparallel library"
type: "code"
category: "Rcode"
tags: ['R','Rparallel','using R with multi cores environment','parallel processing']
disqus: true
post-header: true
header-img: https://user-images.githubusercontent.com/43258282/116018464-06321380-a67d-11eb-9975-f742c1f2b5fe.png
---
서버에서 R연산을 prarllel하게 진행할 수 있다. 좀 더 빠르고 core에 무리가 가지 않는 방법. single cell data처리하면서 cell 수나 데이터 사이즈가 커지고 복잡한 연산을 시키면 run time이 기하급수적으로 늘어나서 병렬처리가 필수적인 경우가 있다. 물론 multi core로 해결이 될 문제인지 memory문제인지는 좀 다르지만.. <br/>

# 전체 코드
```R
library(parallel)
library(pryr) 
library(doParallel)

# 코어 개수 획득
numCores <- parallel::detectCores() - 1

# 클러스터 초기화
myCluster <- parallel::makeCluster(numCores)
doParallel::registerDoParallel(myCluster)

# 변수 할당
parallel::clusterEvalQ(myCluster, library(monocle))
parallel::clusterExport(myCluster, varlist=c("cds"))

# CPU 병렬처리
clustering_DEG_genes <- foreach(x="cds")  %dopar% {
  tryCatch({
       differentialGeneTest(get(x), fullModelFormulaStr = '~detailedDV_cluster', verbose = TRUE, cores=60)  
    }, error=function(e){
      return(paste0("The variable '", x, "'", "or = function(e) { caused the error: '", e, "'"))
    })
}
saveRDS(clustering_DEG_genes,"cds_NPtoMDMV_clustering_DEG_genes.rds")

# 클러스터 해산
parallel::stopCluster(myCluster)
```

--- 

## 클러스터 구성
### 코어 수 구성
```R
numCores <- parallel::detectCores() - 1
```
`parallel::detectCores()` 전체 사용가능한 코어 수를 감지하고, 기본 작업을 하기 위한 코어 하나를 빼서 구성한다.
### 클러스터 초기화
```R
myCluster <- parallel::makeCluster(numCores)
doParallel::registerDoParallel(myCluster)
```
방금 구한 코어수만큼의 클러스터를 생성하고 등록함.
### 변수 할당
```R
parallel::clusterEvalQ(myCluster, library(monocle))
parallel::clusterExport(myCluster, varlist=c("cds"))
```
병렬처리 하기 위해서는 그 안에서 사용할 변수와 라이브러리를 따로 넣어줘야한다. `foreach`외부와 내부의 변수는 공유되지 않는다고 한다. 

## 병렬처리
다양한 방법이 있는데 `foreach`문을 가장 많이 사용하는것 같다.
```R
foreach(i=1:20, etc..)%dopar%{
	수행할 코드
}
``` 
for문과 비슷한 구조를 갖는다. 원래는 카운터 + 범위를 설정하고 그만큼 for loop을 각각 core에서 돌려 합치는 방식으로 운영된다.
- `%dopar%`는 단일코어에서 어떤 작업을 할지 설정하는 것과 같다. 
- 나는 따로 변수에 foreach 결과를 저장하는 방식을 택했다.

### 클러스터해산
```R
parallel::stopCluster(myCluster)
```
연산이 끝나면 클러스터를 해산시킨다.

<br/><br/><br/><br/><br/>
#### reference
- [bloght: Prarallel processing in R](https://hoontaeklee.github.io/en/posts/20200607_r%EB%B3%91%EB%A0%AC%EC%B2%98%EB%A6%AC%EB%A9%94%EB%AA%A8/)
- 