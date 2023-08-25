---
layout: post
title: "[프로그래머스] 힙/이중우선순위큐"
date: "2020-10-18 00:00"
updated: []
categories: ["Algorithm", "Programmers"]
---

#### **# 문제 설명**<br>
이중 우선순위 큐는 다음 연산을 할 수 있는 자료구조를 말합니다.
| 명령어 | 수신 탑(높이) |
| --- | --- |
| I 숫자 | 큐에 주어진 숫자를 삽입합니다. |
| D 1 | 큐에서 최댓값을 삭제합니다. |
| D -1 | 큐에서 최솟값을 삭제합니다. |


이중 우선순위 큐가 할 연산 operations가 매개변수로 주어질 때, 모든 연산을 처리한 후 큐가 비어있으면 [0,0] 비어있지 않으면 [최댓값, 최솟값]을 return 하도록 solution 함수를 구현해주세요.<br>
<br>
#### **# 제한사항**<br>
- operations는 길이가 1 이상 1,000,000 이하인 문자열 배열입니다.
- operations의 원소는 큐가 수행할 연산을 나타냅니다.
  - 원소는 "명령어 데이터" 형식으로 주어집니다.- 최댓값/최솟값을 삭제하는 연산에서 최댓값/최솟값이 둘 이상인 경우, 하나만 삭제합니다.
- 빈 큐에 데이터를 삭제하라는 연산이 주어질 경우, 해당 연산은 무시합니다.

<br>
#### **# 입출력 예**

| operations | return |
| --- | --- |
| \["I 16", "I -5643", "D -1", "D 1", "D 1", "I 123", "D -1"\] | \[0, 0\] |
| \["I -45", "I 653", "D 1", "I -642", "I 45", "I 97", "D 1", "D -1", "I 333"\] | \[333, -45\] |

<br>
#### **# 입출력 예 설명**<br>
**입출력 예 #1**<br>
- 16과 -5643을 삽입합니다.
- 최솟값을 삭제합니다. -5643이 삭제되고 16이 남아있습니다.
- 최댓값을 삭제합니다. 16이 삭제되고 이중 우선순위 큐는 비어있습니다.
- 우선순위 큐가 비어있으므로 최댓값 삭제 연산이 무시됩니다.
- 123을 삽입합니다.
- 최솟값을 삭제합니다. 123이 삭제되고 이중 우선순위 큐는 비어있습니다.

따라서 [0, 0]을 반환합니다.<br>
<br>
**입출력 예 #2**<br>
- -45와 653을 삽입후 최댓값(653)을 삭제합니다. -45가 남아있습니다.
- -642, 45, 97을 삽입 후 최댓값(97), 최솟값(-642)을 삭제합니다. -45와 45가 남아있습니다.
- 333을 삽입합니다.

이중 우선순위 큐에 -45, 45, 333이 남아있으므로, [333, -45]를 반환합니다.<br>
<br>
[출처](http://icpckorea.org/problems/2013/onlineset.pdf){:target="_blank"}

---

#### **# 문제풀이**
```java
// Java
import java.util.*;

class Solution {
    public int[] solution(String[] operations) {
        int[] answer = new int[2];
        
        PriorityQueue<Integer> queue = new PriorityQueue<>(Collections.reverseOrder()); // 내림차순 큐
        PriorityQueue<Integer> reverseQueue = new PriorityQueue<>(); // 오름차순 큐
        for (String op : operations) {
            if (op.split(" ")[0].equals("I")) { // 삽입
                queue.add(Integer.valueOf(op.split(" ")[1]));
            } else { // 삭제
                if (!queue.isEmpty()) { // 큐가 비어있지 않은 경우에만 수행
                    if (op.equals("D 1")) { // 큐에서 최대값 삭제
                        queue.poll();
                    } else { // 큐에서 최소값 삭제
                        reverseQueue.clear(); // 오름차순 큐 초기화
                        // 내림차순 큐에 있는 데이터를 오름차순 큐에 넣기
                        Iterator iterator = queue.iterator();
                        while (iterator.hasNext()) {
                            reverseQueue.add((Integer) iterator.next());
                        }
                        reverseQueue.poll(); // 최소값 삭제
                        queue.clear();
                        // 오름차순 큐에 있는 데이터를 다시 내림차순 큐에 넣기
                        for (Integer i : reverseQueue) {
                            queue.add(i);
                        }
                    }
                }
            }
        }

        // 큐가 비어있으면 [0, 0] 리턴
        if (queue.isEmpty()) {
            answer[0] = 0;
            answer[1] = 0;
        } else {
            answer[0] = queue.peek(); // 배열의 0번째에는 가장 큰 수 입력
            reverseQueue.clear(); // 배열의 1번째에는 가장 작은 수 입력
            Iterator iterator = queue.iterator();
            while (iterator.hasNext()) {
                reverseQueue.add((Integer) iterator.next());
            }
            answer[1] = reverseQueue.peek();
        }
        return answer;
    }
}
```