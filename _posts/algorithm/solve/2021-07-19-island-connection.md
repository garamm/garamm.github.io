---
layout: post
title: "[프로그래머스] 탐욕법(Greedy) / 섬 연결하기"
author: 임가람
date: "2021-07-19 20:37:00"
categories: [알고리즘, 문제풀이]
tags: [프로그래머스]
---

[문제풀기 - 프로그래머스](https://programmers.co.kr/learn/courses/30/lessons/42861){:target="_blank"}<br>

### 문제 설명

n개의 섬 사이에 다리를 건설하는 비용(costs)이 주어질 때, 최소의 비용으로 모든 섬이 서로 통행 가능하도록 만들 때 필요한 최소 비용을 return 하도록 solution을 완성하세요.<br>
<br>
다리를 여러 번 건너더라도, 도달할 수만 있으면 통행 가능하다고 봅니다. 예를 들어 A 섬과 B 섬 사이에 다리가 있고, B 섬과 C 섬 사이에 다리가 있으면 A 섬과 C 섬은 서로 통행 가능합니다.

<br>

### 제한 사항
- 섬의 개수 n은 1 이상 100 이하입니다.
- costs의 길이는 `((n-1) * n) / 2`이하입니다.
- 임의의 i에 대해, costs[i][0] 와 costs[i] [1]에는 다리가 연결되는 두 섬의 번호가 들어있고, costs[i] [2]에는 이 두 섬을 연결하는 다리를 건설할 때 드는 비용입니다.
- 같은 연결은 두 번 주어지지 않습니다. 또한 순서가 바뀌더라도 같은 연결로 봅니다. 즉 0과 1 사이를 연결하는 비용이 주어졌을 때, 1과 0의 비용이 주어지지 않습니다.
- 모든 섬 사이의 다리 건설 비용이 주어지지 않습니다. 이 경우, 두 섬 사이의 건설이 불가능한 것으로 봅니다.
- 연결할 수 없는 섬은 주어지지 않습니다.

<br>

### 입출력 예

|n|costs|return|
|---|---|---|
|4|[[0,1,1],[0,2,2],[1,2,5],[1,3,1],[2,3,8]]|4|

<br>

### 입출력 예 설명

costs를 그림으로 표현하면 다음과 같으며, 이때 초록색 경로로 연결하는 것이 가장 적은 비용으로 모두를 통행할 수 있도록 만드는 방법입니다.<br>
![섬연결하기-입출력예-이미지](/assets/img/posts/2021-07-19-island-connection.png){: width="600" height="150" }

<br>

### 풀이
```python
def solution(n, costs):
    answer = 0
    costs.sort(key = lambda x:x[2]) # 비용 오름차순 정렬
    doneYn = [0] * n # 해당 섬에 들렸는지 여부 체크하는 리스트
    
    # 첫번째 값 세팅
    doneYn[costs[0][0]] = 1
    doneYn[costs[0][1]] = 1
    answer = answer + costs[0][2]
    del costs[0]
    
    while True:
      # 모든 섬이 연결되었으면 종료
      if sum(doneYn) == n:
        break

      for i in range(0, len(costs)):
        # 현재 섬이 아무하고도 연결되어 있지 않으면 넘김
        if doneYn[costs[i][0]] == 0 and doneYn[costs[i][1]] == 0:
          continue
        # 현재 섬이 한쪽과 연결되어 있으면 연결 후 리스트에서 삭제
        elif doneYn[costs[i][0]] == 0 or doneYn[costs[i][1]] == 0:
          doneYn[costs[i][0]] = 1
          doneYn[costs[i][1]] = 1
          answer = answer + costs[i][2]
          del costs[i]
          break
        # 현재 두쪽 다 연결되어 있는 상태면 삭제
        else:
          del costs[i]
          break
        
    return answer
```