---
layout: post
title: "[프로그래머스] 완전탐색/전력망을 둘로 나누기"
date: "2023-03-28 22:04"
updated: []
categories: ["Algorithm", "Programmers"]
---

#### **# 문제 설명**<br>
n개의 송전탑이 전선을 통해 하나의 [트리](https://en.wikipedia.org/wiki/Tree_(data_structure)){:target="_blank"} 형태로 연결되어 있습니다. 당신은 이 전선들 중 하나를 끊어서 현재의 전력망 네트워크를 2개로 분할하려고 합니다. 이때, 두 전력망이 갖게 되는 송전탑의 개수를 최대한 비슷하게 맞추고자 합니다.<br>
송전탑의 개수 n, 그리고 전선 정보 wires가 매개변수로 주어집니다. 전선들 중 하나를 끊어서 송전탑 개수가 가능한 비슷하도록 두 전력망으로 나누었을 때, 두 전력망이 가지고 있는 송전탑 개수의 차이(절대값)를 return 하도록 solution 함수를 완성해주세요.<br>
<br>
#### **# 제한사항**<br>
- n은 2 이상 100 이하인 자연수입니다.
- wires는 길이가 n-1인 정수형 2차원 배열입니다.
  - wires의 각 원소는 [v1, v2] 2개의 자연수로 이루어져 있으며, 이는 전력망의 v1번 송전탑과 v2번 송전탑이 전선으로 연결되어 있다는 것을 의미합니다.
  - 1 ≤ v1 < v2 ≤ n 입니다.
  - 전력망 네트워크가 하나의 트리 형태가 아닌 경우는 입력으로 주어지지 않습니다.

<br>
#### **# 입출력 예**

| n | wires | result |
| --- | --- | --- |
| 9 | \[\[1, 3\], \[2, 3\], \[3, 4\], \[4, 5\], \[4, 6\], \[4, 7\], \[7, 8\], \[7, 9\]\] | 3 |
| 4 | \[\[1, 2\], \[2, 3\], \[3, 4\]\] | 0 |
| 7 | \[\[1, 2\], \[2, 7\], \[3, 7\], \[3, 4\], \[4, 5\], \[6, 7\]\] | 1 |

<br>
#### **# 입출력 예 설명**<br>
**입출력 예 #1**<br>
다음 그림은 주어진 입력을 해결하는 방법 중 하나를 나타낸 것입니다.
<p align="center"><img src="/assets/img/posts/algorithm-programmers-power-grid-division-1.png" alt="완전탐색/전력망을 둘로 나누기1"></p>
4번과 7번을 연결하는 전선을 끊으면 두 전력망은 각 6개와 3개의 송전탑을 가지며, 이보다 더 비슷한 개수로 전력망을 나눌 수 없습니다.<br>
또 다른 방법으로는 3번과 4번을 연결하는 전선을 끊어도 최선의 정답을 도출할 수 있습니다.<br>
<br>
**입출력 예 #2**<br>
다음 그림은 주어진 입력을 해결하는 방법을 나타낸 것입니다.
<p align="center"><img src="/assets/img/posts/algorithm-programmers-power-grid-division-2.png" alt="완전탐색/전력망을 둘로 나누기2"></p>
2번과 3번을 연결하는 전선을 끊으면 두 전력망이 모두 2개의 송전탑을 가지게 되며, 이 방법이 최선입니다.<br>
<br>
**입출력 예 #3**<br>
다음 그림은 주어진 입력을 해결하는 방법을 나타낸 것입니다.
<p align="center"><img src="/assets/img/posts/algorithm-programmers-power-grid-division-3.png" alt="완전탐색/전력망을 둘로 나누기3"></p>
3번과 7번을 연결하는 전선을 끊으면 두 전력망이 각각 4개와 3개의 송전탑을 가지게 되며, 이 방법이 최선입니다. 

---

#### **# 문제풀이**
```python
# Python3
def solution(n, wires):
    answer = n

    for cutedWire in wires:
        leftWires = []  # 왼쪽에 해당하는 송전탑
        rightWires = []  # 오른쪽에 해당하는 송전탑
        nums = list(range(1, n + 1))  # 확인할 송전탑 배열

        # 자른 전선을 제외한 나머지 전선
        otherWires = wires.copy()
        otherWires.remove(cutedWire)

        # 자른 전선과 연결된 송전탑 정보를 각각 배열에 저장
        leftWires.append(cutedWire[0])
        rightWires.append(cutedWire[1])

        # 확인할 송전탑 배열에서 자른 전선 관련 데이터 삭제
        nums.remove(cutedWire[0])
        nums.remove(cutedWire[1])

        # 송전탑 배열을 전부 확인할 때까지 반복
        while len(nums) != 0:
            targetNum = nums[0]  # 현재 확인할 송전탑 번고
            isDone = False
            for wire in otherWires:

                # 현재 확인할 송전탑과 연결된 다른 송전탑 찾기
                pair = 0
                if wire[0] == targetNum:
                    pair = wire[1]
                if wire[1] == targetNum:
                    pair = wire[0]

                # 확인중인 송전탑과 연결된 다른 송전탑이
                # 왼쪽에 있는지 오른쪽에 있는지 확인하고
                # 각각의 배열에 추가
                if pair != 0:
                    for left in leftWires:
                        if left == pair:
                            isDone = True
                            leftWires.append(targetNum)
                    for right in rightWires:
                        if right == pair:
                            isDone = True
                            rightWires.append(targetNum)

            # 확인이 끝났으면 배열에서 제거하고
            # 끝나지 않았으면 배열에서 제거한 후
            # 다시 더해서 해당 송전탑의 순서를 맨 뒤로 보낸다
            if not isDone:
                nums.append(targetNum)
            nums.pop(0)

        # 왼쪽과 오른쪽 차이를 구한다
        diff = abs(len(leftWires) - len(rightWires))
        if answer > diff:
            answer = diff

    return answer
```