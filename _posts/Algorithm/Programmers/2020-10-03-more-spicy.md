---
layout: post
title: "[프로그래머스] 힙/더 맵게"
date: "2020-10-03 00:00"
updated: []
categories: ["Algorithm", "Programmers"]
---

#### **# 문제 설명**<br>
매운 것을 좋아하는 Leo는 모든 음식의 스코빌 지수를 K 이상으로 만들고 싶습니다. 모든 음식의 스코빌 지수를 K 이상으로 만들기 위해 Leo는 스코빌 지수가 가장 낮은 두 개의 음식을 아래와 같이 특별한 방법으로 섞어 새로운 음식을 만듭니다.
```
섞은 음식의 스코빌 지수 = 가장 맵지 않은 음식의 스코빌 지수 + (두 번째로 맵지 않은 음식의 스코빌 지수 * 2)
```
Leo는 모든 음식의 스코빌 지수가 K 이상이 될 때까지 반복하여 섞습니다.<br>
Leo가 가진 음식의 스코빌 지수를 담은 배열 scoville과 원하는 스코빌 지수 K가 주어질 때, 모든 음식의 스코빌 지수를 K 이상으로 만들기 위해 섞어야 하는 최소 횟수를 return 하도록 solution 함수를 작성해주세요.<br>
<br>
#### **# 제한사항**<br>
- scoville의 길이는 2 이상 1,000,000 이하입니다.
- K는 0 이상 1,000,000,000 이하입니다.
- scoville의 원소는 각각 0 이상 1,000,000 이하입니다.
- 모든 음식의 스코빌 지수를 K 이상으로 만들 수 없는 경우에는 -1을 return 합니다.

<br>
#### **# 입출력 예**

| scoville | K | return |
| --- | --- | --- |
| \[1, 2, 3, 9, 10, 12\] | 7 | 2 |

<br>
#### **# 입출력 예 설명**<br>
1\. 스코빌 지수가 1인 음식과 2인 음식을 섞으면 음식의 스코빌 지수가 아래와 같이 됩니다.<br>
새로운 음식의 스코빌 지수 = 1 + (2 * 2) = 5<br>
가진 음식의 스코빌 지수 = [5, 3, 9, 10, 12]<br>
<br>
2\. 스코빌 지수가 3인 음식과 5인 음식을 섞으면 음식의 스코빌 지수가 아래와 같이 됩니다.<br>
새로운 음식의 스코빌 지수 = 3 + (5 * 2) = 13<br>
가진 음식의 스코빌 지수 = [13, 9, 10, 12]<br>
<br>
모든 음식의 스코빌 지수가 7 이상이 되었고 이때 섞은 횟수는 2회입니다.

---

#### **# 문제풀이**
```java
// Java
import java.util.*;

public int solution(int[] scoville, int K) {
    int answer = 0;
    PriorityQueue<Integer> queue = new PriorityQueue<>();
    // 스코빌 지수가 높은 순서대로 정렬
    for (int i : scoville) {
        queue.add(i);
    }

    while (true) {
        // 값이 1개이고 K보다 작으면 -1을 리턴
        if (queue.size() == 1) {
            if (queue.peek() < K) {
                answer = -1;
            }
            break;
        }

        // 첫번째와 두번째를 섞음
        int mix = queue.poll() + (2 * queue.poll());
        answer++;
        queue.add(mix);

        // 만약 모든 음식의 스코빌 지수가 K 이상이면 종료
        boolean isFinish = true;            
        for (int value : queue) {
            if (value < K) {
                isFinish = false;
                break;
            }
        }
        if (isFinish) {
            break;
        }
    }
    return answer;
}
```