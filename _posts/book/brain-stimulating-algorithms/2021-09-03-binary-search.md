---
layout: post
title: "[책/뇌를 자극하는 알고리즘] 6-2. 탐색 / 이진 탐색"
author: 임가람
date: "2021-09-03 22:07:00"
categories: [책, 뇌를 자극하는 알고리즘]
tags: [뇌를 자극하는 알고리즘]
---

## 이진 탐색(Binary Search)
정렬된 데이터 집합에서 사용할 수 있는 고속 탐색 알고리즘
- 데이터 집합의 중앙에 있는 요소 선택
- 중앙 요소의 값과 찾고자 하는 목표 값을 비교
- 목표 값이 중앙 요소의 값보다 작다면 중앙을 기준으로 데이터 집합의 왼편에 대해 새로 검색을 수행하고, 크다면 오른편에 대해 이진 탐색을 새로이 수행
- 찾고자 하는 값을 찾을 때까지 위의 과정을 반복

<br>

67을 찾는 예제
- 데이터 집합
  - 1, 7, 11, 12, 14, 23, 67, 139, 672, 871
- 중앙 요소(23)를 선택
- 67이 23보다 크므로 오른편 탐색
  - 67, 139, 672, 871
- 중앙 요소(672)를 선택
- 67이 23보다 작으므로 왼편 탐색
  - 67, 139
- 중앙 요소(139)를 선택
- 67이 139보다 작으므로 왼편 탐색
- 67 발견! 탐색 종료

<br>

---
## 이진 탐색의 성능 측정
이진 탐색은 처음 탐색을 시도할 때 범위가 $\dfrac{1}{2}$로 줄어든다. 그리고 그 다음 시도에서는 원본 데이터의 $\dfrac{1}{4}$로 줄어든다. 범위를 계속 줄이다 1이 되면  탐색을 중지한다.<br>
데이터 집합의 크기를 $n$, 탐색 반복 횟수를 $x$라고 하면, 마지막 데이터 범위의 크기 1은 $n$에 $\left( \dfrac{1}{2} \right)^x$와 같다<br>

$1 = n \times \left( \dfrac{1}{2} \right)^x$<br>
위 식을 정리하면<br>
$1 = n \times \dfrac{1}{2^x}$<br>
$2^x = n$<br>
즉 반복 횟수 $x = \log_2n$
<br>

---
## 이진 탐색의 구현
[Score.h 파일 다운받기](/assets/file/Score.h){:target="_blank"}<br>

BinarySearch.c
```c
#include <stdlib.h> 
#include <stdio.h>
#include "Score.h"

Score* BinarySearch( Score ScoreList[], int Size, double Target )
{
    int Left, Right, Mid;

    Left = 0;
    Right = Size - 1;

    while ( Left <= Right ) /* 탐색 범위의 크기가 0이 될 때까지 루프 반복 */
    {
        Mid = (Left + Right) / 2; /* 중앙 요소의 위치 계산 */

        if ( Target == ScoreList[Mid].score ) /* 중앙 요소가 담고 있는 값과 목표 값이 일치하면 해당 요소 반환  */
            return &( ScoreList[Mid] );
        else if ( Target > ScoreList[Mid].score )
            Left = Mid + 1;
        else
            Right = Mid - 1;
    }

    return NULL;
}

int CompareScore( const void *_elem1, const void *_elem2 ) 
{ 
    Score* elem1 = (Score*)_elem1; 
    Score* elem2 = (Score*)_elem2; 

    if ( elem1->score > elem2->score ) 
        return 1; 
    else if ( elem1->score < elem2->score ) 
        return -1; 
    else 
        return 0;     
} 

int main( void )
{
    int Length = sizeof DataSet / sizeof DataSet[0];     
    int i = 0;
    Score* found = NULL;

    /*  점수의 오름차순으로 정렬 */
    qsort( (void*)DataSet, Length, sizeof (Score), CompareScore );

    /*  671.78 점을 받은 학생 찾기 */
    found = BinarySearch( DataSet, Length, 671.78 );

    printf("found: %d %f \n", found->number, found->score );

    return 0;
}
```
실행결과
```
found: 1780 671.780000
```
<br>

---
## C 표준 라이브러리의 이진 탐색 함수: bsearch()

BinarySearch2.c
```c
#include <stdlib.h> 
#include <stdio.h>
#include "Score.h"

int CompareScore( const void *_elem1, const void *_elem2 ) 
{ 
    Score* elem1 = (Score*)_elem1; 
    Score* elem2 = (Score*)_elem2; 

    if ( elem1->score > elem2->score ) 
        return 1; 
    else if ( elem1->score < elem2->score ) 
        return -1; 
    else 
        return 0;     
} 

int main( void )
{
    int Length = sizeof DataSet / sizeof DataSet[0];     
    int i = 0;
    Score  target;
    Score* found  = NULL;

    /*  점수의 오름차순으로 정렬 */
    qsort( (void*)DataSet, Length, sizeof (Score), CompareScore );

    /*  671.78 점을 받은 학생 찾기 */
    target.number = 0;
    target.score  = 671.78;

    found = bsearch( 
                (void*)&target,     /* 찾고자 하는 목표 값 데이터의 주소 */
                (void*)DataSet,     /* 데이터 집합 배열의 주소 */
                Length,             /* 데이터 요소의 개수 */
                sizeof (Score),     /* 한 데이터 요소의 크기 */
                CompareScore );     /* 비교 함수에 대한 포인터 */
    
    printf("found: %d %f \n", found->number, found->score );

    return 0;
}
```

<br>

\# 참고<br>
[박상현 / 뇌를 자극하는 알고리즘](https://www.hanbit.co.kr/media/books/book_view.html?p_code=B3450156021){:target="_blank"}<br>