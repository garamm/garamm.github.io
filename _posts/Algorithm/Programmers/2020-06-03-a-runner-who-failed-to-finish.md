---
layout: post
title: "[프로그래머스] 해시/완주하지 못한 선수"
date: "2020-06-03 00:00"
updated: []
categories: ["Algorithm", "Programmers"]
---

#### **# 문제 설명**<br>
수많은 마라톤 선수들이 마라톤에 참여하였습니다. 단 한 명의 선수를 제외하고는 모든 선수가 마라톤을 완주하였습니다.<br>
마라톤에 참여한 선수들의 이름이 담긴 배열 participant와 완주한 선수들의 이름이 담긴 배열 completion이 주어질 때, 완주하지 못한 선수의 이름을 return 하도록 solution 함수를 작성해주세요.<br>
<br>
#### **# 제한사항**<br>
- 마라톤 경기에 참여한 선수의 수는 1명 이상 100,000명 이하입니다.
- completion의 길이는 participant의 길이보다 1 작습니다.
- 참가자의 이름은 1개 이상 20개 이하의 알파벳 소문자로 이루어져 있습니다.
- 참가자 중에는 동명이인이 있을 수 있습니다.

<br>
#### **# 입출력 예**

| participant | completion | return |
| --- | --- | --- |
| \["leo", "kiki", "eden"\] | \["eden", "kiki"\] | "leo" |
| \["marina", "josipa", "nikola", "vinko", "filipa"\] | \["josipa", "filipa", "marina", "nikola"\] | "vinko" |
| \["mislav", "stanko", "mislav", "ana"\] | \["stanko", "ana", "mislav"\] | "mislav" |

<br>
#### **# 입출력 예 설명**<br>
**입출력 예 #1**<br>
"leo"는 참여자 명단에는 있지만, 완주자 명단에는 없기 때문에 완주하지 못했습니다.<br>
<br>
**입출력 예 #2**<br>
"vinko"는 참여자 명단에는 있지만, 완주자 명단에는 없기 때문에 완주하지 못했습니다.<br>
<br>
**입출력 예 #3**<br>
"mislav"는 참여자 명단에는 두 명이 있지만, 완주자 명단에는 한 명밖에 없기 때문에 한명은 완주하지 못했습니다.<br>
<br>
[출처](http://hsin.hr/coci/archive/2014_2015/contest2_tasks.pdf){:target="_blank"}

---

#### **# 문제풀이**
```java
// Java, 내 코드(해시 미사용)
import java.util.Arrays;

class Solution {
    public String solution(String[] participant, String[] completion) {
        
        String answer = "";

        Arrays.sort(participant); // 참가자 정렬
        Arrays.sort(completion); // 완주자 정렬

        int i = 0;
        for(i=0; i<completion.length; i++) {
            // 두 리스트의 같은 index의 값이 같지 않으면 완주하지 못한 선수
            if(!completion[i].equals(participant[i])) {
                return participant[i];
            }
        }
        return participant[i];
    }
}
```
```java
// Java, 다른 사람 코드(해시 사용)
import java.util.HashMap;

class Solution {
    public String solution(String[] participant, String[] completion) {
        String answer = "";
        HashMap<String, Integer> hm = new HashMap<>();
        // getOrDefault() : 찾는 키가 존재하면 해당 키의 값을 반환, 없으면 기본값을 반환
        // 모든 선수를 hash에 저장하는데, 만약 hm에 키가 없으면 0을 넣고, 키가 있으면 1을 더함
        for (String player : participant) hm.put(player, hm.getOrDefault(player, 0) + 1);
        // 완주한 선수의 key값을 모두 1씩 빼준다 
        for (String player : completion) hm.put(player, hm.get(player) - 1);
   
        // hm을 조회해서 값이 0이 아니면 완주하지 못한 선수
        for (String key : hm.keySet()) {
            if (hm.get(key) != 0){
                answer = key;
            }
        }
        return answer;
    }
}
```