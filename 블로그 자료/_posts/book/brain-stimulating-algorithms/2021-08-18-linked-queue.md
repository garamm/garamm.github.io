---
layout: post
title: "[책/뇌를 자극하는 알고리즘] 3-2. 큐 / 링크드 큐"
author: 임가람
date: "2021-08-18 21:53:00"
categories: [책, 뇌를 자극하는 알고리즘]
tags: [뇌를 자극하는 알고리즘]
---

## 순환 큐 VS 링크드 큐
링크드 큐의 각 노드는 앞 노드에 대한 포인터를 이용해 구성되어 있기 때문에, 삽입은 새 노드의 포인터에 후단을 연결하고 제거는 전단 바로 이후의 노드에서 전단에 대한 포인터를 없애는것으로 구현한다. 그리고 용량의 제한이 없어서 큐가 가득 차 있는 상태인지 확인할 필요가 없다.
![링크드큐-제거](/assets/img/posts/2021-08-18-linked-queue-1.png){: width="700" height="150" }
<br>

---
## 링크드 큐 구현하기
### 링크드 큐와 노드의 선언
링크드 큐의 구조체: Front(전단), Rear(후단), Count(노드의 수)

<br>

### 링크드 큐의 생성과 소멸
생성: LinkedQueue 구조체를 자유 저장소에 생성 및 구조체의 각 필드 초기화<br>
소멸: 큐 내부에 있는 모든 노드를 자유 저장소에서 제거하고, 큐를 자유 저장소에서 제거
<br>

### 삽입(Enqueue) 연산
후단에 새로운 노드를 붙인다.
<br>

### 제거(Dequeue) 연산
전단의 바로 다음 노드를 새로운 전단으로 만들고, 전단이었던 노드의 포인터를 호출자에게 반환
<br>

---
## 소스코드
LinkedQueue.h
```c
#ifndef LINKED_QUEUE_H
#define LINKED_QUEUE_H

#include <stdio.h>
#include <string.h>
#include <stdlib.h>

typedef struct tagNode
{
    char* Data;
    struct tagNode* NextNode;
} Node;

typedef struct tagLinkedQueue
{
    Node* Front; /* 전단 */
    Node* Rear; /* 후단 */
    int   Count; /* 노드의 수 */
} LinkedQueue;

void  LQ_CreateQueue( LinkedQueue** Queue );
void  LQ_DestroyQueue( LinkedQueue* Queue );

Node* LQ_CreateNode(char* Data);
void  LQ_DestroyNode(Node* _Node );

void  LQ_Enqueue( LinkedQueue* Queue, Node* NewNode );
Node* LQ_Dequeue( LinkedQueue* Queue );

int   LQ_IsEmpty( LinkedQueue* Queue );

#endif LINKED_QUEUE_H
```
LinkedQueue.c
```c
#include "LinkedQueue.h"

void  LQ_CreateQueue( LinkedQueue** Queue )
{
    /*  큐를 자유저장소에 생성 */
    (*Queue )        = ( LinkedQueue*)malloc(sizeof( LinkedQueue ) );
    (*Queue )->Front = NULL;
    (*Queue )->Rear  = NULL;
    (*Queue )->Count = 0;
}

void LQ_DestroyQueue( LinkedQueue* Queue )
{
    /* 모든 노드를 자유 저장소에서 제거 */
    while ( !LQ_IsEmpty( Queue ) )
    {
        Node* Popped = LQ_Dequeue( Queue );
        LQ_DestroyNode(Popped);    
    }

    /*  큐를 자유 저장소에서 해제 */
    free( Queue );
}

Node* LQ_CreateNode( char* NewData )
{
    Node* NewNode = (Node*)malloc( sizeof( Node ) );
    NewNode->Data = (char*)malloc( strlen( NewData) + 1);

    strcpy(NewNode->Data, NewData);  /*  데이터를 저장한다. */

    NewNode->NextNode = NULL; /*  다음 노드에 대한 포인터는 NULL로 초기화 한다. */

    return NewNode;/*  노드의 주소를 반환한다. */
}

void  LQ_DestroyNode(Node* _Node )
{
    free(_Node->Data);
    free(_Node );
}

void LQ_Enqueue( LinkedQueue* Queue, Node* NewNode )
{
    if ( Queue->Front == NULL ) 
    {        
        Queue->Front = NewNode;
        Queue->Rear  = NewNode;
        Queue->Count++;
    } 
    else
    {
        Queue->Rear->NextNode = NewNode;
        Queue->Rear = NewNode;
        Queue->Count++;
    }
}

Node* LQ_Dequeue( LinkedQueue* Queue )
{
    /*  LQ_Dequeue() 함수가 반환할 최상위 노드 */
    Node* Front = Queue->Front;

    if ( Queue->Front->NextNode == NULL )
    {
        Queue->Front = NULL;
        Queue->Rear  = NULL;
    }
    else
    {
        Queue->Front = Queue->Front->NextNode;
    }

    Queue->Count--;

    return Front;
}

int LQ_IsEmpty( LinkedQueue* Queue )
{
    return ( Queue->Front == NULL);
}
```
Test_LinkedQueue.c
```c
#include "LinkedQueue.h"

int main( void )
{
    Node* Popped;
    LinkedQueue* Queue;

    LQ_CreateQueue(&Queue );
    
    LQ_Enqueue( Queue, LQ_CreateNode("abc") );
    LQ_Enqueue( Queue, LQ_CreateNode("def") );
    LQ_Enqueue( Queue, LQ_CreateNode("efg") );
    LQ_Enqueue( Queue, LQ_CreateNode("hij") );

    printf("Queue Size : %d\n", Queue->Count);

    while ( LQ_IsEmpty( Queue ) == 0 )
    {
        Popped = LQ_Dequeue( Queue );

        printf( "Dequeue: %s \n", Popped->Data );

        LQ_DestroyNode( Popped );
    }

    LQ_DestroyQueue( Queue );

    return 0;
}
```
실행결과
```
Dequeue: abc
Dequeue: def
Dequeue: efg
Dequeue: hij
```

<br>

\# 참고<br>
[박상현 / 뇌를 자극하는 알고리즘](https://www.hanbit.co.kr/media/books/book_view.html?p_code=B3450156021){:target="_blank"}<br>