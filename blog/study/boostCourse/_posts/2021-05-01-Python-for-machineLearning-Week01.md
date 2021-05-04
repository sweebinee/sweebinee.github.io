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
header-img: https://user-images.githubusercontent.com/43258282/116971476-98c55900-acf4-11eb-85c1-b74b47160665.png
---

<div class="bs-callout bs-callout-info">
<div markdown="1">
<h4>이 강의에서 배울 내용</h4>
- python 기초
- 데이터 전처리 방법
- 약간의 머신러닝
</div></div>

# Pythonic Code
## Overview
---
> Life is too short, you need python.

**Pythonic code**란..
- 파이썬 스타일의 코딩 기법.
- 파이썬만의 문법을 활용해 효율적인 표현이 가능.
- 고급 코드로 갈 수록 pythonic code의 필요성이 강조됨.
- 많은 개발자들이 이미 pythonic coding을 하고 있음. (소통의 문제)

## Split & Join
---
### Split function
string type의 값을 빈칸 또는 문자를 기준으로 나눠 list의 형태로 변환함.
```python
>>> items = 'one two three'.split() #빈칸기준 나눔
>>> print(items)
['one','two','three']
>>> example = 'python,jquery,javascript'
>>> example.split(",") # ","를 기준으로 나눔

# unpacking: list로 반환된 각 값을 a, b, c 변수에 저장
>>> a, b, c = example.split(",")
```
### Join function
string list를 합쳐서 하나의 string으로 합침.
```python
>>> colors = ['red', 'blue', 'green', 'yellow']
>>> result = ''.join(colors)
>>> result
'redbluegreenyellow'
>>> result = ' '.join(colors) # 연결 시 빈칸 1칸으로 연결
>>> result
'red blue green yellow'
>>> result = ', '.join(colors) # 연결 시 ", "으로 연결
>>> result
'red, blue, green, yellow'
>>> result = '-'.join(colors) # 연결 시 "-"으로 연결
>>> result
'red-blue-green-yellow'
```

## List Comprehension
---
## Enumerate & Zip
---
## Lambda & MapReduce
---
## Asterisk
---

# Linear Algebra
## Linear algebra codes
---
## Case study 01
---
## Case study 02
---

# Assignment
## Basic linear algebra
---
## Insert operation
---
<br/><br/><br/><br/><br/>
#### reference
- [BoostCourse:머신러닝을 위한 파이썬 1) Pythonic Code](https://www.boostcourse.org/ai222/joinLectures/27836?isDesc=false)

#### 권장도서
- Fluent Python - 전문가를 위한 파이썬(한빛미디어, 2016) Part 2 - 시퀀스 & 딕셔너리
- Effective Python - 파이썬 코딩의 기술(길벗, 2016)
- [The Hitchhiker's Guide to Python](https://docs.python-guide.org/writing/style/) - [파이썬을 여행하는 히치하이커를 위한 안내서](https://python-guide-kr.readthedocs.io/ko/latest/)


