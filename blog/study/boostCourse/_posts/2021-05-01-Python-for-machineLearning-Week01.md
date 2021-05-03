---
layout: post
author: "subin"
title:  "Pythonic Code"
subtitle: "Week01"
type: "Python for MachineLearning"
category: "boostCourse"
tags: ['BoostCourse','Python','machineLearning']
disqus: true
post-header: true
header-img: https://user-images.githubusercontent.com/43258282/116864409-c2be4300-ac42-11eb-9672-8a87d72e5d65.png
---

<div class="bs-callout bs-callout-info">
<div markdown="1">
<h4>이 강의에서 배울 내용</h4>
- python 기초
- 데이터 전처리 방법
- 약간의 머신러닝
</div></div>

# 컴퓨터 과학이란?<br>"the process of sloving problem"
<p align="center"><img src="https://user-images.githubusercontent.com/43258282/107792022-fdc57e80-6d97-11eb-8351-118f2e2d2359.png" alt="computer science">input을 받아서 output을 만들어내는 과정
</p>
<br>
문제를 해결하기 위해서는 입력과 출력을 어떻게 표현할지 모두가 동의할만한 표준이 필요하고, '컴퓨터 과학'의 첫번째 개념은 **정보의 표현 방법**이다.

# :pushpin: Binary : 2진법
컴퓨터에서는 오직 0과 1로만 데이터를 표현한다. 숫자뿐만 아니라 글자, 사진, 영상, 소리 등을 저장할 수 있다. HOW?!

먼저 2진법을 잠시 짚고 넘어가자면, 각 자리수가 2의 거듭제곱을 의미하고 0과 1만을 이용해서 각 자리의 숫자를 더해나가는 식으로 표현할 수 있다. 
<p align="center"><img src="https://user-images.githubusercontent.com/43258282/107794632-023f6680-6d9b-11eb-9ad6-743e7f7749f2.png" alt="example of binary" width="70%" height="70%">이건 101이 아니라 5다! Oh!
</p>

컴퓨터에는 수많은 **스위치(=트렌지스터)**가 있고 **on/off**의, 전기를 통하게 하느냐/아니냐의, **상태를 통해 0과 1을 표현**한다. 

컴퓨터에서 이렇게 하나의 스위치, 하나의 자릿수를 표현하는 단위를 <span style="color:#6495ED">**bit**</span>라고 한다.

### bit
bit(비트)는 "binary digit"의 줄임말. 0과 1, 두가지 값만 가질 수 있는 측정 단위이다. 
<p align="center"><img src="https://user-images.githubusercontent.com/43258282/107797229-0c169900-6d9e-11eb-977d-f7dfb13fcdbb.png" alt="bit and byte"></p>
컴퓨터에서 물리적으로 하나의 비트는 하나의 스위치로 표현된다. 스위치 켜기=1, 끄기=0.   
하지만 하나의 비트만으로 방대한 양의 데이터를 표현하는 것은 불가능하다. 그래서 더 큰 단위의 비트열들이 존재하고, **byte**는 8개의 bit가 모여 만들어진 단위이다. byte를 모으면 더 큰 단위를 표현하는 것도 가능하다.

### "컴퓨터는 어떻게 0과 1만으로 문자, 사진, 영상등 다양한 정보를 처리할 수 있을까?"

# :pushpin: 다양한 정보의 표현
### 문자의 표현
프로그래머들은 컴퓨터가 문자를 숫자로 표현할 수 있도록 표준을 정해뒀다! <span style="color:#6495ED">**ASCII**</span> code(American Standard Code for Information Interchange). ASCII는 총 128개의 부호로 정의 되어 있고, 그 안에는 upper/lower case 뿐만 아니라 !?#같은 문장부호도 포함되어 있다.
<p align="center"><img src="https://user-images.githubusercontent.com/43258282/107799479-cc04e580-6da0-11eb-98e3-2f804ee3fa6a.png" alt="part of ASCII code table"></p>

ASCII는 8bit만 사용하기 때문에 표현하는데는 한계가 있었고, 세상에는 알파벳 말고도 더 많은 문자들이 존재하기 때문에 (emoji도 포함해서:wink:) <span style="color:#6495ED">**Unicode**</span>가 나왔다. 유니코드는 더 많은 bit를 사용하기 때문에 :smiling_imp: 이런 emoji도 표현이 가능하다. 

### 그림, 영상의 표현
<p align="center"><img src="https://user-images.githubusercontent.com/43258282/107806502-4d14aa80-6daa-11eb-94a7-3ac1e1b5eb59.gif" alt="rgb graphics" width="30%" height="30%"></p>
문자와 똑같이 그림 역시 숫자로 표현한다. 스크린을 통해 보는 그림은 수많은 작은 점들로 구성되어 있는데 이것을 **픽셀**이라고 한다. 각각의 픽셀은 <span style="color:#FF0000">**red**</span>, <span style="color:#0000FF">**blue**</span>, <span style="color:#00FF00">**green**</span> 세 가지 색의 다양한 조합으로 특정 색을 나타내게 된다. 
ex) 빨강72, 파랑33, 초록72 = 노랑! 이렇게 숫자로 색을 표현하는 방식을 **RGB**(Red,Green,Blue)라고 한다.


### "컴퓨터는 숫자로 표현된 정보를 input으로 받는다. 이를 어떻게 가공하여 output을 내는 걸까?"

# :pushpin: Algorithm
지금까지 글자, 색깔 등을 컴퓨터가 이해할 수 있게 2진법으로 표현하는 표준에 대해서 알아봤다. 이것은 input(입력)에 해당하는 것. 이번에는 output(출력)에 대해서 알아보자.

<p align="center"><img src="https://user-images.githubusercontent.com/43258282/107845093-5d656d80-6e1c-11eb-80cc-56289a0ceae6.png" alt="the algorithm"></p>
알고리즘은 **문제를 해결하는 단계적 방법**이다. 즉, input으로 받은 자료를 output 의 형태로 만드는 처리과정.

문제를 해결하기 위한 방법(알고리즘)은 여러가지가 있을 수 있지만, **정확성**과 **효율성**의 측면에서 평가하여 가장 좋은 알고리즘을 고를 수 있다. 

이 강의에서는 전화번호부에서 mike smith를 찾는 알고리즘을 짜면서 이를 설명하고 있다.
- :closed_book:처음부터 한 장씩 넘기면서 찾는다 &#8594; 오래걸림 (비효율적)
- :orange_book:2장씩 넘기면서 찾는다 &#8594; 1번 보다는 빠름. 하지만 부정확함
- :green_book:절반을 줄이고 앞과 뒤에 있는지 확인해서 절반씩 줄여나간다. &#8594; 정확하고 효율적임!
<p align="center"><img src="https://user-images.githubusercontent.com/43258282/107845094-5f2f3100-6e1c-11eb-90a0-41b746517b52.png" alt="the best algorithm is..." width="50%" height="50%">문제의 크기가 커져도 3번 알고리즘은 해결 시간이 크게 늘지 않는다!</p>

**알고리즘은 규칙의 순서적 나열이다.**

### 의사코드: pseudocode
의사코드에 대한 정확한 정의는 없지만 알고리즘에 대한 아이디어를 인간이 사용하는 언어(편한 언어)로 코드처럼 작성해 보는 것을 말한다. 
위에서 사용했던 mike smith를 전화번호부에서 찾는 문제를 해결하기 위한 의사코드를 작성하면 다음과 같다.
```
1  Pick up phone book
2  Open to middle of phone book
3  Look at page
4  If Smith is on page
5  		Call Mike
6  Else if Smith is earlier in book
7  		Open to middle of left half of book
8  		Go back to line 3
9  Else if Smith is later in book
10 		Open to middle of right half of book
11 		Go back to line 3
12 Else
13 		Quit
```
위 의사코드에는 다른 프로그래밍 언어에서도 볼 수 있는 공통 규약이 있는데,  
`pick up`, `Open to`, `Look at`, `Go back`, `Quit`과 같은 동사 부분은 <span style="color:#6495ED">**함수(function)**</span>이라고 불린다.  
다음으로 `If`, `Else if`, `Else`와 같은 부분은 <span style="color:#6495ED">**조건(condition)**</span>.  
`Smith is on page`, `Smith is earlier in book`, `Smith is later in book`와 같은 부분은 <span style="color:#6495ED">**참거짓(Boolean)**</span>.  
`Go back to line 3`와 같은 부분은 <span style="color:#6495ED">**반복(loop)**</span>라고 한다.

앞으로 계속 나올 이야기..
<br/><br/><br/><br/><br/><br/>


--------------------
[모두를 위한 컴퓨터 과학 (CS50 2019)](https://www.boostcourse.org/cs112/joinLectures/41485)
