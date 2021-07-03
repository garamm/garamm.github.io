---
layout: post
title: "[Python 3] 내장 함수, 표준 라이브러리 정리"
author: 임가람
date: "2021-06-28 20:57:00"
categories: [Python3]
tags: [내장함수]
---

### Intro
저는 Python을 따로 프로젝트에서 사용하지는 않았는데 알고리즘 공부나 코딩 테스트에서 쓰기 유용해서 이참에 한 번 정리해보려고 합니다.<br>
이번 글은 Python 3에서 제공하는 내장 함수들인데요. 제가 문제 풀면서 유용하게 썼던 함수들 기준으로 차근 차근 정리할 예정입니다 ㅎㅎ

<br>

### 리스트 만들기
[x] * 5 : 모든 값이 x이고 길이가 5인 리스트 생성
```python
arr = [0] * 5
print(arr) # [0, 0, 0, 0, 0]
```

<br>

### 값 찾기
enumerate()<br>
for문과 자주 쓰이는 함수, for문에서 이 값이 몇 번째 값인지 확인하고 싶을 때 사용
```python
children = ["이익준", "채송화", "안정원", "김준완", "양석형"]
for idx, child in enumerate(children):
    print("%d번 학생은 %s 입니다." % (idx, child))
```

<br>

### 리스트에서 값의 개수 세기
Counter(리스트) : 리스트에서 x의 개수별로 정리
dict(Counter(리스트)) : 리스트에서 원소의 개수를 key-value로 정리
```python
from collections import Counter

counter = Counter(["A", "B", "A", "B", "C"])
print(counter["A"]) # 2
print(dict(counter)) # {'A': 2, 'B': 2, 'C': 1}
```

<br>

### 숫자 관련 함수
sum() : 값 더하기
min(), max() : 최소, 최대값 찾기
```python
print(sum([1, 2, 3])) # 6
print(min([1, 2, 3])) # 1
print(max([1, 2, 3])) # 3
```
eval() : 문자열 형태의 계산식을 실제 코드로 해석하여 실행
```python
print(eval("(1*2)+3")) # 5
```

<br>

### 형변환
str()<br>
문자열 형태로 객체를 반환
```python
print(str(123456)) # '123456'
```

<br>

### 아스키 코드
ord() : 문자를 아스키 코드로 변환<br>
chr() : 아스키 코드 값을 문자로 변환
```python
print(ord("A")) # 65
print(chr(65)) # 'A'
```

<br>

### 정렬
sorted(리스트) : 오름차순 정렬
sorted(리스트, reverse=True) : 내림차순 정렬
sorted(리스트, key=lambda x:x[n]) : n번째 키를 기준으로 오름차순 정렬
```python
print(sorted([1, 5, 3, 7, 9])) # [1, 3, 5, 7, 9]
print(sorted([1, 5, 3, 7, 9], reverse=True)) # [9, 7, 5, 3, 1] # [9, 7, 5, 3, 1]

arr = [("이익준", 4), ("채송화", 5), ("안정원", 2), ("김준완", 1), ("양석형", 3)]
print(sorted(arr, key=lambda x:x[1]))
# [('김준완', 1), ('안정원', 2), ('양석형', 3), ('이익준', 4), ('채송화', 5)]
```

<br>

### 순열, 조합
list(permutations(리스트, n)) : 리스트에서 n개를 선택
list(combinations(리스트, n)) : 리스트에서 순서에 상관 없이 n개 선택
list(product(리스트, repeat=n)) : 리스트에서 n개를 뽑는 모든 순열 (중복 허용)
list(combinations_with_replacement(리스트, n)) : 리스트에서 n개를 뽑는 모든 조합 (중복 허용)
list(product(*리스트)) : 2차원 이상의 리스트에서 모든 조합
```python
from itertools import permutations
from itertools import combinations
from itertools import product
from itertools import combinations_with_replacement

arr = [1, 2, 3]
print(list(permutations(arr, 2)))
# [(1, 2), (1, 3), (2, 1), (2, 3), (3, 1), (3, 2)]
print(list(combinations(arr, 2)))
# [(1, 2), (1, 3), (2, 3)]
print(list(product(arr, repeat=2)))
# [(1, 1), (1, 2), (1, 3), (2, 1), (2, 2), (2, 3), (3, 1), (3, 2), (3, 3)]
print(list(combinations_with_replacement(arr, 2)))
# [(1, 1), (1, 2), (1, 3), (2, 2), (2, 3), (3, 3)]

arr2 = [["A", "B"], ["a", "b"], [1, 2]]
# [('A', 'a', 1), ('A', 'a', 2), ('A', 'b', 1), ('A', 'b', 2), ('B', 'a', 1), ('B', 'a', 2), ('B', 'b', 1), ('B', 'b', 2)]
print(list(product(*arr2)))
```

<br>

### 최대공약수, 최소공배수
gcd(x, y) : x와 y의 최대 공약수
x * y // math.gcd(x, y) : x와 y의 최소 공배수
```python
import math
a, b = 6, 15
print(math.gcd(a, b)) # 3
print(a * b // math.gcd(a, b)) # 30
```

<br>

\# 참고<br>
[python 3 공식 문서](https://docs.python.org/ko/3/library/functions.html){:target="_blank"}<br>
[프리라이프의 기술 블로그](https://freedeveloper.tistory.com/365){:target="_blank"}<br>
[불로](https://ourcstory.tistory.com/414){:target="_blank"}<br>
<br>

