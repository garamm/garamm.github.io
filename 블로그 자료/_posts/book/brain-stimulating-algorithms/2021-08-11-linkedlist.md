---
layout: post
title: "[책/뇌를 자극하는 알고리즘] 1-1. 리스트 / 링크드 리스트"
author: 임가람
date: "2021-08-11 22:17:00"
categories: [책, 뇌를 자극하는 알고리즘]
tags: [뇌를 자극하는 알고리즘]
---

## 링크드 리스트란?
- 노드(Node)를 연결해서 만드는 리스트<br>노드: 데이터를 보관하는 필드와, 다음 노드와의 연결 고리 역할을 하는 포인터로 이루어짐
- 리스트의 첫 번째 노드를 헤드라 하고 마지막 노드를 테일이라고 함
- 데이터가 늘어날 때마다 노드를 만들어 테일에 붙임

![링크드 리스트 node의 구조](/assets/img/posts/2021-08-11-linkedlist-1.png){: width="600" height="150" }

<br>

---
<br>

## 주요 연산

### 노드 생성/소멸
C 언어의 세 가지 메모리 영역
- 정적 메모리(Static Memory): 전역 번수나 정적 변수 등이 저장됨, 프로그램이 종료될 때 해제됨
- 자동 메모리(Automatic Memory): 지역 변수 저장, 스택 구조로 이루어져 있어 코드 블록이 종료될 때 해제됨
- 자유 저장소(Free Store): 프로그래머가 직접 메모리를 관리하는 영역
<br>
자동 메모리에 노드를 생성시 함수가 종료되면 자동 메모리에서 제거되므로 자유 저장소에 노드를 생성해야 한다.<br>
그러므로 malloc()를 사용하여 자유 저장소에 메모리를 할당한다.<br>
<br>
노드를 소멸시키려면 자유 저장소의 free()를 사용한다.

<br>

### 노드 추가
링크드 리스트의 테일 노드 뒤에 새로운 노드를 만들어 연결한다.<br>
자유 저장소에 노드를 생성한 다음, 새로 생선한 노드의 주소를 테일의 NextNode 포인터에 대입한다.<br>
<br>
참고) SLL_AppendNode(Node** _Head, Node* _NewNode)의 매개 변수가 Node**인 이유
```c
// 매개 변수 Head가 Node*로 선언되었다고 가정하고 노드 생성하여 리스트에 추가
Node* List = NULL;
Node* NewNode = NULL;
NewNode = SLL_CreateNode(117);
SLL_AppendNode(List, NewNode);
```
우선 List와 NewNode를 선언하면 자동 메모리에 두 포인터 변수가 생성되고, 이 포인터들은 아무 메모리도 가리키지 않는 상태로 초기화되어있다.

![링크드리스트-노드추가-1](/assets/img/posts/2021-08-11-linkedlist-2.png){: width="600" height="150" }

그 다음 SLL_CreateNode를 실행하면서 자유 저장소에 117을 가진 노드가 생성되고, NewNode가 그 주소를 가리킨다.

![링크드리스트-노드추가-2](/assets/img/posts/2021-08-11-linkedlist-3.png){: width="600" height="150" }

그 후 SLL_AppendNode()를 호출할 때, _Head와 _NewNode가 자동 메모리에 생성되고, List는 _Head에, NewNode는 _NewNode에 자신이 가리키고 있는 메모리의 주소를 복사한다.

![링크드리스트-노드추가-3](/assets/img/posts/2021-08-11-linkedlist-4.png){: width="600" height="150" }

SLL_AppendNode()는 List의 헤드가 NULL일 때, 새 노드를 헤드로 만드는데, 이 때 포인터는 메모리 주소를 담는 변수이기 때문에 SLL_AppendNode()를 호출할 때 List 포인터가 담고 있는 '주소 값'만 _Head에 복사되고, 정작 List 포인터 변수의 주소 자체는 전달되지 않는다.<br>
SLL_AppendNode() 호출 후에는 _Head와 _NewNode는 자동 메모리에 의해 제거되고, List는 NULL인 채로 남게된다.

![링크드리스트-노드추가-4](/assets/img/posts/2021-08-11-linkedlist-5.png){: width="600" height="150" }

그렇기 때문에 포인터(*)가 아닌 포인터에 대한 포인터(**)를 함수에 전달해서 포인터가 가진 값이 아닌 포인터 그 자신의 주소를 넘겨줘야 한다.<br>
**로 선언하면 Head 포인터 자신의 주소를 넘겨서 _Head 포인터는 List 포인터 변수의 주소를 가리키고, 이 주소를 이용하여 List가 NewNode의 주소(자유 저장소에 할당된 117 노드의 주소)를 가리킨다.

![링크드리스트-노드추가-5](/assets/img/posts/2021-08-11-linkedlist-6.png){: width="600" height="150" }

<br>

### 노드 탐색

탐색 연산은 링크드 리스트가 갖고 있는 약점 중의 하나이다.<br>
배열은 인덱스를 이용하여 원하는 요소를 즉시 얻을 수 있지만, 링크드 리스트는 헤드부터 시작해서 다음 노드에 대한 포인터를 보며 차근차근 수를 세어야만 원하는 요소에 접근할 수 있다.

<br>

### 노드 삭제

![링크드리스트-노드삭제](/assets/img/posts/2021-08-11-linkedlist-7.png){: width="600" height="150" }

삭제한 노드를 쓸 곳이 없다면 삭제하는것이 좋다.

<br>

### 노드 삽입

![링크드리스트-노드삽입](/assets/img/posts/2021-08-11-linkedlist-8.png){: width="600" height="150" }

<br>

---
<br>

## 링크드 리스트의 장단점
### 장점
- 새로운 노드의 추가/삽입/삭제가 배열 보다 쉽고 빠름
- 현재 노드의 다음 노드를 얻어오는 연산에 대해 비용이 발생하지 않음

### 단점
- 다음 노드를 가리키려는 포인터 때문에 각 노드마다 4byte의 추가 메모리 필요
- 특정 위치에 있는 노드를 얻는데 드는 비용이 크며 속도도 느림

<br>

---
<br>

## 예제 코드

LinkedList.h
```c
#ifndef LINKEDLIST_H
#define LINKEDLIST_H

#include <stdio.h>
#include <stdlib.h>

typedef int ElementType;

/* 노드 구조체 */
typedef struct tagNode 
{
    ElementType Data; /* 데이터 필드 */
    struct tagNode* NextNode; /* 다음 노드를 가리키는 포인터 */
} Node;

/* 함수 원형 선언 */
Node* SLL_CreateNode(ElementType NewData);
void  SLL_DestroyNode(Node* Node);
void  SLL_AppendNode(Node** Head, Node* NewNode);
void  SLL_InsertAfter(Node* Current, Node* NewNode);
void  SLL_InsertNewHead(Node** Head, Node* NewHead);
void  SLL_RemoveNode(Node** Head, Node* Remove);
Node* SLL_GetNodeAt(Node* Head, int Location);
int   SLL_GetNodeCount(Node* Head);

#endif
```
LinkedList.c
```c
#include "LinkedList.h"

/*  노드 생성 */
Node* SLL_CreateNode(ElementType NewData)
{
    Node* NewNode = (Node*)malloc(sizeof(Node));
    // 자동 메모리에 노드를 생성시 함수가 종료되면 자동 메모리에서 제거되므로 malloc()을 사용하여 자유 저장소에 노드를 생성

    NewNode->Data = NewData;  /*  데이터를 저장한다. */
    NewNode->NextNode = NULL; /*  다음 노드에 대한 포인터는 NULL로 초기화 한다. */

    return NewNode;/*  노드의 주소를 반환한다. */
}

/*  노드 소멸 */
void SLL_DestroyNode(Node* Node)
{
    free(Node);
}

/*  노드 추가 */
void SLL_AppendNode(Node** Head, Node* NewNode)
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
    }
}

/*  노드 삽입 */
void SLL_InsertAfter(Node* Current, Node* NewNode)
{
    NewNode->NextNode = Current->NextNode;
    Current->NextNode = NewNode;
}

void  SLL_InsertNewHead(Node** Head, Node* NewHead)
{
    if ( Head == NULL )
    {
        (*Head) = NewHead;    
    }
    else
    {
        NewHead->NextNode = (*Head);
        (*Head) = NewHead;
    }
}

/*  노드 제거 */
void SLL_RemoveNode(Node** Head, Node* Remove)
{
    if ( *Head == Remove )
    {
        *Head = Remove->NextNode;
    }
    else
    {
        Node* Current = *Head;
        while ( Current != NULL && Current->NextNode != Remove )
        {
            Current = Current->NextNode;
        }

        if ( Current != NULL )
            Current->NextNode = Remove->NextNode;
    }
}

/*  노드 탐색 */
Node* SLL_GetNodeAt(Node* Head, int Location)
{
    Node* Current = Head;

    while ( Current != NULL && (--Location) >= 0)
    {
        Current = Current->NextNode;
    }

    return Current;
}

/*  노드 수 세기 */
int SLL_GetNodeCount(Node* Head)
{
    int   Count = 0;
    Node* Current = Head;

    while ( Current != NULL )
    {
        Current = Current->NextNode;
        Count++;
    }

    return Count;
}
```
Test_LinkedList.c
```c
#include "LinkedList.h"

int main( void )
{
    int   i       = 0;
    int   Count   = 0;
    Node* List    = NULL;
    Node* Current = NULL;
    Node* NewNode = NULL;

    /*  노드 5개 추가 */
    for ( i = 0; i<5; i++ )
    {
        NewNode = SLL_CreateNode(i); /* 자유 저장소에 노드 생성 */
        SLL_AppendNode(&List, NewNode); /* 생성한 노드를 List에 추가 */
    }

    NewNode = SLL_CreateNode(-1);
    SLL_InsertNewHead(&List, NewNode);

    NewNode = SLL_CreateNode(-2);
    SLL_InsertNewHead(&List, NewNode);

    /*  리스트 출력 */
    Count = SLL_GetNodeCount(List);
    for ( i = 0; i<Count; i++ )
    {
        Current = SLL_GetNodeAt(List, i);
        printf("List[%d] : %d\n", i, Current->Data);
    }

    /*  리스트의 세번째 노드 뒤에 새 노드 삽입 */
    printf("\nInserting 3000 After [2]...\n\n");

    Current = SLL_GetNodeAt(List, 2);
    NewNode  = SLL_CreateNode(3000);

    SLL_InsertAfter(Current, NewNode);

    /*  리스트 출력 */
    Count = SLL_GetNodeCount(List);
    for ( i = 0; i<Count; i++ )
    {
        Current = SLL_GetNodeAt(List, i);
        printf("List[%d] : %d\n", i, Current->Data);
    }

    /*  모든 노드를 메모리에서 제거     */
    printf("\nDestroying List...\n");

    for ( i = 0; i<Count; i++ )
    {
        Current = SLL_GetNodeAt(List, 0);

        if ( Current != NULL ) 
        {
            SLL_RemoveNode(&List, Current); /* 리스트에서 선택된 노드 제거 */
            SLL_DestroyNode(Current); /* 제거된 노드를 메모리에서 완전히 소멸 */
        }
    }

    return 0;
}
```
실행 결과
```
List[0] : -2
List[1] : -1
List[2] : 0
List[3] : 1
List[4] : 2
List[5] : 3
List[6] : 4


Inserting 3000 After [2]...

List[0] : -2
List[1] : -1
List[2] : 0
List[3] : 3000
List[4] : 1
List[5] : 2
List[6] : 3
List[7] : 4
```

<br>

\# 참고<br>
[박상현 / 뇌를 자극하는 알고리즘](https://www.hanbit.co.kr/media/books/book_view.html?p_code=B3450156021){:target="_blank"}<br>