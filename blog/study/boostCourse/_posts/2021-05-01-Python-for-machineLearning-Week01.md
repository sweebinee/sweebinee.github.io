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
kyobobooks: true
data-name: boostcourse-python-ml-w1
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
### Lambda function
익명함수, 함수의 이름이 따로 없지만 함수처럼 사용할 수 있는 함수. 
- 간단한 함수를 만들어 쓸때 사용한다. 
- `Map` or `Reduce` function과 함께 사용하면 더 잘 쓸 수 있음.
- python3 부터, list comprehension의 등장으로 개발자들은 lambda의 사용을 권장하지 않음.
- Legacy library나 다양한 machine learning code에서는 여전히 사용중.

```python
# general function
>>> def f(x, y):
>>>    return x + y
>>> print(f(1, 4))
5
# lambda function
>>> f = lambda x, y: x + y
>>> print(f(1, 4))
5
```

### Map function
sequence data의 각 element에 동일한 function을 적용함.
<p align="center"><img src="https://user-images.githubusercontent.com/43258282/116981377-5efb4f00-ad02-11eb-8905-dcf50a647b4d.PNG" alt="How dose map function work" height="250px"> Map function의 작동방식
</p>
```python
>>> ex = [1, 2, 3, 4, 5]
>>> f = lambda x: x ** 2
>>> print(list(map(f, ex))) 
[1, 4, 9, 16, 25]
#python2 에서는 map()만 사용해도 동일한 결과가 나오지만, python3 부터는 list(map())의 형태로 반드시 list()를 붙여줘야만 값을 얻을 수 있음.

# Zip function쓰지않고 같은 기능 구현
>>> ex = [1, 2, 3, 4, 5]
>>> f = lambda x, y: x + y
>>> print(list(map(f, ex, ex)))
[2, 4, 6, 8, 10]

>>> list(map(lambda x: x ** 2 if x % 2 == 0 else x, ex))
>>> [value ** 2 for value in ex]
# python3에서는 충분히 lambda와 map을 사용하지 않고, list comprehension만 가지고 구현가능

#python 3에는 list를 꼭 붙여줘야함
# 그렇지 않는다면 for문 사용해서 하나하나 출력할 수 있음.
>>> ex = [1,2,3,4,5]
>>> print(list(map(lambda x: x+x, ex)))
>>> print((map(lambda x: x+x, ex)))
>>> 
>>> f = lambda x: x ** 2
>>> print(map(f, ex))
>>> for i in map(f, ex):
>>>     print(i)
```

### Reduce function
sequence data의 각 element에 동일한 function을 적용하는것은 `map` function과 동일. 하지만 그 결과값을 다음 연산에 input으로 사용해 나아감.
```python 
>>> from functools import reduce
>>> print(reduce(lambda x, y: x+y, [1, 2, 3, 4, 5]))
15

>>> def factorial(n):
>>>     return reduce(
>>>             lambda x, y: x*y, range(1, n+1))
>>> 
>>> factorial(5)
120
```


## Asterisk
---
오픈소스코드에서 많이 사용하는 function. 흔히 알고 있는 `*`를 의미함. 곱셈, 제곱연산, 가변인자, unpacking container 활용 등에 다양하게 사용됨. 한번에 여러개의 변수를 넘겨줄때 사용함.

- 가변인자
```python
>>> def asterisk_test(a, *args):
>>>    print(a, args)
>>>    print(type(args))
>>>    
>>> asterisk_test(1,2,3,4,5,6)
1 (2, 3, 4, 5, 6)
<class 'tuple' >
# 1과 나머지(2,3,4,5,6)은 tuple형태로 args가변인자 에 들어감 
```

- 키워드 인자
```python
>>> def asterisk_test(a, **kargs): #**
>>>     print(a, kargs)
>>>     print(type(kargs))
>>>     
>>> asterisk_test(1, b=2, c=3, d=4, e=5, f=6)
1 {'b':2, 'c':3, 'd':4, 'e':5, 'f':6}
<class 'dict'>
#나머지가 dict 타입으로 키워드인자에 들어감
```
- unpacking a container
tuple, dict같은 자료형에 들어가 있는 값을 unpacking해서 넣고 싶다면 / 함수 입력값 | zip등에 활용
```python
>>> def asterisk_test(a, args): #(1, (2,3,4,5,6))
>>>     print(a, *args) #unpacking
>>>     print(type(args))
>>>     
>>> asterisk_test(1, (2,3,4,5,6))
1 2 3 4 5 6
<class 'tuple'>
```
<br/>
```python
>>> a, b, c = ([1, 2], [3, 4], [5, 6])
>>> print(a, b, c)
>>>
>>> data = ([1, 2], [3, 4], [5, 6])
>>> print(*data)
```
<br/>
```python
>>> for data in zip(*([1, 2], [3, 4], [5, 6]))7:
>>>     print(sum(data))
9 # 1+3+5
12 # 2+4+6
```
<br/>
```python
>>> def asterisk_test(a, b, c, d, e=0):
>>>     print(a, b, c, d, e)
>>>
>>> data = {"d":1 , "c":2, "b":3, "e"=56}
>>> asterisk_test(10, **data)
10 3 2 1 56
```
## Collections
- 자료구조 
- List, Tuple, Dict에 대한 python built-in 확장 자료구조(모듈)
- 편의성, 실행효율 증대
```python
from collecions import deque #stack & queue
from collecions import Counter
from collecions import OrderedDict
from collecions import defaultdict
from collecions import namedtuple
```

- Deque
Stack & queue 지원. list에 비해 효율적인 저장방식 지원.<br/>
```python
from collections import deque

deque_list = deque()
for i in range(5):
    deque_list.append(i)
print(deque_list)

deque_list.appendleft(10)
print(deque_list)

deque_list.rotate(2)
print(deque_list)
deque_list.rotate(2)
print(deque_list)

print(deque_list)
print(deque(reversed(deque_list)))

deque_list.extend([5, 6, 7])
print(deque_list)

deque_list.extendleft([5, 6, 7])
print(deque_list)
```
- 


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


