---
layout: post
title: "[알고리즘공부] 병합정렬"
author: 임가람
date: "2020-05-24 22:56:00"
categories: [알고리즘, 공부]
tags: [정렬]
---

### 정렬 방법
- 리스트를 반으로 나눌 수 없을때까지 반으로 계속 나눈다.
- 나눈 배열을 합치면서 정렬한다.

### 예시
- 8 6 7 3 5 2 1 4(초기상태)
- [8] [6] [7] [3] [5] [2] [1] [4] (크기가 1인 배열로 시작한다.)
- [6 8] [3 7] [2 5] [1 4] (나눠졌던 배열을 합치면서 크기 비교를 하여 정렬한다.)
- [3 6 7 8] [1 2 4 5] (나눠졌던 배열을 합치면서 크기 비교를 하여 정렬한다.)
- [1 2 3 4 5 6 7 8] (나눠졌던 배열을 합치면서 크기 비교를 하여 정렬한다.)

### 소스코드

Java
```java
import java.util.Arrays;

public class Solve {

    static int[] sorted = new int[8]; // 배열을 전역변수로 배치하고, 배열의 크기를 지정해줘야 함

    public static void main(String[] args) {

        int arr[] = {8, 6, 7, 3, 5, 2, 1, 4};
        mergeSort(arr, 0, arr.length - 1);
        System.out.println("결과 : " + Arrays.toString(sorted));
    }

    // 나눠졌던 2개의 배열을 합치는 작업
    public static void merge(int a[], int m, int middle, int n) {
        // m : 병합시 앞에 오는 배열, middle : 병합시 앞 배열과 뒤 배열을 이어주는 위치, n : 병합시 뒤에 오는 배열
        int i = m;
        int j = middle + 1;
        int k = m;

        // 작은 순서대로 배열에 삽입
        while (i <= middle && j <= n) {
            if (a[i] <= a[j]) {
                sorted[k] = a[i];
                i++;
            } else {
                sorted[k] = a[j];
                j++;
            }
            k++;
        }
        // 남은 데이터도 추가
        if (i > middle) {
            for (int t = j; t <= n; t++) {
                sorted[k] = a[t];
                k++;
            }
        } else {
            for (int t = i; t <= middle; t++) {
                sorted[k] = a[t];
                k++;
            }
        }

        // 정렬된 배열 삽입
        for (int t = m; t <= n; t++) {
            a[t] = sorted[t];
        }
    }

    public static void mergeSort(int a[], int m, int n) {
        if (m < n) { // 크기가 1보다 큰 경우
            int middle = (m + n) / 2; // 정중앙 찾기
            mergeSort(a, m, middle); // middle을 기준으로 왼쪽을 병합정렬 수행
            mergeSort(a, middle + 1, n); // middle을 기준으로 오른쪽을 병합정렬 수행
            merge(a, m, middle, n); // 수행 결과를 합침
        }
    }
}
// [1, 2, 3, 4, 5, 6, 7, 8]
```

\# 참고<br>
[블로그: 안경잡이개발자](https://blog.naver.com/ndb796/221227934987){:target="_blank"}