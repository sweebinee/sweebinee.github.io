---
layout: post
author: "subin"
title:  "Computational Thinking, Scratch"
subtitle: "Week01"
type: "CS50"
category: "study/boostCourse"
tags: ['Computer Science','2진법','bit','byte','ASCII','']
disqus: true
post-header: true
header-img: https://user-images.githubusercontent.com/43258282/107766537-efb33600-6d76-11eb-9ec6-a8fd6916fce7.png
---
 > What ultimately matters in this course is not so much where you end up relative to your classmates but where you end up relative to yourself when you began.  
 > pf. David Malan 

# 컴퓨터 과학이란?<br>"the process of sloving problem"
<p align="center"><img src="https://user-images.githubusercontent.com/43258282/107792022-fdc57e80-6d97-11eb-8351-118f2e2d2359.png" alt="computer science">input을 받아서 output을 만들어내는 과정
</p>
<br>
문제를 해결하기 위해서는 입력과 출력을 어떻게 표현할지 모두가 동의할만한 표준이 필요하고, '컴퓨터 과학'의 첫번째 개념은 **정보의 표현 방법**이다.

# Binary : 2진법
컴퓨터에서는 오직 0과 1로만 데이터를 표현한다. 숫자뿐만 아니라 글자, 사진, 영상, 소리 등을 저장할 수 있다. HOW?!

2진법을 잠시 짚고 넘어가자면, 각 자리수가 2의 거듭제곱을 의미하고 0과 1만을 이용해서 각 자리의 숫자를 더해나가는 식으로 표현할 수 있다. 
<p align="center"><img src="https://user-images.githubusercontent.com/43258282/107794632-023f6680-6d9b-11eb-9ad6-743e7f7749f2.png"alt="example of binary">이건 101이 아니라 5다! Oh!
</p>

컴퓨터에는 수많은 스위치(=트렌지스터)가 있고 on/off의, 전기를 통하게 하느냐/아니냐의, 상태를 통해 0과 1을 표현한다. 

컴퓨터에서 하나의 자릿수를 표현하는 단위를 <span style="color:#6495ED">**bit**</span>라고 한다.

### bit
bit(비트)는 "binary digit"의 줄임말. 0과 1, 두가지 값만 가질 수 있는 측정 단위이다. 
<p align="center"><img src="https://user-images.githubusercontent.com/43258282/107797229-0c169900-6d9e-11eb-977d-f7dfb13fcdbb.png"alt="bit and byte"></p>
컴퓨터에서 물리적으로 하나의 비트는 하나의 스위치로 표현된다. 스위치 켜기=1, 끄기=0.   
하지만 하나의 비트만으로 방대한 양의 데이터를 표현하는 것은 불가능하다. 그래서 더 큰 단위의 비트열들이 존재하고, **byte**는 8개의 bit가 모여 만들어진 단위이다. byte를 모으면 더 큰 단위를 표현하는 것도 가능하다.

### "컴퓨터는 어떻게 0과 1만으로 문자, 사진, 영상, 음악등 다양한 정보를 처리할 수 있었을까?"



[^2]  

첨부한 reference[^3]의 pdf에 같은 샘플을 가지고 single index와 dual index로 시퀀싱해서 비교한 결과가 나와있다. 궁금한 사람을 열어봐라. (clustering, immune cell subpopulation, library complexity & correlation) <U>모든 측면에서 눈에 띄는 큰 차이는 없었다.</U>

10,000개가 넘는 cell을 분석해서 그럴수도.. cell갯수가 적어지면 그 영향이 더 크지 않을까 싶다. 그래도 난 가격 차이가 별로 안난다면 dual index를 선택할 듯!


[^1]: sample multiplexing : multiplex sequencing, 많은 수의 라이브러리들을 모아서 동시에 시퀀싱(single run)하는것. High-throughput이 가능하게 하며, cost-effective하다. 샘플을 "바코드"를 통해 구분해서 분석이 용이하다는 장점이 있다.
[^2]: [index hopping](https://www.illumina.com/content/dam/illumina-marketing/documents/products/whitepapers/index-hopping-white-paper-770-2017-004.pdf)
[^3]: [Chromium Next GEM Single Cell 3ʹ v3.1: Dual Index Libraries](https://assets.ctfassets.net/an68im79xiti/Licpd2PiHP4hrHKDpjO89/2779c006e6317ed9ca724635b32e14e9/CG000325_TechNote_ChromiumNextGEMSingle_Cell_3___v3.1_Dual_Index_Rev_A.pdf)
