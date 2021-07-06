---
layout: post
category: "algorithm-solve"
title: "[프로그래머스] 완전탐색/소수 찾기"
date: "2020-11-07"
---

### 문제 설명
한자리 숫자가 적힌 종이 조각이 흩어져있습니다. 흩어진 종이 조각을 붙여 소수를 몇 개 만들 수 있는지 알아내려 합니다.<br>
각 종이 조각에 적힌 숫자가 적힌 문자열 numbers가 주어졌을 때, 종이 조각으로 만들 수 있는 소수가 몇 개인지 return 하도록 solution 함수를 완성해주세요.

<br>

### 제한사항
- numbers는 길이 1 이상 7 이하인 문자열입니다.
- numbers는 0~9까지 숫자만으로 이루어져 있습니다.
- "013"은 0, 1, 3 숫자가 적힌 종이 조각이 흩어져있다는 의미입니다.

<br>

### 입출력 예

|numbers|return|
|---|---|
|"17"|3|
|"011"|2|

입출력 예 설명<br>
- 입출력 예 #1
  - [1, 7]으로는 소수 [7, 17, 71]를 만들 수 있습니다.
- 입출력 예 #2
  - [0, 1, 1]으로는 소수 [11, 101]를 만들 수 있습니다.
  - 11과 011은 같은 숫자로 취급합니다.

<br>

### 문제풀이 (python)

```python
from itertools import permutations

def solution(numbers):
  answer = 0
  numberArr = list(numbers) # string split
  permArr = []
  for i in range(1, len(numberArr)+1):
    for j in list(permutations(numberArr,i)): # 순열 구하기
      permArr.append(''.join(j)) # int 배열을 숫자 string으로 변환하여 배열에 저장

  permArr = list(map(int, permArr)) # string 배열을 int 배열로 변환
  permArr = set(permArr) # 중복 제거

  # 소수찾기
  for i in permArr:
    if i != 0 and i != 1: # 0이나 1이 아닌 경우만 수행
      isPrime = True
      for j in range(2, i):
        if i % j == 0: # 나눈 나머지가 0인 경우 소수가 아님
          isPrime = False
          break
      if isPrime:
        answer += 1

  return answer
```