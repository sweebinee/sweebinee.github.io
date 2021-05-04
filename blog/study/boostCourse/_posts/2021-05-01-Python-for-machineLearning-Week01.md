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
파이썬에서 가장 많이 사용되는 기법 중 하나로, 기존의 list를 활용해 다른 list를 만드는 방법

- for loop + .append()사용한 방법
```python
>>> result = []
>>> for i in range(10):
... result.append(i)
...
>>> result
[0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
```
- **list comprehension** 사용한 방법
```python
>>> result = [i for i in range(10)]
>>> result
[0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
>>> result = [i for i in range(10) if i % 2 == 0] # 조건물 넣을 수 있음
>>> result
[0, 2, 4, 6, 8]
```
<br/>
- Nested for loop
for loop 두번, for loop안에 for loop쓴 방법. **one dimension list로 나온다**
```python
>>> word_1 = "Hello"
>>> word_2 = "World"
>>> result = [i+j for i in word_1 for j in word_2] # Nested For loop
>>> result
['HW', 'Ho', 'Hr', 'Hl', 'Hd', 'eW', 'eo', 'er',
'el', 'ed', 'lW', 'lo', 'lr', 'll', 'ld', 'lW',
'lo', 'lr', 'll', 'ld', 'oW', 'oo', 'or', 'ol', 'od']
```
- Nested for loop + if statement
```python
>>> case_1 = ["A","B","C"]
>>> case_2 = ["D","E","A"]
>>> result = [i+j for i in case_1 for j in case_2]
>>> result
['AD', 'AE', 'AA', 'BD', 'BE', 'BA', 'CD', 'CE', 'CA']
>>> result = [i+j for i in case_1 for j in case_2 if not(i==j)] #if문 추가
# Filter: i랑 j과 같다면 List에 추가하지 않음 
>>> result
['AD', 'AE', 'BD', 'BE', 'BA', 'CD', 'CE', 'CA']
>>> result.sort()
>>> result
['AD', 'AE', 'BA', 'BD', 'BE', 'CA', 'CD', 'CE']
```
- split() + list conprehension -> two dimensional list
```python
>>> words = 'The quick brown fox jumps over the lazy dog'.split()
# 문장을 빈칸 기준으로 나눠 list로 변환
>>> print (words)
['The', 'quick', 'brown', 'fox',
'jumps', 'over', 'the', 'lazy', 'dog']
>>>
>>> stuff = [[w.upper(), w.lower(), len(w)] for w in words]
# list의 각 elemente들을 대문자, 소문자, 길이로 변환하여 two dimensional list로 변환
>>>
>>> for i in stuff:
... print (i)
...
['THE', 'the', 3]
['QUICK', 'quick', 5]
['BROWN', 'brown', 5]
['FOX', 'fox', 3]
['JUMPS', 'jumps', 5]
['OVER', 'over', 4]
['THE', 'the', 3]
['LAZY', 'lazy', 4]
['DOG', 'dog', 3]
```

### One dimensional list vs. Two dimensional list
```python
>>> case_1 = ["A","B","C"]
>>> case_2 = ["D","E","A"]
>>> result = [i+j for i in case_1 for j in case_2]
>>> result
['AD', 'AE', 'AA', 'BD', 'BE', 'BA', 'CD', 'CE', 'CA']
>>> result = [ [i+j for i in case_1] for j in case_2]
>>> result
[['AD', 'BD', 'CD'], ['AE','BE','CE'],['AA','BA','CA']]
```

## Enumerate & Zip
---
### Enumerate function
리스트의 값을 추출할때 인덱스를 함께 추출할 수 있는 방법
```python
>>> for i, v in enumerate(['tic', 'tac', 'toe']): 
# list의 있는 index와 값을 unpacking
... print (i, v)
...
0 tic
1 tac
2 toe
>>> mylist = ["a","b","c","d"]
>>> list(enumerate(mylist)) # list의 있는 index와 값을 unpacking하여 list로 저장
[(0, 'a'), (1, 'b'), (2, 'c'), (3, 'd')]
>>> {i:j for i,j in enumerate('Gachon University is an academic institute
located in South Korea.'.split())}
# 문장을 list로 만들고 list의 index와 값을 unpacking하여 dict로 저장
{0: 'Gachon', 1: 'University', 2: 'is', 3: 'an', 4: 'academic', 5: 'institute', 6: 'located', 7: 'in', 8: 'South', 9: 'Korea.'}
```

### Zip function
두개의 list를 병렬로 추출. 같은 index에 있는 값을 뽑아줌.
```python
>>> alist = ['a1', 'a2', 'a3']
>>> blist = ['b1', 'b2', 'b3']
>>> for a, b in zip(alist, blist): # 병렬적으로 값을 추출
... print (a,b)
...
a1 b1
a2 b2
a3 b3
>>> a,b,c =zip((1,2,3),(10,20,30),(100,200,300)) #각 tuple의 같은 index 끼리
묶음
(1, 10, 100) (2, 20, 200) (3, 30, 300)
>>> [sum(x) for x in zip((1,2,3), (10,20,30), (100,200,300))]
# zip + list comprehension -> such as vector calculation
# 각 Tuple 같은 index를 묶어 합을 list로 변환
[111, 222, 333]
```

- Enumerate & Zip
```python
>>> alist = ['a1', 'a2', 'a3']
>>> blist = ['b1', 'b2', 'b3']
>>>
>>> for i, (a, b) in enumerate(zip(alist, blist)):
... print (i, a, b) # index alist[index] blist[index] 표시
...
0 a1 b1
1 a2 b2
2 a3 b3
```


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


