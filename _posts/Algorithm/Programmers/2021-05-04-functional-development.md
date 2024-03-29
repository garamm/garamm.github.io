---
layout: post
title: "[프로그래머스] 스택,큐/기능개발"
date: "2021-05-04 00:00"
updated: []
categories: ["Algorithm", "Programmers"]
---

#### **# 문제 설명**<br>
프로그래머스 팀에서는 기능 개선 작업을 수행 중입니다. 각 기능은 진도가 100%일 때 서비스에 반영할 수 있습니다.<br>
또, 각 기능의 개발속도는 모두 다르기 때문에 뒤에 있는 기능이 앞에 있는 기능보다 먼저 개발될 수 있고, 이때 뒤에 있는 기능은 앞에 있는 기능이 배포될 때 함께 배포됩니다.<br>
먼저 배포되어야 하는 순서대로 작업의 진도가 적힌 정수 배열 progresses와 각 작업의 개발 속도가 적힌 정수 배열 speeds가 주어질 때 각 배포마다 몇 개의 기능이 배포되는지를 return 하도록 solution 함수를 완성하세요.<br>
<br>
#### **# 제한사항**<br>
- 작업의 개수(progresses, speeds배열의 길이)는 100개 이하입니다.
- 작업 진도는 100 미만의 자연수입니다.
- 작업 속도는 100 이하의 자연수입니다.
- 배포는 하루에 한 번만 할 수 있으며, 하루의 끝에 이루어진다고 가정합니다. 예를 들어 진도율이 95%인 작업의 개발 속도가 하루에 4%라면 배포는 2일 뒤에 이루어집니다.

<br>
#### **# 입출력 예**

| progresses | speeds | return |
| --- | --- | --- |
| [93, 30, 55] | [1, 30, 5] | [2, 1] |
| [95, 90, 99, 99, 80, 99] | [1, 1, 1, 1, 1, 1] | [1, 3, 2] |

<br>
#### **# 입출력 예 설명**<br>
**입출력 예 #1**<br>
첫 번째 기능은 93% 완료되어 있고 하루에 1%씩 작업이 가능하므로 7일간 작업 후 배포가 가능합니다.<br>
두 번째 기능은 30%가 완료되어 있고 하루에 30%씩 작업이 가능하므로 3일간 작업 후 배포가 가능합니다. 하지만 이전 첫 번째 기능이 아직 완성된 상태가 아니기 때문에 첫 번째 기능이 배포되는 7일째 배포됩니다.<br>
세 번째 기능은 55%가 완료되어 있고 하루에 5%씩 작업이 가능하므로 9일간 작업 후 배포가 가능합니다.<br>
따라서 7일째에 2개의 기능, 9일째에 1개의 기능이 배포됩니다.<br>
<br>
**입출력 예 #2**<br>
모든 기능이 하루에 1%씩 작업이 가능하므로, 작업이 끝나기까지 남은 일수는 각각 5일, 10일, 1일, 1일, 20일, 1일입니다. 어떤 기능이 먼저 완성되었더라도 앞에 있는 모든 기능이 완성되지 않으면 배포가 불가능합니다.<br>
따라서 5일째에 1개의 기능, 10일째에 3개의 기능, 20일째에 2개의 기능이 배포됩니다.<br>

---

#### **# 문제풀이**
```java
// Java
import java.util.*;

class Solution {
    public int[] solution(int[] progresses, int[] speeds) {
        int[] answer = {};
        ArrayList<Integer> doneList = new ArrayList<>();
        int targetPosition = 0; // 작업 완료 기준으로 삼을 위치를 저장하는 변수

        while (true) {
            // 작업 진행
            for (int i = 0; i < progresses.length; i++) {
                progresses[i] += speeds[i];
            }
            
            // 만약 기준 작업이 완료된 경우
            if (progresses[targetPosition] >= 100) {
                int doneCnt = 0;
                // 기준 작업 이후에 완료된 항목을 조사
                for (int i = targetPosition; i < progresses.length; i++) {
                    // 만약 기준 작업 이후에 완료되지 않은 작업이 있으면
                    if (progresses[i] < 100) {
                        // 작업 완료 목록에 카운트 저장 및 기준 작업 위치 저장
                        doneList.add(doneCnt);
                        targetPosition = i;
                        break;
                    }
                    doneCnt++;
                }
                boolean isDone = true;
                // 모든 작업이 완료됐으면 break
                for (int i : progresses) {
                    if (i < 100) {
                        isDone = false;
                    }
                }
                if (isDone) {
                    doneList.add(doneCnt);
                    break;
                }
            }
        }
        answer = new int[doneList.size()];
        for(int i=0; i<doneList.size(); i++) {
            answer[i] = doneList.get(i);
        }
        return answer;
    }
}
```