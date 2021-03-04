---
layout: post
author: "subin"
title:  "single-cell RNA-seq FAQs"
subtitle: "자주 찾아보는 질문"
type: "Basic"
category: "singleCell"
tags: ['single-cell RNA seq','FAQ']
disqus: true
---

내가 자주 찾아보는 질문 계속 찾기 번거로워서 한곳에 모아둔 곳.. 좀 기억하자 <br/>
:raising_hand: 따로 더 궁금하거나 찾기 어려운 질문이 있다면 댓글 또는 메일(subin.cho@i.ewha.ac.kr)로! <br/>
:crown: 이 세상에 바보같은 질문은 없다. :crown: <br/><br/>

기본 분석 툴: `10X` + `Seurat` 사용 가정

### :poop: subset 따로 떼서 subclustering진행할때, 어떤 assay사용? scTransform 또 진행해도 되나?
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; **RNA** assay **counts** slot으로 subset떼와서 <u>scTransform다시 진행.<u/> <br/>
:link: [Seurat Issue #2014 'SCTransform on subsets'](https://github.com/satijalab/seurat/issues/2014)

### :poop: subset() 사용할때 원하는 assay정보만 떼오는 기능은 없나? 
괜히 후속 분석할때 불안한데, RNA assay만 subsetting해올 순 없을까?