---
layout: post
title: "[책/뇌를 자극하는 알고리즘] 1-2. 리스트 / 더블 링크드 리스트"
author: 임가람
date: "2021-08-12 23:41:00"
categories: [책, 뇌를 자극하는 알고리즘]
tags: [뇌를 자극하는 알고리즘]
---

## 더블 링크드 리스트란?
- 링크드 리스트의 탐색 기능을 개선한 자료구조
- 양방향으로 탐색 가능
- 링크드 리스트의 노드는 다음 노드를 가리키는 포인터만 가지고 있었는데, 더블 링크드 리스트의 노드는 이전 노드를 가리키는 포인터도 갖고 있음

![더블 링크드 리스트 node의 구조](/assets/img/posts/2021-08-12-doubly-linkedlist-1.png){: width="600" height="150" }

<br>

---
<br>

## 주요 연산
### 노드 생성/소멸
링크드 리스트에서 이전 노드인 PrevNode에 NULL을 대입하여 초기화 하는 부분만 추가됨<br>
소멸하는 부분은 링크드 리스트와 동일

<br>

### 노드 추가
링크드 리스트에서 노드 추가하는 방식에 새로운 테일의 PrevNode 포인터가 기존 테일의 주소를 가리키도록 추가해야 함<br>

![더블링크드리스트-노드추가](/assets/img/posts/2021-08-12-doubly-linkedlist-2.png){: width="600" height="150" }

<br>

### 노드 탐색

링크드 리스트와 동일하다

<br>

### 노드 삭제
삭제할 노드의 NextNode 포인터가 가리키고 있던 노드를 앞 노드의 NextNode 포인터가 가리키게 바꾸고, 삭제할 노드의 PrevNode 포인터가 가리키고 있던 노드를 뒷 노드의 PrevNode 포인터가 가리키게 바꾼다.<br>
그리고 삭제할 노드의 NextNode와 PrevNode는 NULL로 초기화한다.

![더블링크드리스트-노드삭제](/assets/img/posts/2021-08-12-doubly-linkedlist-3.png){: width="600" height="150" }

<br>

### 노드 삽입
새로운 노드는 PrevNode 포인터로는 앞 노드를, NextNode 포인터로는 뒷 노드를 가리킨다.<br>
그리고 앞 노드의 NextNode 포인터와 뒷 노드의 PrevNode 포인터는 새 노드를 가리킨다.

![더블링크드리스트-노드삽입](/assets/img/posts/2021-08-12-doubly-linkedlist-4.png){: width="600" height="150" }

<br>

---
<br>

## 예제 코드

DoublyLinkedList.h
```c
#ifndef DOUBLY_LINKEDLIST_H
#define DOUBLY_LINKEDLIST_H

#include <stdio.h>
#include <stdlib.h>

typedef int ElementType;

typedef struct tagNode
{
    ElementType Data;
    struct tagNode* PrevNode;
    struct tagNode* NextNode;
} Node;

/* 함수 원형 선언 */
Node* DLL_CreateNode( ElementType NewData );
void  DLL_DestroyNode( Node* Node);
void  DLL_AppendNode( Node** Head, Node* NewNode );
void  DLL_InsertAfter( Node* Current, Node* NewNode );
void  DLL_RemoveNode( Node** Head, Node* Remove );
Node* DLL_GetNodeAt( Node* Head, int Location );
int   DLL_GetNodeCount( Node* Head );

#endif DOUBLY_LINKEDLIST_H
```
DoublyLinkedList.c
```c
#include "DoublyLinkedList.h"

/*  노드 생성 */
Node* DLL_CreateNode( ElementType NewData )
{
    Node* NewNode = (Node*)malloc(sizeof(Node));

    NewNode->Data = NewData;
    NewNode->PrevNode = NULL;
    NewNode->NextNode = NULL;

    return NewNode;
}

/*  노드 소멸 */
void DLL_DestroyNode( Node* Node )
{
    free(Node);
}

/*  노드 추가 */
void DLL_AppendNode( Node** Head, Node* NewNode )
{
    /*  헤드 노드가 NULL이라면 새로운 노드가 Head */
    if ( (*Head) == NULL ) 
    {
        *Head = NewNode;
    } 
    else
    {
        /*  테일을 찾아 NewNode를 연결한다. */
        Node* Tail = (*Head);
        while ( Tail->NextNode != NULL )
        {
            Tail = Tail->NextNode;
        }

        Tail->NextNode = NewNode;
        NewNode->PrevNode = Tail; /*  기존의 테일을 새로운 테일의 PrevNode가 가리킨다. */
    }
}

/*  노드 삽입 */
void DLL_InsertAfter( Node* Current, Node* NewNode )
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
void DLL_RemoveNode( Node** Head, Node* Remove )
{
    if ( *Head == Remove )
    {
        *Head = Remove->NextNode;
        if ( (*Head) != NULL )
            (*Head)->PrevNode = NULL;
        
        Remove->PrevNode = NULL;
        Remove->NextNode = NULL;
    }
    else
    {
        Node* Temp = Remove;

        if ( Remove->PrevNode != NULL )
            Remove->PrevNode->NextNode = Temp->NextNode;

        if ( Remove->NextNode != NULL )
            Remove->NextNode->PrevNode = Temp->PrevNode;

        Remove->PrevNode = NULL;
        Remove->NextNode = NULL;
    }    
}

/*  노드 탐색 */
Node* DLL_GetNodeAt( Node* Head, int Location )
{
    Node* Current = Head;

    while ( Current != NULL && (--Location) >= 0)
    {
        Current = Current->NextNode;
    }

    return Current;
}

/*  노드 수 세기 */
int DLL_GetNodeCount( Node* Head )
{
    unsigned int  Count = 0;
    Node*         Current = Head;

    while ( Current != NULL )
    {
        Current = Current->NextNode;
        Count++;
    }

    return Count;
}

void PrintNode( Node* _Node )
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
Test_DoublyLinkedList.c
```c
#include "DoublyLinkedList.h"

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
        NewNode = DLL_CreateNode( i );
        DLL_AppendNode( &List, NewNode );
    }

    /*  리스트 출력 */
    Count = DLL_GetNodeCount( List );
    for ( i = 0; i<Count; i++ )
    {
        Current = DLL_GetNodeAt( List, i );
        printf( "List[%d] : %d\n", i, Current->Data );
    }

    /*  리스트의 세번째 칸 뒤에 노드 삽입 */
    printf( "\nInserting 3000 After [2]...\n\n" );

    Current = DLL_GetNodeAt( List, 2 );
    NewNode = DLL_CreateNode( 3000 );
    DLL_InsertAfter( Current, NewNode );

    /*  리스트 출력 */
    Count = DLL_GetNodeCount( List );
    for ( i = 0; i<Count; i++ )
    {
        Current = DLL_GetNodeAt( List, i );
        printf( "List[%d] : %d\n", i, Current->Data );
    }

    /*  모든 노드를 메모리에서 제거     */
    printf( "\nDestroying List...\n" );

    Count = DLL_GetNodeCount(List);

    for ( i = 0; i<Count; i++ )
    {
        Current = DLL_GetNodeAt( List, 0 );

        if ( Current != NULL ) 
        {
            DLL_RemoveNode( &List, Current );
            DLL_DestroyNode( Current );
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

Destroying List...
```

<br>

\# 참고<br>
[박상현 / 뇌를 자극하는 알고리즘](https://www.hanbit.co.kr/media/books/book_view.html?p_code=B3450156021){:target="_blank"}<br>