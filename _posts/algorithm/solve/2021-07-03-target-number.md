---
layout: post
title: "[프로그래머스] 깊이/너비 우선 탐색(DFS/BFS) / 타겟 넘버"
author: 임가람
date: "2021-07-04 01:11:00"
categories: [알고리즘, 문제풀이]
tags: [프로그래머스]
---

[문제풀기 - 프로그래머스](https://programmers.co.kr/learn/courses/30/lessons/43165){:target="_blank"}<br>

### 문제 설명

n개의 음이 아닌 정수가 있습니다. 이 수를 적절히 더하거나 빼서 타겟 넘버를 만들려고 합니다. 예를 들어 [1, 1, 1, 1, 1]로 숫자 3을 만들려면 다음 다섯 방법을 쓸 수 있습니다.<br>
```
-1+1+1+1+1 = 3
+1-1+1+1+1 = 3
+1+1-1+1+1 = 3
+1+1+1-1+1 = 3
+1+1+1+1-1 = 3
```
사용할 수 있는 숫자가 담긴 배열 numbers, 타겟 넘버 target이 매개변수로 주어질 때 숫자를 적절히 더하고 빼서 타겟 넘버를 만드는 방법의 수를 return 하도록 solution 함수를 작성해주세요.

<br>

### 제한사항
- 주어지는 숫자의 개수는 2개 이상 20개 이하입니다.
- 각 숫자는 1 이상 50 이하인 자연수입니다.
- 타겟 넘버는 1 이상 1000 이하인 자연수입니다.

<br>

### 입출력 예

|numbers|target|return|
|---|---|---|---|
|[1, 1, 1, 1, 1]|3|5|

<br>

### 풀이

이 문제에서 나올 수 있는 경우의 수는 2ⁿ개 입니다.<br>
만약 numbers가 [1, 1, 1]인 경우 전체 경우의 수는 2³(=8)개 입니다.

![타겟넘버-이미지](/assets/img/posts/2021-07-03-target-number.png){: width="600" height="150" }

그래서 나올 수 있는 모든 경우의 수를 찾은 후 target과 일치하는 경우 answer를 더해주는 방식으로 풀었습니다.

```python
def solution(numbers, target):
  answer = 0
  
  numList = []
  for i in range(0, len(numbers)):
    arrlen = pow(2, i+1)
    divs = int(pow(2, len(numbers)) / arrlen)
    arr = []
    for j in range(0, arrlen):
      if j % 2 == 0:
        arr += [numbers[i]] * divs
      else:
        arr += [-numbers[i]] * divs
    numList.append(arr)
  print(numList)
  
  for i in range(0, pow(2, len(numbers))):
    sumnum = 0
    for j in range(0, len(numList)):
      sumnum += numList[j][i]
    if sumnum == target: answer += 1
  return answer
    

solution([1, 1, 1, 1, 1], 3) # 5
```