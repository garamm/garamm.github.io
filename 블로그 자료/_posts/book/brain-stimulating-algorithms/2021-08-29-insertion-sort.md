---
layout: post
title: "[책/뇌를 자극하는 알고리즘] 5-2. 정렬 / 삽입정렬"
author: 임가람
date: "2021-08-29 15:03:00"
categories: [책, 뇌를 자극하는 알고리즘]
tags: [뇌를 자극하는 알고리즘]
---

## 삽입 정렬(Insertion Sort)
데이터 집합을 순회하면서 정렬이 필요한 요소를 뽑아내어 다시 적당한 곳에 삽입해 나가는 알고리즘<br>
1. 데이터 집합에서 정렬 대상이 되는 요소들을 선택한다. 이 정렬 대상은 왼쪽부터 선택해나가며, 그 범위가 처음에는 2개지만, 알고리즘 반복 횟수가 늘어날 때마다 1개씩 커진다. 정렬 대상의 최대 범위는 '데이터 집합의 크기 - 1'이다.
2. 정렬 대상의 가장 오른쪽에 있는 요소가 정렬 대상 중 가장 큰 값을 갖고 있는지 확인한다. 만약 그렇지 않다면, 이 요소를 정렬 대상에서 뽑아내고, 이 요소가 위치할 적절한 곳(가장 왼쪽을 기준으로 자신보다 더 작은 요소가 없는 위치)을 정렬 대상 내에서 찾는다.
3. 뽑아 든 요소를 삽입할 적절한 곳을 찾았다면, 정렬 대상 내에서 삽입할 값보다 큰 값을 갖는 모든 요소를 한 자리씩 오른쪽으로 이동시키고, 새로 빈 자리에 삽입시킨다.
4. 전체 데이터 집합의 정렬이 완료될 때까지 위 절차를 반복한다.

<br>

### 삽입 정렬 예시
![삽입정렬-예시1](/assets/img/posts/2021-08-29-insertion-sort-1.png){: width="700" height="150" }<br>

맨 앞의 두 요소 중 오른쪽이 작으므로 오른쪽 카드를 뽑는다.<br>
![삽입정렬-예시2](/assets/img/posts/2021-08-29-insertion-sort-2.png){: width="700" height="150" }<br>

1이 들어갈 자리를 만들기 위해 5를 오른쪽으로 한 자리 옮긴다.<br>
![삽입정렬-예시3](/assets/img/posts/2021-08-29-insertion-sort-3.png){: width="700" height="150" }<br>

5가 이동하여 생긴 빈 자리에 1을 삽입한다.<br>
![삽입정렬-예시4](/assets/img/posts/2021-08-29-insertion-sort-4.png){: width="700" height="150" }<br>

첫 번째 반복이 끝났으므로 정렬 대상의 범위를 하나 더 늘린다. 마지막 요소가 6인데 바로 앞에 있는 요소 5와 비교하니 이미 정렬 된 상태이므로 다음 반복 단계로 넘어간다.<br>
![삽입정렬-예시5](/assets/img/posts/2021-08-29-insertion-sort-5.png){: width="700" height="150" }<br>

세번째 반복에서 정렬 대상의 마지막 요소는 4이고 바로 앞에 있는 6보다 작으므로 4를 뽑는다.<br>
![삽입정렬-예시6](/assets/img/posts/2021-08-29-insertion-sort-6.png){: width="700" height="150" }<br>

4가 들어갈 곳은 5 앞이므로 정렬 대상 중 4보다 큰 5와 6을 오른쪽으로 한 자리씩 옮긴다.<br>
![삽입정렬-예시7](/assets/img/posts/2021-08-29-insertion-sort-7.png){: width="700" height="150" }<br>

빈 자리에 4를 삽입하고 이런 식으로 계속 반복한다.<br>
![삽입정렬-예시8](/assets/img/posts/2021-08-29-insertion-sort-8.png){: width="700" height="150" }<br>
<br>

---
## 삽입 정렬의 속도
데이터 집합 내 요소들간의 비교는 정렬 대상의 범위가 2개일 때 한 번, 3개일 때 두 번, 4개일 때 세 번, ..., n개일 때 n-1번이다.<br>
$삽입 정렬의 비교 횟수 = 1 + 2 + \cdots + (n-2) + (n-1) = \dfrac{n(n-1)}{2}$<br>
식으로 확인하면 버블 정렬과 삽입 정렬은 동일해 보이지만, 버블 정렬은 데이터 집합이 정렬되어 있든 아니든 반드시 n(n-1)/2회 만큼 비교를 하지만, 삽입 정렬은 데이터 집합이 정렬되어 있는 경우에는 한번도 비교를 거치지 않는다.<br>
그러므로 삽입 정렬의 최선의 경우와 최악의 경우에 수행하는 비교 횟수의 평균을 내면 $\dfrac{(n(n-1)/2)+(n-1)}{2} = \dfrac{(n^2+n-2)}{2}$회 이다.<br>
그러므로 두 알고리즘의 성능이 똑같아 보이지만, 평균적으로는 삽입 정렬이 더 나은 성능을 보인다.<br>
<br>

---
## 소스코드
InsertionSort.c
```c
#include <stdio.h> 
#include <string.h> 

void InsertionSort(int DataSet[], int Length) 
{ 
    int i = 0; 
    int j = 0; 
    int value = 0; 

    for ( i=1; i<Length; i++ ) 
    { 
        if ( DataSet[i-1] <= DataSet[i] ) 
            continue; 

        value = DataSet[i]; 

        for ( j=0; j<i; j++ )  
        {             
            if (DataSet[j] > value)  
            { 
                /* memmove() 함수: C 표준 함수로써, 메모리의 내용을 이동시키는 기능을 수행 */
                memmove(&DataSet[j+1], &DataSet[j], sizeof(DataSet[0]) * (i-j)); 
                DataSet[j] = value; 
                break;     
            } 
        } 
    } 
} 

int main( void ) 
{ 
    int DataSet[] = {6, 4, 2, 3, 1, 5}; 
    int Length = sizeof DataSet / sizeof DataSet[0];     
    int i = 0; 

    InsertionSort(DataSet, Length); 

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