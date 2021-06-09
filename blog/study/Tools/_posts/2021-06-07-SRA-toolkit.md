---
layout: post
author: "subin"
title:  "SRA toolkit"
subtitle: "Downlaod sequencing raw file from SRA"
type: "toolkit"
category: "Tools"
tags: ['']
disqus: true
post-header: true
header-img: https://user-images.githubusercontent.com/43258282/120978944-03642b80-c7b0-11eb-80a8-3b5f45728026.jpg
---

- [Installation](#installation)
- [Basic command](#basic-command)

## Installation
---
[download link](https://github.com/ncbi/sra-tools/wiki/02.-Installing-SRA-Toolkit) 여기에서 자신의 OS와 맞는 파일을 다운로드 받아 설치한다. linux의 경우 tar.gz파일 을 다운받아 압축을 해제하면 바로 사용가능하다. 편하게 사용하고 싶다면 압축해제한 경로를 bash또는 bash_profile에 넣어주면 된다.

## Basic command
---
```bash
fastq-dump --split-files SRR8209110
```