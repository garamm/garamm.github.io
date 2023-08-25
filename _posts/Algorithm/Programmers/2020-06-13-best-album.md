---
layout: post
title: "[프로그래머스] 해시/베스트앨범"
date: "2020-06-13 00:00"
updated: []
categories: ["Algorithm", "Programmers"]
---

#### **# 문제 설명**<br>
스트리밍 사이트에서 장르 별로 가장 많이 재생된 노래를 두 개씩 모아 베스트 앨범을 출시하려 합니다. 노래는 고유 번호로 구분하며, 노래를 수록하는 기준은 다음과 같습니다.
1. 속한 노래가 많이 재생된 장르를 먼저 수록합니다.
2. 장르 내에서 많이 재생된 노래를 먼저 수록합니다.
3. 장르 내에서 재생 횟수가 같은 노래 중에서는 고유 번호가 낮은 노래를 먼저 수록합니다.

노래의 장르를 나타내는 문자열 배열 genres와 노래별 재생 횟수를 나타내는 정수 배열 plays가 주어질 때, 베스트 앨범에 들어갈 노래의 고유 번호를 순서대로 return 하도록 solution 함수를 완성하세요.<br>
<br>
#### **# 제한사항**<br>
- genres[i]는 고유번호가 i인 노래의 장르입니다.
- plays[i]는 고유번호가 i인 노래가 재생된 횟수입니다.
- genres와 plays의 길이는 같으며, 이는 1 이상 10,000 이하입니다.
- 장르 종류는 100개 미만입니다.
- 장르에 속한 곡이 하나라면, 하나의 곡만 선택합니다.
- 모든 장르는 재생된 횟수가 다릅니다.

<br>
#### **# 입출력 예**

| genres | plays | return |
| --- | --- | --- |
| \["classic", "pop", "classic", "classic", "pop"\] | \[500, 600, 150, 800, 2500\] | \[4, 1, 3, 0\] |

<br>
#### **# 입출력 예 설명**<br>
classic 장르는 1,450회 재생되었으며, classic 노래는 다음과 같습니다.
- 고유 번호 3: 800회 재생
- 고유 번호 0: 500회 재생
- 고유 번호 2: 150회 재생

<br>
pop 장르는 3,100회 재생되었으며, pop 노래는 다음과 같습니다.
- 고유 번호 4: 2,500회 재생
- 고유 번호 1: 600회 재생

<br>
따라서 pop 장르의 [4, 1]번 노래를 먼저, classic 장르의 [3, 0]번 노래를 그다음에 수록합니다.
- 장르 별로 가장 많이 재생된 노래를 최대 두 개까지 모아 베스트 앨범을 출시하므로 2번 노래는 수록되지 않습니다.

---

#### **# 문제풀이**
```java
// Java
import java.util.*;

class Solution {
    public int[] solution(String[] genres, int[] plays) {
        // HashMap<장르, List<HashMap<순번, 플레이횟수>> 의 구조를 갖는 HashMap 생성
        HashMap<String, ArrayList<HashMap<String, Integer>>> map = new HashMap<>();
        HashMap<String, Integer> average = new HashMap<>(); // 장르별 플레이 횟수 합계 저장하는 해시맵

        // 위의 구조에 맞게 데이터 추가
        for (int i = 0; i < genres.length; i++) {
            String g = genres[i];
            ArrayList<HashMap<String, Integer>> list = new ArrayList<>();
            if (map.get(g) != null) {
                list = map.get(g);
                average.put(g, average.get(g) + plays[i]);
            } else {
                average.put(g, plays[i]);
            }
            HashMap<String, Integer> m = new HashMap<>();
            m.put("index", i);
            m.put("plays", plays[i]);
            list.add(m);
            map.put(g, list);
        }

        // 플레이 횟수의 총합이 많은 순서대로 정렬
        List<Map.Entry<String, Integer>> list_entries = new ArrayList<Map.Entry<String, Integer>>(average.entrySet());
        Collections.sort(list_entries, new Comparator<Map.Entry<String, Integer>>() {
            public int compare(Map.Entry<String, Integer> obj1, Map.Entry<String, Integer> obj2) {
                return obj2.getValue().compareTo(obj1.getValue());
            }
        });

        for (String s : map.keySet()) {
            // 장르별 플레이 횟수가 많은 순서대로 정렬
            map.get(s).sort(Comparator.comparing(
                    m -> m.get("plays"),
                    Comparator.nullsLast(Comparator.reverseOrder()))
            );
        }

        List<Integer> result = new ArrayList<>();

        for(Map.Entry e : list_entries) {
            List<HashMap<String, Integer>> playList = map.get(e.getKey());
            result.add(playList.get(0).get("index"));
            if(playList.size() != 1){
                result.add(playList.get(1).get("index"));
            }
        }
        return result.stream().mapToInt(Integer::intValue).toArray();
    }
}
```