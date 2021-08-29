---
layout: post
title: "[책/뇌를 자극하는 알고리즘] 2-2. 스택 / 링크드 리스트로 구현하기"
author: 임가람
date: "2021-08-14 11:23:00"
categories: [책, 뇌를 자극하는 알고리즘]
tags: [뇌를 자극하는 알고리즘]
---

## 스택과 스택의 노드 표현하기
![스택-링스크리스트-구조](/assets/img/posts/2021-08-14-stack-linkedlist-1.png){: width="600" height="300" }<br>
배열 기반의 스택과는 다르게 스택의 용량, 최상위 노드의 인덱스가 없고, 리스트의 헤드와 테일에 대한 포인터가 있다.<br>
Top 포인터는 링크드 리스트의 테일을 가리킨다.<br>
![스택-링스크리스트-구조2](/assets/img/posts/2021-08-14-stack-linkedlist-2.png){: width="700" height="300" }

<br>

---
<br>

## 스택의 기본 연산

### 스택의 생성과 소멸
생성: LinkedListStack 구조체를 자유 저장소에 할당한다.<br>
소멸: 스택 내의 모든 노드를 제거한 후, free를 통해 스택을 자유 저장소에서 해제해준다.

<br>

### 삽입(Push) 연산
최상위 노드의 테일을 찾아 새 노드를 연결하고, Top 필드에 최상위 노드의 주소를 등록한다.

<br>

### 제거(Pop) 연산
- 현재 최상위 노드의 주소를 다른 포인터에 복사해 둔다.
- 새로운 최상위 노드(현재 최상위 노드의 바로 이전(아래) 노드)를 찾는다.
- LinkedListStack 구조체의 Top 필드에 새로운 최상위 노드의 주소를 등록한다.
- \1번에서 다른 포인터에 저장해둔 옛 최상위 노드의 주소를 반환한다.

<br>

---
<br>

## 예제 코드

LinkedListStack.h
```c
#ifndef LINKEDLIST_STACK_H
#define LINKEDLIST_STACK_H

#include <stdio.h>
#include <string.h>
#include <stdlib.h>

typedef struct tagNode
{
    char* Data;
    struct tagNode* NextNode;
} Node;

typedef struct tagLinkedListStack
{
    Node* List;
    Node* Top;
} LinkedListStack;

void  LLS_CreateStack( LinkedListStack** Stack );
void  LLS_DestroyStack( LinkedListStack* Stack );

Node* LLS_CreateNode( char* Data );
void  LLS_DestroyNode( Node* _Node );

void  LLS_Push( LinkedListStack* Stack, Node* NewNode );
Node* LLS_Pop( LinkedListStack* Stack );

Node* LLS_Top( LinkedListStack* Stack );
int   LLS_GetSize( LinkedListStack* Stack );
int   LLS_IsEmpty( LinkedListStack* Stack );

#endif LINKEDLIST_STACK_H
```
LinkedListStack.c
```c
#include "LinkedListStack.h"

void  LLS_CreateStack( LinkedListStack** Stack )
{
    /*  스택을 자유저장소에 생성 */
    (*Stack )       = ( LinkedListStack*)malloc(sizeof( LinkedListStack ) );
    (*Stack )->List = NULL;
    (*Stack )->Top  = NULL;
}

void LLS_DestroyStack( LinkedListStack* Stack )
{
    while ( !LLS_IsEmpty(Stack ) )
    {
        Node* Popped = LLS_Pop( Stack );
        LLS_DestroyNode(Popped);    
    }

    /*  스택을 자유 저장소에서 해제 */
    free(Stack );
}

Node* LLS_CreateNode( char* NewData )
{
    Node* NewNode = ( Node*)malloc(sizeof( Node ) );
    NewNode->Data = ( char*)malloc(strlen( NewData ) + 1);

    strcpy( NewNode->Data, NewData );  /*  데이터를 저장한다. */

    NewNode->NextNode = NULL; /*  다음 노드에 대한 포인터는 NULL로 초기화 한다. */

    return NewNode;/*  노드의 주소를 반환한다. */
}

void  LLS_DestroyNode( Node* _Node )
{
    free(_Node->Data );
    free(_Node );
}

void LLS_Push( LinkedListStack* Stack, Node* NewNode )
{
    if ( Stack->List == NULL ) 
    {        
        Stack->List = NewNode;
    } 
    else
    {
        /*  최상위 노드를 찾아 NewNode를 연결한다(쌓는다). */
        Node* OldTop = Stack->List;
        while ( OldTop->NextNode != NULL )
        {
            OldTop = OldTop->NextNode;
        }

        OldTop->NextNode = NewNode;
    }
    
    /*  스택의 Top 필드에 새 노드의 주소를 등록한다. */
    Stack->Top = NewNode;
}

Node* LLS_Pop( LinkedListStack* Stack )
{
    /*  LLS_Pop() 함수가 반환할 최상위 노드 */
    Node* TopNode = Stack->Top;

    if ( Stack->List == Stack->Top )
    {
        Stack->List = NULL;
        Stack->Top  = NULL;
    }
    else
    {
        /*  새로운 최상위 노드를 스택의 Top 필드에 등록한다. */
        Node* CurrentTop = Stack->List;
        while ( CurrentTop != NULL && CurrentTop->NextNode != Stack->Top )
        {
            CurrentTop = CurrentTop->NextNode;
        }

        Stack->Top = CurrentTop;
        CurrentTop->NextNode = NULL;
    }

    return TopNode;
}

Node* LLS_Top( LinkedListStack* Stack )
{
    return Stack->Top;
}

int LLS_GetSize( LinkedListStack* Stack )
{
    int    Count = 0;
    Node*  Current = Stack->List;

    while ( Current != NULL )
    {
        Current = Current->NextNode;
        Count++;
    }

    return Count;
}

int LLS_IsEmpty( LinkedListStack* Stack )
{
    return (Stack->List == NULL);
}
```
Test_LinkedListStack.c
```c
#include "LinkedListStack.h"

int main( void )
{
    int i= 0;
    int Count = 0;
    Node* Popped;

    LinkedListStack* Stack;

    LLS_CreateStack(&Stack);
    
    LLS_Push( Stack, LLS_CreateNode("abc") );
    LLS_Push( Stack, LLS_CreateNode("def") );
    LLS_Push( Stack, LLS_CreateNode("efg") );
    LLS_Push( Stack, LLS_CreateNode("hij") );

    Count = LLS_GetSize(Stack);
    printf( "Size: %d, Top: %s\n\n", 
        Count, LLS_Top(Stack)->Data );

    for ( i=0; i<Count; i++ )
    {
        if ( LLS_IsEmpty(Stack) )
            break;

        Popped = LLS_Pop( Stack );

        printf( "Popped: %s, ", Popped->Data );

        LLS_DestroyNode(Popped);

        if ( ! LLS_IsEmpty(Stack) ) 
        {
            printf( "Current Top: %s\n", LLS_Top(Stack)->Data );
        }
        else
        {
            printf( "Stack Is Empty.\n");
        }
    }

    LLS_DestroyStack(Stack);

    return 0;
}
```
실행 결과
```
Size: 4, Top: hij

Popped: hij, Current Top: efg
Popped: efg, Current Top: def
Popped: def, Current Top: abc
Popped: abc, Stack Is Empty.
```

<br>

\# 참고<br>
[박상현 / 뇌를 자극하는 알고리즘](https://www.hanbit.co.kr/media/books/book_view.html?p_code=B3450156021){:target="_blank"}<br>