---
layout: post
title: "[프로그래머스] 정렬/가장큰 수"
date: "2020-05-29 21:55"
updated: []
categories: ["Algorithm", "Programmers"]
---

#### **# 문제 설명**<br>
0 또는 양의 정수가 주어졌을 때, 정수를 이어 붙여 만들 수 있는 가장 큰 수를 알아내 주세요.
예를 들어, 주어진 정수가 [6, 10, 2]라면 [6102, 6210, 1062, 1026, 2610, 2106]를 만들 수 있고, 이중 가장 큰 수는 6210입니다.<br>
0 또는 양의 정수가 담긴 배열 numbers가 매개변수로 주어질 때, 순서를 재배치하여 만들 수 있는 가장 큰 수를 문자열로 바꾸어 return 하도록 solution 함수를 작성해주세요<br>
<br>
#### **# 제한사항**<br>
- numbers의 길이는 1 이상 100,000 이하입니다.
- numbers의 원소는 0 이상 1,000 이하입니다.
- 정답이 너무 클 수 있으니 문자열로 바꾸어 return 합니다.

<br>
#### **# 입출력 예**

| numbers | return |
| --- | --- |
| \[6, 10, 2\] | 6210 |
| \[3, 30, 34, 5, 9\] | 9534330 |

---

#### **# 문제풀이**
```java
// Java
import java.util.Arrays;

class Solution {
    public String solution(int[] numbers) {
        String answer = "";
        
        String[] resultArr = new String[numbers.length]; // 정렬 결과를 저장할 배열
        for (int i = 0; i < numbers.length; i++) { // int 배열을 String 배열로 변환
            resultArr[i] = String.valueOf(numbers[i]);
        }

        // 두 숫자를 합치는데 a+b, b+a를 비교하여 더 큰 숫자를 저장
        Arrays.sort(resultArr, (o1, o2) -> (o2 + o1).compareTo(o1 + o2));

        if (resultArr[0].equals("0")) { // 숫자의 맨앞자리가 0이면 0으로 return
            return "0";
        }

        StringBuilder builder = new StringBuilder(); // 숫자 string 만들기
        for (int i = 0; i < resultArr.length; i++) {
            builder.append(resultArr[i]);
        }
        answer = builder.toString();
        return answer;
    }
}
```