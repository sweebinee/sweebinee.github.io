---
layout: post
author: "subin"
title:  "R package (dependency) 설치에 지친 당신을 위한 꿀팁"
subtitle: "너 하고 싶은거 다해, R 따위한테 질 수 없지"
type: "Master key"
category: "Rcode"
tags: ["R with conda"]
disqus: true
post-header: true
header-img: https://user-images.githubusercontent.com/43258282/120966147-c80e3080-c7a0-11eb-8a24-c11ea43862d2.jpg
---

어쩌다 이 길로 들어오게 됐는지 기억도 나지 않는다. <br/>
이 글을 읽고 있는 당신(또는 미래의 나) 또한 R package 설치 에러에 지칠대로 지쳐 여기까지 흘러왔겠지. 인정하고 받아들이면 편해진다고 이야기해주고 싶다.<br/><br/>
<span style="color:#997ADB"> *정말 어렸을 때, 모든 것에 근자감이 넘치던 시절엔 '진정한 프로그래머라면 conda따위에 의존하지 말아야한다'고 생각했었는데.. 좋은게 있으면 써야지 나만 안쓰고 바보같이 일하면 머리나 빠지고 시간만 잡아먹는다. 쓰라고 만든거니 잘 사용해주자!* </span><br/>

- [What is Conda](#conda)
- [Installation](#installation)
- [Basic command](#basic-command)

# Conda
conda는 우리가 패키지를 직접 설치할때 발생할 수 있는 dependency 문제나, 버전 관리 등을 가상환경을 통해 손쉽게 관리할 수 있게 해주는 도구다. 일반적으로는 python 언어의 패키지 관리를 위해 사용하는데 R 언어도 사용가능하다. 많이 사용하는 소스로는 `Anaconda`가 있다. 

## Installation
---
Anaconda를 설치하는 방법은 매우 간단하다. ([download link](https://www.anaconda.com/products/individual-b))<br/>
링크로 들어가서 자신의 OS에 맞는 installer를 받아준다. <span style="color:#997ADB">*나는 linux기준* </span>
```bash
wget https://repo.anaconda.com/archive/Anaconda3-2021.05-Linux-x86_64.sh
bash Anaconda3-2021.05-Linux-x86_64.sh
```
설치 경로나 동의여부 같은걸 물어볼텐데 알아서 잘 대답하면 손쉽게 설치가 완료된다.<br/>
잘 깔렸는지 확인
```bash
conda --version
#최신버전이 아니라면 업데이트 가능
conda update conda
```
<br/>


## Basic Command
---
- **새로운 환경 생성**<br/>
conda를 시작하면 기본 환경인 `base`가 열려 있다. conda는 프로젝트별로 또는 도구별로 별도의 환경을 구성해서 작업할 수 있게 한다. 
```bash
conda create --name [env_name]
#--name 또는 -n
```
<br/>
- **환경 활성화/비활성화**< br/>
생성한 환경을 활성화 시켜야 그 환경에 설치한 프로그램이나 패키지들을 사용할 수 있다.
```bash
conda activate [env_name]
conda deactivate [env_name]
```
<br/>
- **환경 목록 보기**<br/>
지금까지 만든 환경들의 이름목록을 볼 수 있다.
```bash
conda env list
```
<br/>
- **환경 삭제**<br/>
```bash
conda env remove -n [env_name]
```
<br/>
---
여기까지가 기본 command.<br/> 이제 가상환경을 하나 생성하고 R과 필요한 package를 설치해보자.
```bash
conda create -n scRNAanalysis #생성
conda activate scRNAanalysis #활성화

conda install -c r r-essentials #R 설치
#기본적으로 필요한 R package 설치
conda install -c bioconda r-seurat
conda install -c conda-forge r-ggplot2

```
더 필요한 R package가 있다면 구글에 "conda [package neme]"해서 검색해보자. 왠만하면 제일 처음에 anaconda.org에 올라온 설치 command 페이지가 나온다.