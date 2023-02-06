---
layout: post
title: "[책/뇌를 자극하는 알고리즘] 5-1. 정렬 / 버블정렬"
author: 임가람
date: "2021-08-28 11:27:00"
categories: [책, 뇌를 자극하는 알고리즘]
tags: [뇌를 자극하는 알고리즘]
---

## 버블 정렬(Bubble Sort)
데이터 집합을 순회하면서 집합 내의 이웃 요소들끼리의 교환을 통해 정렬을 수행
<br>

### 버블 정렬 예시

![버블정렬-예시1](/assets/img/posts/2021-08-28-sort-bubble-1.png){: width="700" height="150" }<br>
위 데이터를 오름차순 정렬한다.<br>
오름차순으로 정렬하려면 왼쪽의 요소가 오른쪽 요소보다 작아야 한다.<br>
그러므로 제일 왼쪽의 요소 5와 그 다음 요소 1을 비교한다.<br>
왼쪽의 요소가 더 크므로 두 요소를 교환해준다.<br>
![버블정렬-예시2](/assets/img/posts/2021-08-28-sort-bubble-2.png){: width="700" height="150" }<br>

첫 번째 교환을 끝내고 다음 요소를 비교한다.<br>
![버블정렬-예시3](/assets/img/posts/2021-08-28-sort-bubble-3.png){: width="700" height="150" }<br>

왼쪽이 5, 오른쪽이 6이므로 교환할 필요 없이 다음 요소로 넘어가 비교를 한다.<br>
왼쪽이 6, 오른쪽이 4이므로 교환을 수행한다.<br>
![버블정렬-예시4](/assets/img/posts/2021-08-28-sort-bubble-4.png){: width="700" height="150" }

위 방식으로 요소를 순회하면서 이웃간의 비교/교환을 수행한다.<br>
![버블정렬-예시5](/assets/img/posts/2021-08-28-sort-bubble-5.png){: width="700" height="150" }

<br>

모든 요소를 확인해도 아직 정렬이 끝나지 않았으므로 마지막 요소를 제외한 나머지 요소들을 다시 버블 정렬 수행한다.<br>
![버블정렬-예시6](/assets/img/posts/2021-08-28-sort-bubble-6.png){: width="700" height="150" }

<br>

---
## 버블 정렬의 속도
버블 정렬은 데이터 집합을 한번 순회할 때 마다 가장 큰 값이 가장 뒤에 놓이므로 정렬해야 하는 데이터의 집합의 범위가 하나씩 줄어든다.<br>
그러므로 데이터 집합의 범위를 n개라고 한다면, n-1만큼 반복하면 정렬이 끝난다.<br>
이 식을 일반화하면<br>
버블 정렬의 비교 횟수<br>
$ = (n-1) + (n-2) + (n-3) + ... + (n-(n-2)) + (n-(n-1)) \\\\\\
 = (n-1) + (n-2) + (n-3) + ... + 3 + 2 + 1  \\\\\\
 = (n-1) \times \dfrac{n}{2} = \dfrac{n(n-1)}{2} \\\\\\
$
(가우스 등차수열의 합 참고)
<br>

---
## 소스코드
BubbleSort.c
```c
#include <stdio.h> 

void BubbleSort(int DataSet[], int Length) 
{ 
    int i = 0; 
    int j = 0; 
    int temp = 0; 
    /* 데이터 집합의 크기만큼 for 루프 실행 */
    for ( i=0; i<Length-1; i++ ) 
    {
        /* 바깥의 for 루프가 한번 실행될 때마다 반복 횟수가 줄어든다. */
        for ( j=0; j<Length-(i+1); j++ )  
        { 
            if ( DataSet[j] > DataSet[j+1] ) 
            { 
                temp = DataSet[j+1]; 
                DataSet[j+1] = DataSet[j]; 
                DataSet[j] = temp; 
            } 
        } 
    } 
}

int main( void ) 
{ 
    int DataSet[] = {6, 4, 2, 3, 1, 5}; 
    int Length = sizeof DataSet / sizeof DataSet[0];     
    int i = 0; 

    BubbleSort(DataSet, Length); 

    for ( i=0; i<Length; i++ ) 
    { 
        printf("%d ", DataSet[i]); 
    } 

    printf("\n"); 

    return 0; 
}
```
실행결과
```
1 2 3 4 5 6
```

<br>

\# 참고<br>
[박상현 / 뇌를 자극하는 알고리즘](https://www.hanbit.co.kr/media/books/book_view.html?p_code=B3450156021){:target="_blank"}<br>