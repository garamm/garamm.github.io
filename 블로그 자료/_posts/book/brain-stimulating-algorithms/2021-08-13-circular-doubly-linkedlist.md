---
layout: post
title: "[책/뇌를 자극하는 알고리즘] 1-3. 리스트 / 환형 링크드 리스트"
author: 임가람
date: "2021-08-13 22:24:00"
categories: [책, 뇌를 자극하는 알고리즘]
tags: [뇌를 자극하는 알고리즘]
---

## 환형 링크드 리스트란?
- 헤드가 테일과 연결된 형태의 링크드 리스트

![환형 링크드 리스트 node의 구조](/assets/img/posts/2021-08-13-circular-doubly-linkedlist-1.png){: width="600" height="150" }

<br>

---
<br>

## 주요 연산
- 테일은 헤드의 '앞 노드'이다.
- 헤드는 테일의 '뒷 노드'이다.

### 노드 추가
리스트가 비어있다면 새로운 노드는 헤드가 되고, 헤드의 앞 노드는 헤드 자신이 된다.<br>
리스트가 비어있지 않은 경우엔 테일과 헤드 사이에 새 노드를 삽입한다.

![환형링크드리스트-노드추가](/assets/img/posts/2021-08-13-circular-doubly-linkedlist-2.png){: width="600" height="150" }

<br>

### 노드 삭제
테일과 헤드가 연결되어 있다는 점 외에 더블 링크드 리스트와 크게 달라진 점은 없다.

<br>

---
<br>

## 예제 코드

CircularDoublyLinkedList.h
```c
#ifndef CIRCULAR_DOUBLY_LINKEDLIST_H
#define CIRCULAR_DOUBLY_LINKEDLIST_H

#include <stdio.h>
#include <stdlib.h>

typedef int ElementType;

typedef struct tagNode
{
    ElementType Data;
    struct tagNode* PrevNode;
    struct tagNode* NextNode;
} Node;

Node* CDLL_CreateNode(ElementType NewData);
void  CDLL_DestroyNode(Node* Node);
void  CDLL_AppendNode(Node** Head, Node* NewNode);
void  CDLL_InsertAfter(Node* Current, Node* NewNode);
void  CDLL_RemoveNode(Node** Head, Node* Remove);
Node* CDLL_GetNodeAt(Node* Head, int Location);
int   CDLL_GetNodeCount(Node* Head);

#endif CIRCULAR_DOUBLY_LINKEDLIST_H
```
CircularDoublyLinkedList.c
```c
#include "CircularDoublyLinkedList.h"

/*  노드 생성 */
Node* CDLL_CreateNode(ElementType NewData)
{
    Node* NewNode = (Node*)malloc(sizeof(Node));

    NewNode->Data = NewData;
    NewNode->PrevNode = NULL;
    NewNode->NextNode = NULL;

    return NewNode;
}

/*  노드 소멸 */
void CDLL_DestroyNode(Node* Node)
{
    free(Node);
}

/*  노드 추가 */
void CDLL_AppendNode(Node** Head, Node* NewNode)
{
    /*  헤드 노드가 NULL이라면 새로운 노드가 Head */
    if ( (*Head) == NULL ) 
    {
        *Head = NewNode;
        (*Head)->NextNode = *Head;
        (*Head)->PrevNode = *Head;
    } 
    else
    {
        /*  테일과 헤드 사이에 NewNode를 삽입한다. */
        Node* Tail = (*Head)->PrevNode;
        
        Tail->NextNode->PrevNode = NewNode;
        Tail->NextNode = NewNode;

        NewNode->NextNode = (*Head);
        NewNode->PrevNode = Tail; /*  기존의 테일을 새로운  */
                                  /*  테일의 PrevNode가 가리킨다. */
    }
}

/*  노드 삽입 */
void CDLL_InsertAfter(Node* Current, Node* NewNode)
{
    NewNode->NextNode = Current->NextNode;
    NewNode->PrevNode = Current;

    if ( Current->NextNode != NULL )
    {
        Current->NextNode->PrevNode = NewNode;
        Current->NextNode = NewNode;
    }
}

/*  노드 제거 */
void CDLL_RemoveNode(Node** Head, Node* Remove)
{
    if ( *Head == Remove )
    {
        (*Head)->PrevNode->NextNode = Remove->NextNode;
        (*Head)->NextNode->PrevNode = Remove->PrevNode;

        *Head = Remove->NextNode;
        
        Remove->PrevNode = NULL;
        Remove->NextNode = NULL;
    }
    else
    {
/*
        Node* Temp = Remove;

        Remove->PrevNode->NextNode = Temp->NextNode;
        Remove->NextNode->PrevNode = Temp->PrevNode;
*/
        Remove->PrevNode->NextNode = Remove->NextNode;
        Remove->NextNode->PrevNode = Remove->PrevNode;

        Remove->PrevNode = NULL;
        Remove->NextNode = NULL;
    }    
}

/*  노드 탐색 */
Node* CDLL_GetNodeAt(Node* Head, int Location)
{
    Node* Current = Head;

    while ( Current != NULL && (--Location) >= 0)
    {
        Current = Current->NextNode;
    }

    return Current;
}

/*  노드 수 세기 */
int CDLL_GetNodeCount(Node* Head)
{
    unsigned int  Count = 0;
    Node*         Current = Head;

    while ( Current != NULL )
    {
        Current = Current->NextNode;
        Count++;

        if ( Current == Head )
            break;
    }

    return Count;
}

void PrintNode(Node* _Node)
{
    if ( _Node->PrevNode == NULL )
        printf("Prev: NULL");
    else
        printf("Prev: %d", _Node->PrevNode->Data);

    printf(" Current: %d ", _Node->Data);

    if ( _Node->NextNode == NULL )
        printf("Next: NULL\n");
    else
        printf("Next: %d\n", _Node->NextNode->Data);
}
```
Test_CircularDoublyLinkedList.c
```c
#include "CircularDoublyLinkedList.h"

int main( void )
{
    int   i       = 0;
    int   Count   = 0;
    Node* List    = NULL;
    Node* NewNode = NULL;
    Node* Current = NULL;

    /*  노드 5개 추가 */
    for ( i = 0; i<5; i++ )
    {
        NewNode = CDLL_CreateNode( i );
        CDLL_AppendNode( &List,NewNode );
    }

    /*  리스트 출력 */
    Count = CDLL_GetNodeCount( List );
    for ( i = 0; i<Count; i++ )
    {
        Current = CDLL_GetNodeAt( List, i );
        printf( "List[%d] : %d\n", i, Current->Data );
    }

    /*  리스트의 세번째 칸 뒤에 노드 삽입 */
    printf( "\nInserting 3000 After [2]...\n\n" );

    Current = CDLL_GetNodeAt( List, 2 );
    NewNode = CDLL_CreateNode( 3000 );
    CDLL_InsertAfter( Current, NewNode );

    printf( "\nRemoving Node at 2...\n" );
    Current = CDLL_GetNodeAt( List, 2 );
    CDLL_RemoveNode( &List, Current );
    CDLL_DestroyNode( Current );

    /*  리스트 출력  */
    /*  (노드 수의 2배만큼 루프를 돌며 환형임을 확인한다.) */
    Count = CDLL_GetNodeCount( List );
    for ( i = 0; i<Count*2; i++ )
    {
        if ( i == 0 )
            Current = List;
        else
            Current = Current->NextNode;
        
        printf( "List[%d] : %d\n", i, Current->Data );
    }

    /*  모든 노드를 메모리에서 제거     */
    printf( "\nDestroying List...\n" );

    Count = CDLL_GetNodeCount( List );

    for ( i = 0; i<Count; i++ )
    {
        Current = CDLL_GetNodeAt( List, 0 );

        if ( Current != NULL ) 
        {
            CDLL_RemoveNode( &List, Current );
            CDLL_DestroyNode( Current );
        }
    }

    return 0;
}
```
실행 결과
```
List[0] : 0
List[1] : 1
List[2] : 2
List[3] : 3
List[4] : 4

Inserting 3000 After [2]...

List[0] : 0
List[1] : 1
List[2] : 2
List[3] : 3000
List[4] : 3
List[5] : 4
List[6] : 0
List[7] : 1
List[8] : 2
List[9] : 3000
List[10] : 3
List[11] : 4
```

<br>

\# 참고<br>
[박상현 / 뇌를 자극하는 알고리즘](https://www.hanbit.co.kr/media/books/book_view.html?p_code=B3450156021){:target="_blank"}<br>