---
layout: post
title: "[책/뇌를 자극하는 알고리즘] 6-1. 탐색 / 순차 탐색"
author: 임가람
date: "2021-08-30 21:41:00"
categories: [책, 뇌를 자극하는 알고리즘]
tags: [뇌를 자극하는 알고리즘]
---

## 순차 탐색(Sequential Search)
데이터 집합의 처음부터 끝까지 차례대로 모든 요소를 비교해서 데이터를 찾는 탐색 알고리즘<br>
한쪽 방향으로만 탐색을 수행해서 선형 탐색(Linear Search)이라고 부르기도 함<br>
높은 성능이 필요하지 않거나, 데이터 집합의 크기가 작은 곳에서 자주 쓰인다.<br>
<br>

순차 탐색의 구현 예
```c
Node* SLL_SequentialSearch(Node* Head, int Target)
{
    Node* Current = Head;
    Node* Match = NULL;

    while ( Current != NULL )
    {
        /* 찾고자 하는 값을 해당 노드가 갖고 있으면 노드의 주소를 Match에 저장 */
        if ( Current->Data == Target )
        {
            Match = Current;
            break;
        }
        else /* 현재 노드에 찾는 값이 없으면 다음 노드를 조사 */
            Current = Current->NextNode;
    }

    return Match; /* 찾는 값을 갖고 있는 노드늬 주소 반환 */
}
```
<br>

---
## 자기 구성 순차 탐색(Self-Organizing Sequential Search)
자주 사용되는 항목을 데이터 집합의 앞쪽에 배치해서 순차 탐색의 검색 효율을 올리는 방법
<br>

### 전진 이동법(Move To Front)
어느 항목이 한번 탐색되고 나면, 그 항목을 데이터 집합의 가장 앞에 위치시키는 방법
- 데이터 집합
  - 71, 5, 13, 1, 2, 48, 222, 136, 3, 15
- 48 탐색
  - 48, 71, 5, 13, 1, 2, 222, 136, 3, 15
<br>

링크드 리스트에 사용 가능한 전진 이동 순차 탐색법 예제
```c
Node* SLL_MoveToFront(Node* Head, int Target)
{
    Node* Current   = (*Head);
    Node* Previous  = NULL;
    Node* Match     = NULL;

    while ( Current != NULL )
    {
        /* 찾고자 하는 값을 해당 노드가 갖고 있으면 노드의 주소를 Match에 저장 */
        if ( Current->Data == Target )
        {
            Match = Current;
            if ( Previous != NULL )
            {
                /* 자신의 앞 노드와 다음 노드를 연결 */
                Previous->NextNode = Current->NextNode;

                /* 자신을 리스트의 가장 앞으로 옮기기 */
                Current->NextNode = (*Head);
                (*Head) = Current;
            }
            break;
        }
        else
        {
            Previous = Current;
            Current = Current->NextNode;
        }
    }
    return Match;
}
```
<br>

### 전위법(Transpose)
어느 항목이 한번 탐색되고 나면, 그 항목을 자신 바로 앞 데이터와 교환하는 방법
- 데이터 집합
  - 71, 5, 13, 1, 2, 48, 222, 136, 3, 15
- 48 탐색
  - 71, 5, 13, 1, 48, 2, 222, 136, 3, 15
- 다시 48 탐색
  - 71, 5, 13, 48, 1, 2, 222, 136, 3, 15
<br>

링크드 리스트에 사용 가능한 전위법 순차 탐색 예제
```c
Node* SLL_Transpose(Node** Head, int Target)
{
    Node* Current   = (*Head);
    Node* PPrevious = NULL; /* 이전 노드의 이전 노드 */
    Node* Previous  = NULL; /* 이전 노드 */
    Node* Match     = NULL;

    while ( Current != NULL )
    {
        if ( Current->Data == NULL )
        {
            Match = Current;
            if ( Previous != NULL )
            {
                if ( PPrevious != NULL )
                    PPrevious->NextNode = Current;
                else
                    (*Head) = Current;
                
                Previous->NextNode = Current->NextNode;

                Current->NextNode = Previous;
            }
            break;
        }
        else
        {
            if ( Previous != NULL )
                PPrevious = Previous;
                
            Previous = Current;
            Current = Current->NextNode;
        }
    }
    return Match;
}
```
<br>

### 빈도 계수법(Frequency Count)
데이터 집합 내의 각 요소들이 탐색된 횟수를 별도의 공간에 저장해 두고, 탐색된 횟수가 높은 순으로 데이터 집합을 재구성하는 알고리즘

<br>

\# 참고<br>
[박상현 / 뇌를 자극하는 알고리즘](https://www.hanbit.co.kr/media/books/book_view.html?p_code=B3450156021){:target="_blank"}<br>