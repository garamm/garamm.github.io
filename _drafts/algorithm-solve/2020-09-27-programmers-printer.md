---
layout: post
category: "algorithm-solve"
title: "[프로그래머스] 스택,큐/프린터"
date: "2020-09-27"
---

### 문제 설명
일반적인 프린터는 인쇄 요청이 들어온 순서대로 인쇄합니다. 그렇기 때문에 중요한 문서가 나중에 인쇄될 수 있습니다. 이런 문제를 보완하기 위해 중요도가 높은 문서를 먼저 인쇄하는 프린터를 개발했습니다. 이 새롭게 개발한 프린터는 아래와 같은 방식으로 인쇄 작업을 수행합니다.<br>

```
1. 인쇄 대기목록의 가장 앞에 있는 문서(J)를 대기목록에서 꺼냅니다.
2. 나머지 인쇄 대기목록에서 J보다 중요도가 높은 문서가 한 개라도 존재하면 J를 대기목록의 가장 마지막에 넣습니다.
3. 그렇지 않으면 J를 인쇄합니다.
```

예를 들어, 4개의 문서(A, B, C, D)가 순서대로 인쇄 대기목록에 있고 중요도가 2 1 3 2 라면 C D A B 순으로 인쇄하게 됩니다.<br>
내가 인쇄를 요청한 문서가 몇 번째로 인쇄되는지 알고 싶습니다. 위의 예에서 C는 1번째로, A는 3번째로 인쇄됩니다.<br>
현재 대기목록에 있는 문서의 중요도가 순서대로 담긴 배열 priorities와 내가 인쇄를 요청한 문서가 현재 대기목록의 어떤 위치에 있는지를 알려주는 location이 매개변수로 주어질 때, 내가 인쇄를 요청한 문서가 몇 번째로 인쇄되는지 return 하도록 solution 함수를 작성해주세요.

<br>

### 제한사항
- 현재 대기목록에는 1개 이상 100개 이하의 문서가 있습니다.
- 인쇄 작업의 중요도는 1~9로 표현하며 숫자가 클수록 중요하다는 뜻입니다.
- location은 0 이상 (현재 대기목록에 있는 작업 수 - 1) 이하의 값을 가지며 대기목록의 가장 앞에 있으면 0, 두 번째에 있으면 1로 표현합니다.

<br>

### 입출력 예

|priorities|location|return|
|---|---|---|
|[2,1,3,2]|2|1|
|[1,1,9,1,1,1]|0|5|

<br>

### 입출력 예 설명
- 예제 #1
  - 문제에 나온 예와 같습니다.
- 예제 #2
  - 6개의 문서(A, B, C, D, E, F)가 인쇄 대기목록에 있고 중요도가 1 1 9 1 1 1 이므로 C D E F A B 순으로 인쇄합니다.

<br>

### 문제풀이

```java
public int solution(int[] priorities, int location) {
  int answer = 0;

  Queue<Map<String, Integer>> queue = new LinkedList<>(); //  대기 문서 큐
  ArrayList<Integer> printed = new ArrayList<>(); // 출력된 문서 리스트

  // 문서 리스트를 큐에 넣고, 대기 문서 순서를 리스트로 저장
  for (int i = 0; i < priorities.length; i++) {
      Map<String, Integer> map = new HashMap<>();
      map.put("priority", priorities[i]); // 우선순위 저장
      map.put("location", i); // 위치 저장
      queue.add(map);
  }

  while (!queue.isEmpty()) { // 대기큐가 빌때까지 반복
      Map<String, Integer> nowDoc = queue.poll(); // 대기큐의 가장 첫번째 문서 선택
      boolean isPrint = true; // 현재 문서를 프린트할지, 다음 대기큐로 넘길지 저장하는 변수

      for (Map<String, Integer> selectedMap : queue) { // 큐 안의 내용 반복
          // 만약 현재 선택된 문서가 우선순위가 크지 않으면 false
          if (nowDoc.get("priority") < selectedMap.get("priority")) {
              isPrint = false;
              break;
          }
      }

      int nowPosition = nowDoc.get("location"); // 현재 문서의 순서
      if (isPrint) { // 출력이면
          printed.add(nowDoc.get("priority")); // 출력된 문서 리스트에 저장
          if (nowPosition == location) { // 만약 현재 문서순서가 location 값과 같으면 종료
              answer = printed.size();
              break;
          }
      } else { // 출력이 아니면
          queue.add(nowDoc); // 대기 큐의 마지막으로 추가
      }
  }
  return answer;
}
```