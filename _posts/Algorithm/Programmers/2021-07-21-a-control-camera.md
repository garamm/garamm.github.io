---
layout: post
title: "[프로그래머스] 탐욕법/단속카메라"
date: "2021-07-21 23:11"
updated: []
categories: ["Algorithm", "Programmers"]
---

#### **# 문제 설명**<br>
고속도로를 이동하는 모든 차량이 고속도로를 이용하면서 단속용 카메라를 한 번은 만나도록 카메라를 설치하려고 합니다.<br>
고속도로를 이동하는 차량의 경로 routes가 매개변수로 주어질 때, 모든 차량이 한 번은 단속용 카메라를 만나도록 하려면 최소 몇 대의 카메라를 설치해야 하는지를 return 하도록 solution 함수를 완성하세요.<br>
<br>
#### **# 제한사항**<br>
- 차량의 대수는 1대 이상 10,000대 이하입니다.
- routes에는 차량의 이동 경로가 포함되어 있으며 routes[i][0]에는 i번째 차량이 고속도로에 진입한 지점, routes[i][1]에는 i번째 차량이 고속도로에서 나간 지점이 적혀 있습니다.
- 차량의 진입/진출 지점에 카메라가 설치되어 있어도 카메라를 만난것으로 간주합니다.
- 차량의 진입 지점, 진출 지점은 -30,000 이상 30,000 이하입니다.

<br>
#### **# 입출력 예**

| routes | return |
| --- | --- |
| \[\[-20,-15\], \[-14,-5\], \[-18,-13\], \[-5,-3\]\] | 2 |

<br>
#### **# 입출력 예 설명**<br>
-5 지점에 카메라를 설치하면 두 번째, 네 번째 차량이 카메라를 만납니다.<br>
-15 지점에 카메라를 설치하면 첫 번째, 세 번째 차량이 카메라를 만납니다.<br>

---

#### **# 문제풀이**
```python
# Python3
def solution(routes):
    answer = 0
    # 출구 기준으로 오름차순 정렬
    routes = sorted(routes, key=lambda x:x[1])

    # 가장 낮은 진출 지점에 하나 설치 후 리스트에서 삭제
    cursor = routes[0][1]
    answer = 1
    del routes[0]

    while len(routes) != 0:
      for i in range(0, len(routes)):
        if routes[i][0] <= cursor or routes[i][1] <= cursor:
          del routes[i]
        else:
          answer = answer + 1
          cursor = routes[i][1]
          del routes[i]
        break

    return answer

solution([[-20,15], [-14,-5], [-18,-13], [-5,-3]]) # 2
solution([[-191,-107], [-184,-151], [-150,-102], [-171,-124], [-120,-114]]) # 2
solution([[0,2],[2,3],[3,4],[4,6]]) # 2
```