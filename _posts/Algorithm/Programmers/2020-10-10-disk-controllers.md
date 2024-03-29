---
layout: post
title: "[프로그래머스] 힙/디스크 컨트롤러"
date: "2020-10-10 00:00"
updated: []
categories: ["Algorithm", "Programmers"]
---

#### **# 문제 설명**<br>
하드디스크는 한 번에 하나의 작업만 수행할 수 있습니다. 디스크 컨트롤러를 구현하는 방법은 여러 가지가 있습니다. 가장 일반적인 방법은 요청이 들어온 순서대로 처리하는 것입니다.<br>
예를들어
```
- 0ms 시점에 3ms가 소요되는 A작업 요청
- 1ms 시점에 9ms가 소요되는 B작업 요청
- 2ms 시점에 6ms가 소요되는 C작업 요청
```
와 같은 요청이 들어왔습니다. 이를 그림으로 표현하면 아래와 같습니다.
<p align="center"><img src="/assets/img/posts/algorithm-programmers-disk-controllers-1.png" alt="힙/디스크 컨트롤러1"></p>
한 번에 하나의 요청만을 수행할 수 있기 때문에 각각의 작업을 요청받은 순서대로 처리하면 다음과 같이 처리 됩니다.
<p align="center"><img src="/assets/img/posts/algorithm-programmers-disk-controllers-2.png" alt="힙/디스크 컨트롤러2"></p>
```
- A: 3ms 시점에 작업 완료 (요청에서 종료까지 : 3ms)
- B: 1ms부터 대기하다가, 3ms 시점에 작업을 시작해서 12ms 시점에 작업 완료(요청에서 종료까지 : 11ms)
- C: 2ms부터 대기하다가, 12ms 시점에 작업을 시작해서 18ms 시점에 작업 완료(요청에서 종료까지 : 16ms)
```
이 때 각 작업의 요청부터 종료까지 걸린 시간의 평균은 10ms(= (3 + 11 + 16) / 3)가 됩니다.<br>
하지만 A → C → B 순서대로 처리하면
<p align="center"><img src="/assets/img/posts/algorithm-programmers-disk-controllers-3.png" alt="힙/디스크 컨트롤러3"></p>
이렇게 A → C → B의 순서로 처리하면 각 작업의 요청부터 종료까지 걸린 시간의 평균은 9ms(= (3 + 7 + 17) / 3)가 됩니다.<br>
각 작업에 대해 [작업이 요청되는 시점, 작업의 소요시간]을 담은 2차원 배열 jobs가 매개변수로 주어질 때, 작업의 요청부터 종료까지 걸린 시간의 평균을 가장 줄이는 방법으로 처리하면 평균이 얼마가 되는지 return 하도록 solution 함수를 작성해주세요. (단, 소수점 이하의 수는 버립니다)<br>
<br>
#### **# 제한사항**<br>
- jobs의 길이는 1 이상 500 이하입니다.
- jobs의 각 행은 하나의 작업에 대한 [작업이 요청되는 시점, 작업의 소요시간] 입니다.
- 각 작업에 대해 작업이 요청되는 시간은 0 이상 1,000 이하입니다.
- 각 작업에 대해 작업의 소요시간은 1 이상 1,000 이하입니다.
- 하드디스크가 작업을 수행하고 있지 않을 때에는 먼저 요청이 들어온 작업부터 처리합니다.

<br>
#### **# 입출력 예**

| jobs | return |
| --- | --- |
| \[\[0, 3\], \[1, 9\], \[2, 6\]\] | 9 |

<br>
#### **# 입출력 예 설명**<br>
문제에 주어진 예와 같습니다.
- 0ms 시점에 3ms 걸리는 작업 요청이 들어옵니다.
- 1ms 시점에 9ms 걸리는 작업 요청이 들어옵니다.
- 2ms 시점에 6ms 걸리는 작업 요청이 들어옵니다.

---

#### **# 문제풀이**
```java
// Java
import java.util.*;

class Solution {
    public int solution(int[][] jobs) {

        int answer = 0;
        int time = 0;
        int finishCount = 0; // 실행 완료 개수
        PriorityQueue<Job> q = new PriorityQueue<>(); // 대기큐

        while (true) {

            for (int[] job : jobs) {
                // 시작시간이 현재시간보다 작거나 같은 경우 대기큐에 추가
                // 단, 이미 대기큐에 있을 경우를 대비해 대기큐에 들어간 값은 -1로 바꿔줌
                if (job[0] <= time && job[1] != -1) {
                    q.add(new Job(job[0], job[1]));
                    job[1] = -1;
                }
            }

            if (q.isEmpty()) {
                // 큐가 비어있고 실행 완료 개수와 jobs의 길이가 같으면 종료
                if (finishCount == jobs.length) {
                    break;
                }
                time++;
            } else {
                // 큐가 비어있지 않으면 제일 앞에 저장된 값을 가져옴
                Job nowJob = q.poll();
                // answer = 기존값 + 현재시간 + 수행시간 - 대기시간
                answer = answer + time + nowJob.jobTime - nowJob.inTime;
                finishCount++; // 실행 완료 개수 추가
                time = time + nowJob.jobTime; // 수행한 시간만큼 더해줌
            }
        }
        return answer / jobs.length;
    }
}

class Job implements Comparable<Job> {
    int inTime;
    int jobTime;

    Job(int inTime, int jobTime) {
        this.inTime = inTime;
        this.jobTime = jobTime;
    }

    @Override
    public int compareTo(Job target) {
        return this.jobTime - target.jobTime;
    }

}
```