---
layout: post
title: "[책/뇌를 자극하는 알고리즘] 7. 우선순위 큐와 힙"
author: 임가람
date: "2021-09-05 15:17:00"
categories: [책, 뇌를 자극하는 알고리즘]
tags: [뇌를 자극하는 알고리즘]
---

## 우선순위 큐(Priority Queue)
큐의 FIFO(First-In First-Out)구조를 그대로 씀과 동시에 새 요소에 우선순위를 부여하여 큐에 삽입하고 가장 높은 우선순위를 갖는 요소부터 빠져나오도록 한다.<br>
우선순위는 프로그래머가 정하기 나름이지만 이 글의 예시는 값이 작을수록 우선순위가 높음을 가정한다.<br>

### 우선순위 큐의 삽입 연산
![우선순위큐와힙-삽입연산1](/assets/img/posts/2021-09-05-priority-queue-1.png){: width="700" height="150" }<br>
우선순위 20의 요소를 삽입한다고 했을 때 일반 큐라면 117 뒤에 추가되겠지만,<br>
우선순위 큐는 첫 요소부터 순차적으로 우선순위를 비교하여 17과 22 사이에 삽입한다.<br>
![우선순위큐와힙-삽입연산2](/assets/img/posts/2021-09-05-priority-queue-2.png){: width="700" height="150" }<br>
<br>

### 우선순위 큐의 제거 연산
일반큐와 동일하게 전단을 제거해서 반환한다.<br>
![우선순위큐와힙-제거연산](/assets/img/posts/2021-09-05-priority-queue-3.png){: width="700" height="150" }<br>
<br>

---
## 힙(Heap)
힙 순서 속성(Heap Order Property)을 만족하는 완전 이진 트리<br>
힙 순서 속성
- 트리 내의 모든 노드가 부모 노드보다 커야한다.(루트 노드는 부모 노드가 없으니 예외)
- 힙에서 가장 작은 데이터를 갖는 노드는 루트 노드이다.
<br>

### 힙에 새 노드 삽입하기
- 힙의 최고 깊이, 최 우측에 새 노드를 추가한다.(이때 힙은 완전 이진 트리를 유지해야 한다.)
- 삽입한 노드를 부모 노드와 비교한다. 삽입한 노드가 부모보다 크면 연산을 종료하고, 그렇지 않으면 3번을 진행한다.
- 삽입한 노드가 부모보다 작으면 부모 노드와 삽입한 노드의 위치를 서로 바꾼다. 바꾸고 나면 다시 2번을 진행한다.

예제) 아래의 힙에 새 노드 '7'을 삽입한다.<br>
![우선순위큐와힙-노드삽입1](/assets/img/posts/2021-09-05-priority-queue-4.png){: width="700" height="150" }<br>
새 노드 7을 힙의 가장 깊은 곳에 추가한다.<br>
완전 이진 트리가 무너지면 안되므로 노드 37의 오른쪽 자식으로 추가한다.<br>
그리고 추가한 노드를 부모 노드와 비교한다. 7은 37보다 작으므로 이 두 노드를 교환한다.<br>
![우선순위큐와힙-노드삽입2](/assets/img/posts/2021-09-05-priority-queue-5.png){: width="700" height="150" }<br>
다시 부모 노드와 비교한다. 8과 7을 교환한다.<br>
![우선순위큐와힙-노드삽입3](/assets/img/posts/2021-09-05-priority-queue-6.png){: width="700" height="150" }<br>
다시 부모 노드와 비교를 한다. 7은 루트 노드 2보다 크므로 힙 순서 속성을 위반하지 않아 삽입을 완료한다.<br>
![우선순위큐와힙-노드삽입4](/assets/img/posts/2021-09-05-priority-queue-7.png){: width="700" height="150" }<br>

<br>

### 힙의 최소값 삭제
힙의 최소값을 삭제 = 루트 노드를 삭제<br>
루트 노드를 삭제하면 삭제 후에 힙 순서 속성을 유지하기 위해 아래와 같이 처리해줘야 한다.<br>

루트 노드를 삭제한 후의 뒤 처리 과정
- 힙의 루트에 최고 깊이, 최 우측에 있던 노드를 루트 노드로 옮긴다. 이때 힙의 힙 순서 속성이 파괴된다.
- 힙 순서 속성이 지켜지려면 부모 노드는 양쪽 자식보다 작은 값을 가져야 하기 때문에 옮겨온 노드의 양쪽 자식을 비교하여 작은 쪽 자식과 위치 교환을 한다.
- 옮겨온 자식 노드가 더 이상 자식이 없는 잎 노드가 되거나 양쪽 자식보다 작은 값을 갖는 경우엔 삭제 연산을 종료한다. 그렇지 않으면 2번째 단계를 반복한다.

루트 노드 삭제 예시<br>
![우선순위큐와힙-삭제1](/assets/img/posts/2021-09-05-priority-queue-8.png){: width="700" height="150" }<br>

최고 깊이, 최 우측에 있는 노드 37을 루트 노드로 옮긴다.<br>
루트 노드 37이 왼쪽 자식 노드 7보다 크므로 힙 순서 속성을 위반하기 때문에 7과 37을 교환한다.<br>
![우선순위큐와힙-삭제2](/assets/img/posts/2021-09-05-priority-queue-9.png){: width="700" height="150" }<br>

37이 양쪽 자식 13, 8보다 크므로 더 작은 노드 8과 37 노드를 교환한다.<br>
![우선순위큐와힙-삭제3](/assets/img/posts/2021-09-05-priority-queue-10.png){: width="700" height="150" }<br>

37이 자식 노드 88보다 작으므로 힙 순서 속성을 위반하지 않기 때문에 삭제 연산을 종료한다.<br>
![우선순위큐와힙-삭제4](/assets/img/posts/2021-09-05-priority-queue-11.png){: width="700" height="150" }<br>

<br>

### 힙의 구현
힙은 완전 이진 트리이기 때문에 링크드 리스트 기반으로 구현하는 것도 방법이긴 하지만 힙의 가장 마지막 노드(최고 깊이, 최 우측 노드)를 찾는데 효율적이지 않기 때문에 힙의 구현에는 링크드 리스트보다는 배열이 더 효율적이다.<br>
<br>
완전 이진 트리를 배열로 나타내는 방법
- 깊이 0의 노드는 배열의 0번 요소에 저장한다.
- 깊이 1의 노드(모두 2개)는 배열의 1-2번 요소에 저장한다.
- 깊이 2의 노드(모두 4개)는 배열의 3-6번 요소에 저장한다.
- 깊이 n의 노드(모두 $2^n$ 개)는 배열의 $2^n-1$ ~ $2^{n+1}-2$ 번 요소에 저장한다. 

![우선순위큐와힙-구현](/assets/img/posts/2021-09-05-priority-queue-12.png){: width="700" height="150" }<br>

이렇게 배열을 쓰면 완전 이진 트리인 힙의 각 노드의 위치나 부모/자식 관계 등을 배열의 인덱스만으로 단번에 알아낼 수 있다.<br>
다음은 배열 기반의 완전 이진 트리에서 유용하게 쓸 수 있는 공식이다.
- k번 인덱스에 위치한 노드의 양쪽 자식 노드들이 위치한 인덱스
  - 왼쪽 자식 노드: $2k + 1$
  - 오른쪽 자식 노드: $2k + 2$
- k번 인덱스에 위치한 노드의 부모 노드가 위치한 인덱스: $\dfrac{k-1}{2}$의 몫

<br>

---
## 힙 예제 프로그램

Heap.h
```c
#ifndef HEAP_H
#define HEAP_H

#include <stdio.h>
#include <memory.h>
#include <stdlib.h>

typedef int ElementType;

typedef struct tageHeapNode {
    ElementType Data;
} HeapNode;

typedef struct tagHeap {
    HeapNode* Nodes;
    int Capacity; /* 힙의 용량 = Nodes 배열의 크기 */
    int UsedSize; /* 실제 배열 안에 들어 있는 큐 요소의 수 */
} Heap;

Heap*     HEAP_Create( int InitialSize );
void      HEAP_Destroy( Heap* H );
void      HEAP_Insert(  Heap* H, ElementType NewData );
void      HEAP_DeleteMin( Heap* H, HeapNode* Root );
int       HEAP_GetParent( int Index );
int       HEAP_GetLeftChild( int Index );
void      HEAP_SwapNodes( Heap* H, int Index1, int Index2 );
void      HEAP_PrintNodes(Heap* H );

#endif
```
Heap.c
```c
#include "Heap.h"

Heap* HEAP_Create( int IntitialSize )
{
    Heap* NewHeap = (Heap*) malloc( sizeof( Heap ) );
    NewHeap->Capacity = IntitialSize;
    NewHeap->UsedSize = 0;
    NewHeap->Nodes = (HeapNode*) malloc( sizeof ( HeapNode ) * NewHeap->Capacity );

    printf("size : %d\n", sizeof(HeapNode));

    return NewHeap;
}

void  HEAP_Destroy( Heap* H )
{
    free( H->Nodes );
    free( H );
}

void  HEAP_Insert( Heap* H, ElementType NewData )
{
    int CurrentPosition = H->UsedSize;
    int ParentPosition  = HEAP_GetParent( CurrentPosition );
    /* UsedSize가 Capacity에 도달하면 Capacity를 두 배로 늘린다. */
    if ( H->UsedSize == H->Capacity ) 
    {
        H->Capacity *= 2;
        H->Nodes = (HeapNode*) 
                   realloc( H->Nodes, sizeof( HeapNode ) * H->Capacity );
    }

    H->Nodes[CurrentPosition].Data = NewData;

    /*  후속 처리. */
    while ( CurrentPosition > 0 
        && H->Nodes[CurrentPosition].Data < H->Nodes[ParentPosition].Data )
    {
        HEAP_SwapNodes( H, CurrentPosition, ParentPosition );
        
        CurrentPosition = ParentPosition;
        ParentPosition  = HEAP_GetParent( CurrentPosition );
    }

    H->UsedSize++;
}

void      HEAP_SwapNodes( Heap* H, int Index1, int Index2 )
{ 
    int CopySize = sizeof( HeapNode );
    HeapNode* Temp = (HeapNode*)malloc(CopySize);
        
    memcpy( Temp,               &H->Nodes[Index1], CopySize);
    memcpy( &H->Nodes[Index1] , &H->Nodes[Index2], CopySize);
    memcpy( &H->Nodes[Index2] , Temp,              CopySize);

    free(Temp);
}

void      HEAP_DeleteMin( Heap* H, HeapNode* Root )
{
    int ParentPosition = 0;
    int LeftPosition   = 0;    
    int RightPosition  = 0;    
    /* Root에 최소값을 저장 */
    memcpy(Root, &H->Nodes[0], sizeof(HeapNode));
    memset(&H->Nodes[0], 0, sizeof(HeapNode));

    H->UsedSize--;
    /* 힙의 첫 번째 요소에 마지막 요소의 값을 복사 */
    HEAP_SwapNodes( H, 0, H->UsedSize );

    LeftPosition  = HEAP_GetLeftChild( 0 );
    RightPosition = LeftPosition + 1;

    /* 힙순서 속성을 만족할 때까지 자식 노드와의 교환을 반복 */
    while ( 1 )
    {
        int SelectedChild = 0;

        if ( LeftPosition >= H->UsedSize )
            break;

        if ( RightPosition >= H->UsedSize )
        {
            SelectedChild = LeftPosition;
        }
        else {
            if ( H->Nodes[LeftPosition].Data > H->Nodes[RightPosition].Data)
                SelectedChild = RightPosition;
            else
                SelectedChild = LeftPosition;                
        }

        if ( H->Nodes[SelectedChild].Data < H->Nodes[ParentPosition].Data )
        {
            HEAP_SwapNodes(H, ParentPosition, SelectedChild);
            ParentPosition = SelectedChild;
        }
        else
            break;

        LeftPosition  = HEAP_GetLeftChild(ParentPosition);
        RightPosition = LeftPosition + 1;
    }

    if ( H->UsedSize < ( H->Capacity / 2 ) ) 
    {
        H->Capacity /= 2;
        H->Nodes = 
            (HeapNode*) realloc( H->Nodes, sizeof( HeapNode ) * H->Capacity );
    }
}

int HEAP_GetParent( int Index )
{
    return (int) ((Index - 1) / 2);
}

int HEAP_GetLeftChild( int Index )
{
    return (2 * Index) + 1;
}

void      HEAP_PrintNodes(Heap* H )
{
    int i = 0;
    for ( i=0; i < H->UsedSize; i++ )
    {
        printf("%d ", H->Nodes[i].Data);
    }
    printf("\n");
}
```
Test_Heap.c
```c
#include "Heap.h"

int main( void )
{
    Heap* H = HEAP_Create( 3 );
    HeapNode MinNode;
    
    HEAP_Insert( H, 12 );
    HEAP_Insert( H, 87 );
    HEAP_Insert( H, 111 );
    HEAP_Insert( H, 34 );
    HEAP_Insert( H, 16 );
    HEAP_Insert( H, 75 );
    HEAP_PrintNodes( H );
    
    HEAP_DeleteMin( H, &MinNode );
    HEAP_PrintNodes( H );

    HEAP_DeleteMin( H, &MinNode );
    HEAP_PrintNodes( H );

    HEAP_DeleteMin( H, &MinNode );
    HEAP_PrintNodes( H );

    HEAP_DeleteMin( H, &MinNode );
    HEAP_PrintNodes( H );

    HEAP_DeleteMin( H, &MinNode );
    HEAP_PrintNodes( H );

    HEAP_DeleteMin( H, &MinNode );
    HEAP_PrintNodes( H );

    return 0;
}
```
실행결과
```
size : 4
12 16 75 87 34 111
16 34 75 87 111
34 87 75 111
75 87 111
87 111
111
```
<br>

---
## 힙을 이용한 우선순위 큐의 구현

PriorityQueue.h
```c
#ifndef PRIORITYQUEUE_H
#define PRIORITYQUEUE_H

#include <stdio.h>
#include <memory.h>
#include <stdlib.h>

typedef int       PriorityType;

typedef struct tagePQNode 
{
    PriorityType Priority;
    void*        Data;
} PQNode;

typedef struct tagPriorityQueue 
{
    PQNode* Nodes;
    int Capacity;
    int UsedSize;
} PriorityQueue;

PriorityQueue* PQ_Create( int InitialSize );
void           PQ_Destroy( PriorityQueue* PQ );
void           PQ_Enqueue( PriorityQueue* PQ, PQNode NewData );
void           PQ_Dequeue( PriorityQueue* PQ, PQNode* Root );
int            PQ_GetParent( int Index );
int            PQ_GetLeftChild( int Index );
void           PQ_SwapNodes( PriorityQueue* PQ, int Index1, int Index2 );
int            PQ_IsEmpty( PriorityQueue* PQ );

#endif
```
PriorityQueue.c
```c
#include "PriorityQueue.h"

PriorityQueue* PQ_Create( int IntitialSize )
{
    PriorityQueue* NewPQ = (PriorityQueue*) malloc( sizeof( PriorityQueue ) );
    NewPQ->Capacity = IntitialSize;
    NewPQ->UsedSize = 0;
    NewPQ->Nodes = (PQNode*) malloc( sizeof ( PQNode ) * NewPQ->Capacity );

    return NewPQ;
}

void  PQ_Destroy( PriorityQueue* PQ )
{
    free( PQ->Nodes );
    free( PQ );
}

void  PQ_Enqueue( PriorityQueue* PQ, PQNode NewNode )
{
    int CurrentPosition = PQ->UsedSize;
    int ParentPosition  = PQ_GetParent( CurrentPosition );

    if ( PQ->UsedSize == PQ->Capacity ) 
    {
        if ( PQ->Capacity == 0 )
            PQ->Capacity = 1;
            
        PQ->Capacity *= 2;
        PQ->Nodes = (PQNode*) realloc( PQ->Nodes, sizeof( PQNode ) * PQ->Capacity );
    }

    PQ->Nodes[CurrentPosition] = NewNode;

    /*  후속 처리. */
    while ( CurrentPosition > 0 
        && PQ->Nodes[CurrentPosition].Priority < PQ->Nodes[ParentPosition].Priority )
    {
        PQ_SwapNodes( PQ, CurrentPosition, ParentPosition );
        
        CurrentPosition = ParentPosition;
        ParentPosition  = PQ_GetParent( CurrentPosition );
    }

    PQ->UsedSize++;
}

void      PQ_SwapNodes( PriorityQueue* PQ, int Index1, int Index2 )
{ 
    int CopySize = sizeof( PQNode );
    PQNode* Temp = (PQNode*)malloc(CopySize);
        
    memcpy( Temp,               &PQ->Nodes[Index1], CopySize);
    memcpy( &PQ->Nodes[Index1] , &PQ->Nodes[Index2], CopySize);
    memcpy( &PQ->Nodes[Index2] , Temp,              CopySize);

    free(Temp);
}

void      PQ_Dequeue( PriorityQueue* PQ, PQNode* Root )
{
    int ParentPosition = 0;
    int LeftPosition   = 0;    
    int RightPosition  = 0;    
    
    memcpy(Root, &PQ->Nodes[0], sizeof(PQNode));
    memset(&PQ->Nodes[0], 0, sizeof(PQNode));

    PQ->UsedSize--;
    PQ_SwapNodes( PQ, 0, PQ->UsedSize );

    LeftPosition  = PQ_GetLeftChild( 0 );
    RightPosition = LeftPosition + 1;

    while ( 1 )
    {
        int SelectedChild = 0;

        if ( LeftPosition >= PQ->UsedSize )
            break;

        if ( RightPosition >= PQ->UsedSize )
        {
            SelectedChild = LeftPosition;
        }
        else {
            if ( PQ->Nodes[LeftPosition].Priority > PQ->Nodes[RightPosition].Priority)
                SelectedChild = RightPosition;
            else
                SelectedChild = LeftPosition;                
        }

        if ( PQ->Nodes[SelectedChild].Priority < PQ->Nodes[ParentPosition].Priority)
        {
            PQ_SwapNodes(PQ, ParentPosition, SelectedChild);
            ParentPosition = SelectedChild;
        }
        else
            break;

        LeftPosition  = PQ_GetLeftChild(ParentPosition);
        RightPosition = LeftPosition + 1;
    }

    if ( PQ->UsedSize < ( PQ->Capacity / 2 ) ) 
    {
        PQ->Capacity /= 2;
        PQ->Nodes = 
            (PQNode*) realloc( PQ->Nodes, sizeof( PQNode ) * PQ->Capacity );
    }
}

int PQ_GetParent( int Index )
{
    return (int) ((Index - 1) / 2);
}

int PQ_GetLeftChild( int Index )
{
    return (2 * Index) + 1;
}

int PQ_IsEmpty( PriorityQueue* PQ )
{
    return ( PQ->UsedSize == 0 );
}
```
Tes_tPriorityQueue.c
```c
#include "PriorityQueue.h"

void PrintNode( PQNode* Node )
{
    printf("작업명 : %s (우선순위:%d)\n", Node->Data, Node->Priority);
}

int main( void )
{
    PriorityQueue* PQ = PQ_Create(  3 );
    PQNode Popped;

    PQNode Nodes[7] = 
    {
        {34, (void*)"코딩"},
        {12, (void*)"고객미팅"},
        {87, (void*)"커피타기"},        
        {45, (void*)"문서작성"},
        {35, (void*)"디버깅"},
        {66, (void*)"이닦기"}
    };
    
    PQ_Enqueue(PQ, Nodes[0] );
    PQ_Enqueue(PQ, Nodes[1] );
    PQ_Enqueue(PQ, Nodes[2] );
    PQ_Enqueue(PQ, Nodes[3] );
    PQ_Enqueue(PQ, Nodes[4] );
    PQ_Enqueue(PQ, Nodes[5]);

    printf( "큐에 남아 있는 작업의 수 : %d\n", PQ->UsedSize );
    
    while ( !PQ_IsEmpty ( PQ ) )
    {
        PQ_Dequeue( PQ, &Popped );
        PrintNode( &Popped );
    }
    
    return 0;
}
```
실행결과
```
큐에 남아 있는 작업의 수 : 6
작업명 : 고객미팅 (우선순위:12)
작업명 : 코딩 (우선순위:34)
작업명 : 디버깅 (우선순위:35)
작업명 : 문서작성 (우선순위:45)
작업명 : 이닦기 (우선순위:66)
작업명 : 커피타기 (우선순위:87)
```

<br>

\# 참고<br>
[박상현 / 뇌를 자극하는 알고리즘](https://www.hanbit.co.kr/media/books/book_view.html?p_code=B3450156021){:target="_blank"}<br>