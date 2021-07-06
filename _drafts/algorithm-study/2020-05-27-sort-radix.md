---
layout: post
category: "algorithm-study"
title: "기수정렬"
date: "2020-05-27"
---

### 기수정렬이란
- 자릿수를 기준으로 차례대로 데이터를 정렬하는 알고리즘

<br><br>

### 정렬 방법
 - 1의자리를 기준으로 정렬한다.
 - 10의자리를 기준으로 정렬한다.
 - 100의자리를 기준으로 정렬한다.
 - 반복...
 
<br><br>

### 예시
- 초기상태
  - 6 2794 93 105 804
- 1의자리 기준으로 정렬
  
  |0|1|2|3|4|5|6|7|8|9|
  |--|--|--|--|--|--|--|--|--|--|
  ||||93|2794<br>804|105|6||||
  
- 10의자리 기준으로 정렬
  - 93 2794 804 105 6
  
  |0|1|2|3|4|5|6|7|8|9|
  |--|--|--|--|--|--|--|--|--|--|
  |804<br>105<br>6|||||||||93<br>2794|

- 100의자리 기준으로 정렬
  - 804 105 6 93 2794
  
  |0|1|2|3|4|5|6|7|8|9|
  |--|--|--|--|--|--|--|--|--|--|
  |6<br>93|105||||||2794|804||

- 1000의자리 기준으로 정렬
  - 6 93 105 2794 804
  
  |0|1|2|3|4|5|6|7|8|9|
  |--|--|--|--|--|--|--|--|--|--|
  |6<br>93<br>105<br>804||2794||||||||

- 정렬결과
  - 6 93 105 804 2794

<br><br>

### 소스코드

```java
import java.util.Arrays;

public class Solve {

  public static void main(String[] args) {
    int[] array = {6, 2794, 93, 105, 804};
    System.out.println("정렬 전 : " + Arrays.toString(array));
    radixSort(array);
  }

  public static void radixSort(int[] array) {
    final int MAX_LENGTH = Solve.getMaxLength(array), myArrLen = array.length;
    int myRadix = 1;
    int[] sortedArray = new int[myArrLen], counts;
    for (int p = 0; p < MAX_LENGTH; p++) {
      counts = new int[10];
      for (int numOfTemp : array) {
        counts[(numOfTemp / myRadix) % 10]++;
      }
      for (int i = 1; i < 10; i++)
        counts[i] += counts[i - 1];
      for (int i = myArrLen - 1; i >= 0; i--) {
        sortedArray[counts[(array[i] / myRadix) % 10]-- - 1] = array[i];
      }
      array = sortedArray;
      sortedArray = new int[myArrLen];
      myRadix *= 10;
    }
    System.out.println("정렬 후 : " + Arrays.toString(array));
  }

  public static int getMaxLength(int[] array) {
    int max = 0;
    for (int numOfTemp : array) {
      if (max < numOfTemp)
        max = numOfTemp;
    }
    return (int) Math.log10((double) max) + 1;
  }
}
```

[참고](http://blog.naver.com/PostView.nhn?blogId=xxxstarxxx&logNo=220961546548&parentCategoryNo=&categoryNo=87&viewDate=&isShowPopularPosts=true&from=search)