---
layout: post
title: "[알고리즘공부] 퀵정렬"
author: 임가람
date: "2020-05-23 23:48:00"
categories: [알고리즘, 공부]
tags: [정렬]
---

### 정렬 방법
- 1\. 피벗(pivot)을 설정(보통 첫번째 원소를 피벗으로 설정)
- 2\. 두개의 숫자를 찾는다.
  - 2-1\. 피벗을 기준으로 왼쪽에서 오른쪽으로 가면서 피벗보다 큰 수를 찾는다.
  - 2-2\. 피벗을 기준으로 오른쪽에서 왼쪽으로 가면서 피벗보다 작은 수를 찾는다.
- 3\. 찾은 두 숫자의 위치를 바꿔준다.
- 4\. 2, 3번의 작업을 반복하다가, 2-1과 2-2를 진행하는 도중에 엇갈리는 경우엔 피벗과 작은수를 비교하여 바꿔준다.
- 5\. 엇갈린 경우 피벗은 정렬이 완료됐다고 판정하며, 왼쪽집합과 오른쪽집합을 나눠서 각각의 집합에서 피벗을 재설정하여 반복한다.

### 예시
- 9 6 7 3 5 (초기상태)
- <span style="color: red;">9</span> 6 7 3 5<br>1: 피벗 설정
- <span style="color: red;">9</span> 6 7 3 [5]<br>2-1: 왼쪽에서 오른쪽으로 가면서 피벗보다 큰 수를 찾는다.(없음)<br>2-2: 오른쪽에서 왼쪽으로 가면서 작은 수를 찾는다. (5)
- <span style="color: red;">9</span> 6 7 3 [5]<br>4: 엇갈렸으므로 피벗과 작은수의 위치를 바꿔준다.
- <span style="color: red;">5</span> 6 7 3 <b>9</b><br>5: 9는 정렬이 완료됐고, 피벗을 재설정해준다.
- <span style="color: red;">5</span> [6] 7 [3] <b>9</b><br>2-1, 2-2: 6과 3을 찾는다.
- <span style="color: red;">5</span> 3 7 6 <b>9</b><br>3: 자리를 바꿔준다.
- <span style="color: red;">5</span> [3] [7] 6 <b>9</b><br>2-1, 2-2: 7과 3을 찾는다.
- 3 <span style="color: red;">5</span> 7 6 <b>9</b><br>4: 엇갈렸으므로 피벗과 작은수의 위치를 바꿔준다.
- 3 <b>5</b> 7 6 <b>9</b><br>5: 5는 정렬이 완료됐고, 피벗을 재설정해준다.
- <span style="color: red;">3</span> <b>5</b> <span style="color: red;">7</span> 6 <b>9</b><br>5: 5는 정렬이 완료됐고, 피벗을 재설정해준다.
- <b>3</b> <b>5</b> 6 <span style="color: red;">7</span> <b>9</b><br>왼쪽그룹은 하나이므로 정렬 완료, 오른쪽그룹은 엇갈렸으므로 위치를 바꿔준다.
- <b>3</b> <b>5</b> <b>6</b> <b>7</b> <b>9</b><br>정렬완료

### 소스코드

Java
```java
import java.util.Arrays;

public class Solve {

    public static void main(String[] args) {
        int arr[] = {9, 6, 7, 3, 5};
        quickSort(arr, 0, arr.length - 1);
        System.out.println("결과 : " + Arrays.toString(arr));
    }

    public static void quickSort(int[] arr, int start, int end) {
        if (start >= end) {
            return; // 원소가 1개인 경우 바로 return
        }

        int key = start; // 키는 첫번째 원소
        int i = start + 1, j = end, temp;

        while (i <= j) { // 엇갈릴때까지 반복
            while (i <= end && arr[i] <= arr[key]) { // 키보다 큰 값을 만날 때까지 반복
                i++;
            }
            while (j > start && arr[j] >= arr[key]) { // 키보다 작은 값을 만날 때까지 반복
                j--;
            }
            if (i > j) { // 엇갈린 상태라면 키와 자리바꿈
                temp = arr[j];
                arr[j] = arr[key];
                arr[key] = temp;
            } else { // 엇갈리지 않았다면 i, j 교체
                temp = arr[i];
                arr[i] = arr[j];
                arr[j] = temp;
            }

            quickSort(arr, start, j - 1);
            quickSort(arr, j + 1, end);
        }
    }
}
```

\# 참고<br>
[블로그: 안경잡이개발자](https://blog.naver.com/ndb796/221226813382){:target="_blank"}<br>
[유튜브: 안경잡이개발자](https://www.youtube.com/watch?v=O-O-90zX-U4&list=PLRx0vPvlEmdDHxCvAQS1_6XV4deOwfVrz&index=5){:target="_blank"}