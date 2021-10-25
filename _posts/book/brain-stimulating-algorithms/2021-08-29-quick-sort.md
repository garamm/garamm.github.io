---
layout: post
title: "[책/뇌를 자극하는 알고리즘] 5-3. 정렬 / 퀵 정렬"
author: 임가람
date: "2021-08-29 20:17:00"
categories: [책, 뇌를 자극하는 알고리즘]
tags: [뇌를 자극하는 알고리즘]
---

## 퀵 정렬(Quick Sort)
분할 정보(Divide and Conquer)에 기반한 알고리즘<br>
분할정복: 적군의 전체를 공략하는 대신, 전체를 구성하는 구성 요소들을 나누어 잘게 쪼개진 적을 공략하는 전법<br>
<br>
퀵 정렬의 과정<br>

1. 데이터 집합 내에서 임의의 기준 요소를 선택하고, 기준 요소보다 작은 요소들은 순서에 관계없이 무조건 기준 요소의 왼편에, 큰 값은 오른편에 위치시킨다.
2. 나눈 데이터 집합들을 다시 1번 에서와 같이 임의의 기준 요소를 선택하고 같은 방법으로 데이터 집합을 분할한다.
3. 1, 2번의 과정을 데이터 집합을 나눌 수 없을 때까지 반복하면 정렬된 데이터 집합을 얻을 수 있다.

<br>

### 퀵 정렬 예시

![퀵정렬-예시1](/assets/img/posts/2021-08-29-quick-sort-1.png){: width="700" height="150" }<br>
요소 5를 기준으로 5보다 작은 요소들은 왼쪽에, 큰 요소들은 오른쪽에 모은다.<br>
이렇게 생긴 왼쪽과 오른쪽의 데이터 집합에 대해 분할을 수행한다. 우선 왼쪽을 분할하면 다음과 같다.<br>
![퀵정렬-예시2](/assets/img/posts/2021-08-29-quick-sort-2.png){: width="700" height="150" }<br>
분리를 위해서는 왼쪽 데이터 집합에서 기준 요소를 선택한다.<br>
![퀵정렬-예시3](/assets/img/posts/2021-08-29-quick-sort-3.png){: width="700" height="150" }<br>
그런데 요소 1보다 큰 요소는 모두 1의 오른쪽에 있으므로 이미 분할되어 있는 상태이다.<br>
그러므로 1을 제외한 나머지 요소 4, 3, 2에 대해 분할을 수행한다.<br>
![퀵정렬-예시4](/assets/img/posts/2021-08-29-quick-sort-4.png){: width="700" height="150" }<br>
4, 3, 2 중 첫 번째인 4를 기준 요소로 선택하여 분할을 수행한다.<br>
![퀵정렬-예시5](/assets/img/posts/2021-08-29-quick-sort-5.png){: width="700" height="150" }<br>
4보다 큰 요소는 없으므로 왼쪽에만 데이터 집합이 생긴다. 이제 3, 2에 대해 분할을 수행한다.<br>
![퀵정렬-예시6](/assets/img/posts/2021-08-29-quick-sort-6.png){: width="700" height="150" }<br>
3, 2 중 첫 번째에 위치한 3을 선택하여 분할을 수행한다.<br>
![퀵정렬-예시7](/assets/img/posts/2021-08-29-quick-sort-7.png){: width="700" height="150" }<br>
2는 3보다 작으니 3의 왼쪽에 두면 왼쪽 데이터 집합의 정렬이 끝난다.<br>
남아있는 오른쪽 데이터 집합의 분할을 진행한다.<br>
![퀵정렬-예시8](/assets/img/posts/2021-08-29-quick-sort-8.png){: width="700" height="150" }<br>
6을 기준 요소로 선택했는데 6보다 작은 요소가 없으므로 6의 오른쪽에 있는 데이터 집합에 대해 분할을 수행한다.<br>
![퀵정렬-예시9](/assets/img/posts/2021-08-29-quick-sort-9.png){: width="700" height="150" }<br>
8, 7, 9 중 첫 번째 요소인 8을 기준으로 지정하고 분할을 수행한다.<br>
![퀵정렬-예시10](/assets/img/posts/2021-08-29-quick-sort-10.png){: width="700" height="150" }<br>
더 이상 분할할 수 없으므로 정렬이 종료된다.<br>
![퀵정렬-예시11](/assets/img/posts/2021-08-29-quick-sort-11.png){: width="700" height="150" }<br>
<br>

참고) 기준 요소의 선택<br>
기준 요소는 데이터 집합에서 무작위로 뽑을 수도 있지만, 처음 데이터 집합의 처음 세 개 요소 중에서 중간 값을 기준 요소로 지정하면 처음 세 개 중의 중간 값을 찾기 위한 아주 작은 성능 비용을 지출하긴 하지만, 최악의 경우를 피할 수 있다.
<br>

---
## 퀵 정렬이 퀵 정렬을 호출합니다
퀵 정렬 소스코드 작성시 고려할 것
- 배열을 데이터 집합의 자료구조로 사용하면 링크드 리스트처럼 삽입/삭제가 자유롭지 못하기 때문에 분할을 어떻게 수행해야 하는가?
- 반복되는 분할 과정을 어떻게 처리할 것인가?
<br>
첫 번째 문제는 '수색 섬멸' 작전을 사용하여 정찰별 2명을 투입해서 해결한다.<br>
한 명은 왼쪽부터 오른쪽 방향으로 데이터 집합을 검사하면서 기준보다 큰 요소를 찾고, 또 한 명은 오른쪽부터 왼쪽 방향으로 검사하면서 기준보다 작은 요소를 찾는다. 그리고 두 정찰병이 찾아낸 두 요소를 교환한다. 그리고 서로 접선할 때까지 검사를 계속 진행하면서 교환해야 될 요소를 찾아내고 교환을 수행한다. 두 정찰병이 접선하면 기준 요소를 왼쪽 데이터 집합의 마지막 요소와 교환하는 것으로 종료한다.

![퀵정렬-고려사항](/assets/img/posts/2021-08-29-quick-sort-12.png){: width="700" height="150" }<br>
<br>
두 번째 문제는 함수 내에서 함수 자신을 호출하는 '재귀 호출(Recursive Call)' 기능을 사용하여 해결한다.<br>
<br>

---
## 소스코드
QuickSort.c
```c
#include <stdio.h>

void Swap( int* A, int* B )
{
    int Temp = *A;
    *A = *B;
    *B = Temp;
}

int Partition( int DataSet[], int Left, int Right )
{
    int First = Left;
    int Pivot = DataSet[First];

    ++Left;

    while( Left <= Right )
    {
        while( DataSet[Left] <= Pivot && Left < Right )
            ++Left;

        while( DataSet[Right] >= Pivot && Left <= Right )
            --Right;

        if ( Left < Right )
            Swap( &DataSet[Left], &DataSet[Right]);
        else
            break;
    }

    Swap( &DataSet[First], &DataSet[Right] );

    return Right;
}

void QuickSort(int DataSet[], int Left, int Right)
{
    if ( Left < Right )
    {
        int Index = Partition(DataSet, Left, Right);

        QuickSort( DataSet, Left, Index - 1 );
        QuickSort( DataSet, Index + 1, Right );
    }
}

int main( void )
{
    int DataSet[] = {6, 4, 2, 3, 1, 5};
    int Length = sizeof DataSet / sizeof DataSet[0];    
    int i = 0;

    QuickSort( DataSet, 0, Length-1 );

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

---
## 퀵 정렬의 성능
퀵 정렬은 데이터가 미리 정렬되어 있거나 또는 역순으로 정렬되어 있는 경우에는 최악의 성을 보이지만, 데이터 요소들이 흩어져 있는 경우에는 최고의 성능을 보인다.<br>
<br>

### 최선의 경우
![퀵정렬-최선의경우](/assets/img/posts/2021-08-29-quick-sort-13.png){: width="700" height="150" }<br>
위 그림을 보면 퀵 정렬이 한번 호출될 떄마다 데이터 집합이 $\frac{1}{2}$ 로 쪼개진다.<br>
위 상황을 토대로 성능 분석을 해보면 데이터 집합의 크기가 $n$개일 때, 퀵 정렬의 재귀 호출 깊이는 $\log_{2}{n}$이다.<br>
퀵 정렬의 첫 단계에서 수행하는 비교는 $n$회이고, 두 번째 단계는 $\frac{n}{2}+\frac{n}{2}=n$회이다. 세 번째 단계는 $\frac{n}{3}+\frac{n}{3}+\frac{n}{3}=n$회이다.<br>
그러므로 이상적인 경우에 퀵 정렬의 성능은 다음과 같다.<br>
이상적인 경우의 퀵 정렬의 비교 횟수 = 재귀 호출의 깊이 * 각 재귀 호출 단계에서의 비교 횟수 = $n \times \log_{2}{n} = n\log_{2}{n}$<br>
<br>

### 최악의 경우
![퀵정렬-최악의경우](/assets/img/posts/2021-08-29-quick-sort-14.png){: width="700" height="150" }<br>
퀵 정렬에게 있어 최악인 경우, 위 그림과 같이 매 재귀 호출마다 데이터 집합의 분할이 $1:n-1$로 이루어 진다.<br>
이렇게 되면 재귀 호출의 깊이는 $n-1$이 되고, 매 재귀 호출이 일어날 때마다 정렬 대상의 범위로 1씩 줄어든다.<br>
최악의 경우 퀵 정렬의 비교 횟수 = $(n-1) + (n-2) + (n-3) + \ldots + 3 + 2 1 = n\frac{n-1}{2} = \frac{n^2+n}{2}$<br>
<br>

### 평균의 경우
![퀵정렬-평균의경우](/assets/img/posts/2021-08-29-quick-sort-15.png){: width="700" height="150" }<br>
퀵 정렬은 평균적으로 $1.39n\log_{2}{n}$회 비교를 수행한다. 이것은 $n\log_2n$인 최선의 경우에 비해 39% 정도만 느리다.<br>
<br>

---
## C 표준 라이브러리의 퀵 정렬 함수
### qsort() 함수
```c
void qsort(
    void *base,     /* 데이터 집합 배열의 주소 */
    size_t num,     /* 데이터 요소의 개수 */
    size_t width,   /* 한 데이터 요소의 크기 */
    int (__cdecl *compare ) (const void *, const void *)  /* 비교 함수에 대한 포인터 */
)
```

### 소스코드
QuickSort2.c
```c
#include <stdlib.h> 
#include <stdio.h> 
#include <string.h> 

/*  리턴값이 
    < 0 이면, _elem1이 _elem2보다 작다. 
    0   이면, _elem1이 _elem2와 같다.  
    > 0 이면, _elem1이 _elem2보다 크다.  */
int CompareScore( const void *_elem1, const void *_elem2 ) 
{ 
    int* elem1 = (int*)_elem1; 
    int* elem2 = (int*)_elem2; 

    if ( *elem1 > *elem2) 
        return 1; 
    else if ( *elem1 < *elem2) 
        return -1; 
    else 
        return 0;     
} 

int main( void ) 
{ 
    int DataSet[] = {6, 4, 2, 3, 1, 5}; 
    int Length = sizeof DataSet / sizeof DataSet[0];     
    int i = 0; 

    qsort((void*)DataSet, Length, sizeof (int), CompareScore ); 

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