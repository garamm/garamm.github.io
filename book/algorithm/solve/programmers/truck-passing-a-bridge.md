---
layout: solve
title: "다리를 지나는 트럭"
date: "2020-06-19"
---

### 문제 설명
트럭 여러 대가 강을 가로지르는 일 차선 다리를 정해진 순으로 건너려 합니다. 모든 트럭이 다리를 건너려면 최소 몇 초가 걸리는지 알아내야 합니다. 트럭은 1초에 1만큼 움직이며, 다리 길이는 bridge_length이고 다리는 무게 weight까지 견딥니다.<br>
※ 트럭이 다리에 완전히 오르지 않은 경우, 이 트럭의 무게는 고려하지 않습니다.<br>
<br>
예를 들어, 길이가 2이고 10kg 무게를 견디는 다리가 있습니다. 무게가 [7, 4, 5, 6]kg인 트럭이 순서대로 최단 시간 안에 다리를 건너려면 다음과 같이 건너야 합니다.<br>
경과시간|다리를 지난 트럭|다리를 건너는 트럭|대기트럭
|---|---|---|---|
|0|[]|[]|[7,4,5,6]|
|1~2|[]|[7]|[4,5,6]|
|3|[7]|[4]|[5,6]|
|4|[7]|[4,5]|[6]|
|5|[7,4]|[5]|[6]|
|6~7|[7,4,5]|[6]|[]|
|8|[7,4,5,6]|[]|[]|

따라서, 모든 트럭이 다리를 지나려면 최소 8초가 걸립니다.<br>
solution 함수의 매개변수로 다리 길이 bridge_length, 다리가 견딜 수 있는 무게 weight, 트럭별 무게 truck_weights가 주어집니다. 이때 모든 트럭이 다리를 건너려면 최소 몇 초가 걸리는지 return 하도록 solution 함수를 완성하세요.

<br>

### 제한사항
- bridge_length는 1 이상 10,000 이하입니다.
- weight는 1 이상 10,000 이하입니다.
- truck_weights의 길이는 1 이상 10,000 이하입니다.
- 모든 트럭의 무게는 1 이상 weight 이하입니다.

<br>

### 입출력 예

|bridge_length|weight|truck_weights|return|
|---|---|---|---|
|2|10|[7,4,5,6]|8|
|100|100|[10]|101|
|100|100|[10,10,10,10,10,10,10,10,10,10]|110|

<br>

### 문제풀이

```java
import java.util.ArrayList;
import java.util.LinkedList;
import java.util.Queue;

class Solution {

    public static void main(String[] args) {

        int[] arr1 = {7, 4, 5, 7};
        int[] arr2 = {10};
        int[] arr3 = {10, 10, 10, 10, 10, 10, 10, 10, 10, 10};
        System.out.println(solution(2, 10, arr1));
        System.out.println(solution(100, 100, arr2));
        System.out.println(solution(100, 100, arr3));
    }

    public static int solution(int bridge_length, int weight, int[] truck_weights) {
        int answer = 0;

        Queue<Integer> bridging = new LinkedList<>(); // 다리를 건너고 있는 트럭을 저장할 큐
        ArrayList<Integer> passTime = new ArrayList<>(); // 큐에 담긴 트럭이 몇초동안 지났는지 저장하는 리스트
        ArrayList<Integer> passed = new ArrayList<>(); // 다리를 건넌 트럭을 저장하는 리스트
        int targetPosition = 0; // 현재 다리를 건널 트럭의 위치를 저장하는 변수
        boolean isFirst = true; // 처음 반복인지 구분해주는 변수
        int sumTruckWeight = 0; // 다리를 건너는중인 트럭의 무게를 저장하는 변수

        while (true) {
            // 1. 모든 차가 다리를 건넜으면 break
            if (passed.size() == truck_weights.length) {
                break;
            }

            // 2. 다리를 다 건넌 트럭은 큐에서 제거
            if (!isFirst && passTime.get(0) == bridge_length) {
                passTime.remove(0);
                sumTruckWeight = sumTruckWeight - bridging.element(); // peek() : 큐의 맨 앞에 있는 요소 반환
                passed.add(bridging.poll()); // poll() : 큐의 맨 앞에 있는 요소를 반환하고 큐에서 제거
            }

            // 3. 현재 신규로 트럭이 다리를 건널 수 있으면 큐에 넣기
            if (targetPosition < truck_weights.length && (isFirst || weight - sumTruckWeight >= truck_weights[targetPosition])) { // 처음이거나, 현재 다리를 건너고 있는 트럭의 총 무게와 최대 무게를 비교한 후 저장
                bridging.offer(truck_weights[targetPosition]); // offer() : 큐의 맨 뒤에 요소 삽입(성공 실패 여부를 boolean으로 return)
                passTime.add(0);
                sumTruckWeight += truck_weights[targetPosition];
                targetPosition++;
            }

            // 4. 시간++
            for (int i = 0; i < passTime.size(); i++) {
                passTime.set(i, passTime.get(i) + 1);
            }
            answer++;
            isFirst = false;
        }
        return answer;
    }
}
```