---
layout: post
author: "subin"
title:  "Chromium Single Cell Gene Expression Version 별 차이"
subtitle: "Dual Index vs. Single Index"
type: "10X"
category: "study/singleCell"
disqus: true
text: true
post-header: true
header-img: https://user-images.githubusercontent.com/43258282/105624857-2f69ba80-5e68-11eb-83ee-14a55783cb6d.jpg
---

현재 Chromium에는 3가지 버전의 solution을 팔고 있다.

- `Gene Expression v3.1-Dual Index`
- `Gene Expression v3.1-Single Index`
- `Gene Expression v3`

Dual index가 가장 최근 버전이며, 세 가지 버전 모두 `Gene Expression Type`과 `Feature Barcode Selection`, `Automation` 옵션을 선택할 수 있다.
<center>
![10X genomics product list에서 제공하는 옵션](https://user-images.githubusercontent.com/43258282/105625290-5c6b9c80-5e6b-11eb-942c-21b9e8966a31.png)*10X genomics product list에서 제공하는 옵션*
</center> 

Gene Expression solution은 모두 3' end만을 잡아내는 kit이다. 5'이나 full length를 원하면 V(D)J solution 쪽에서 알아봐야 한다. 3' seq과 5'seq의 차이와 장단점은 다른 포스트에서 다루도록 하자.

어떤 점이 업그레이드되어 출시됐는지 알아보자.

# Dual Index vs. Single Index
이름에서도 알 수 있듯이 Dual Index에서는 library 만드는 과정에 index가 하나 더 추가됐다.
(Sample index i5)
<center>
![dual_single_index](https://user-images.githubusercontent.com/43258282/105625512-3515cf00-5e6d-11eb-858c-3a062cde8a7c.png)*Dual Index(왼) 와 Single Index(오)의 library condtruction 과정 모식도*
</center> 

두 개의 index를 처리하기 위해 **CellRanger 4.0**의 `cellranger mkfastq`에서 **dual index libarary demultiplexing**을 지원한다고 한다. 

i5 index를 추가함으로써 어떤 <span style="color:#6495ED">**장점**</span>이 있을까?

##Index Hopping Migration

**Index hopping** 이란..
Index switching이라고도 하며, sample multiplexing[^1]이 개발된 이후로 NGS 기술에서 중요한 이슈 중 하나이다.
<center>
![index-hopping](https://user-images.githubusercontent.com/43258282/105625769-b588ff80-5e6e-11eb-8ba9-bbc4a527c078.png)
</center> 

이는 demultiplexing과정 도중에 발생하는 현상을 말하는데, <U>read가 expected index가 아닌 다른 index에 붙어 read와 index가 잘못 배치</U>된다. 이런 잘못은 misalignment 와 부정확한 sequencing results로 이어져 후속 분석에도 영향을 미친다.
<center>
![index-hopping-effect](https://user-images.githubusercontent.com/43258282/105625823-187a9680-5e6f-11eb-8aa8-e78febfeaaa5.png)
</center> 

그래서 dual index를 사용하게 되면, demultiplexing과정에서 unique한 i5와 i7 pair를 갖고 있는지 검사를 함께 진행하고 index hopping이 일어나 unique pair를 갖지 못한 read를 제거할 수 있다. 일반적으로 **0.1~2%의 read**가 이 과정에서 제거된다고 한다.[^2]  

첨부한 reference[^3]의 pdf에 같은 샘플을 가지고 single index와 dual index로 시퀀싱해서 비교한 결과가 나와있다. 궁금한 사람을 열어봐라. (clustering, immune cell subpopulation, library complexity & correlation) <U>모든 측면에서 눈에 띄는 큰 차이는 없었다.</U>

10,000개가 넘는 cell을 분석해서 그럴수도.. cell갯수가 적어지면 그 영향이 더 크지 않을까 싶다. 그래도 난 가격 차이가 별로 안난다면 dual index를 선택할 듯!
- - - 
#####Reference
[^1]: sample multiplexing : multiplex sequencing, 많은 수의 라이브러리들을 모아서 동시에 시퀀싱(single run)하는것. High-throughput이 가능하게 하며, cost-effective하다. 샘플을 "바코드"를 통해 구분해서 분석이 용이하다는 장점이 있다.
[^2]: [index hopping](https://www.illumina.com/content/dam/illumina-marketing/documents/products/whitepapers/index-hopping-white-paper-770-2017-004.pdf)
[^3]: [Chromium Next GEM Single Cell 3ʹ v3.1: Dual Index Libraries] (https://assets.ctfassets.net/an68im79xiti/Licpd2PiHP4hrHKDpjO89/2779c006e6317ed9ca724635b32e14e9/CG000325_TechNote_ChromiumNextGEMSingle_Cell_3___v3.1_Dual_Index_Rev_A.pdf)


<br><br><br>