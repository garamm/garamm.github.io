---
layout: post
title: "[책/뇌를 자극하는 알고리즘] 2-1. 스택 / 배열로 구현하기"
author: 임가람
date: "2021-08-13 23:07:00"
categories: [책, 뇌를 자극하는 알고리즘]
tags: [뇌를 자극하는 알고리즘]
---

## 스택의 구조

처음에 들어간 요소가 나중에 나오는 선입후출(FILO; First In Last Out) 구조<br>
![스택-배열-구조](/assets/img/posts/2021-08-13-stack-array-1.png){: width="150" height="150"}
<br>

---
## 스택의 주요 기능

### 삽입과 제거
- 삽입: 스택 위에 새로운 노드를 쌓는다.
- 제거: 스택에서 최상위 노드를 걷어낸다.

![스택-배열-삽입/제거](/assets/img/posts/2021-08-13-stack-array-2.png){: width="600" height="600" }
<br>

---
## 배열로 구현하는 스택
- 용량을 조절하기 어렵지만, 구현이 간단하다.
- 각 노드를 동적으로 생성하고 제거하는 대신 스택 생성 초기에 용량 만큼의 노드를 한꺼번에 생성한다.
- 최상위 노드의 위치를 나타내는 변수를 이용하여 삽입, 제거 연산을 수행한다.

### 스택의 노드 표현하기
스택 구조체
- 용량: 스택의 한계를 알기 위해 사용됨
- 최상위 노드의 위치: 삽입/제거 시에 최상위 노드에 접근할 때 사용
- 노드 배열: 노드 보관

Nodes의 포인터는 자유 저장소에 할당한 배열의 첫 번째 요소를 가리킨다.
![스택-배열-구조체](/assets/img/posts/2021-08-13-stack-array-3.png){: width="600" height="600" }
<br>

---
## 스택의 기본 연산

### 스택의 생성과 소멸
malloc()을 통해 ArrayStack을 자유 저장소에 올리고, 다시 malloc()을 통해 스택 한헤 있는 노드를 입력된 용량만큼 자유 저장소에 생성한다. Top은 0으로 초기화해두고 노드가 쌓일 때마다 1씩 증가하고, 제거될 때마다 1씩 감소한다.


### 삽입(Push) 연산
Top에 1을 더한 곳에 새 노드를 놓는다.<br>
단 소스코드에서는 수가 0부터 시작되기 때문에 Top 값을 그대로 새 노드의 인덱스로 사용하고, 새 노드를 스택에 올린 후 Top의 값을 1만큼 증가시킨다.


### 제거(Pop) 연산
최상위 노드의 인덱스(Top)값을 1만큼 낮추고, 최상위 노드의 데이터를 호출자에게 반환한다.<br>
단, 소스코드에서는 실제 최상위 노드가 배열 내의 인덱스 값보다 1만큼 더 크므로 ‘최상위 노드의 인덱스 == Top - 1’이다.
<br>

---
## 예제 코드
ArrayStack.h
```c

#ifndef ARRAYSTACK_H
#define ARRAYSTACK_H

#include <stdio.h>
#include <stdlib.h>

typedef int ElementType;

typedef struct tagNode
{
    ElementType Data;
} Node;

typedef struct tagArrayStack
{
    int  Capacity; /* 용량 */
    int  Top; /* 최상위 노드의 위치 */
    Node* Nodes; /* 노드 배열;*/
} ArrayStack;

void        AS_CreateStack(ArrayStack** Stack, int Capacity);
void        AS_DestroyStack(ArrayStack* Stack);
void        AS_Push(ArrayStack* Stack, ElementType Data);
ElementType AS_Pop(ArrayStack* Stack);
ElementType AS_Top(ArrayStack* Stack);
int        AS_GetSize(ArrayStack* Stack);
int        AS_IsEmpty(ArrayStack* Stack);

#endif ARRAYSTACK_H
```
ArrayStack.c
```c

#include "ArrayStack.h"

void  AS_CreateStack(ArrayStack** Stack, int Capacity)
{
    /*  스택을 자유저장소에 생성 */
    (*Stack)          = (ArrayStack*)malloc(sizeof(ArrayStack));

    /*  입력된 Capacity만큼의 노드를 자유저장소에 생성 */
    (*Stack)->Nodes    = (Node*)malloc(sizeof(Node)*Capacity);

    /*  Capacity 및 Top 초기화 */
    (*Stack)->Capacity = Capacity;
    (*Stack)->Top = 0;
}

void AS_DestroyStack(ArrayStack* Stack)
{
    /*  노드를 자유 저장소에서 해제 */
    free(Stack->Nodes);

    /*  스택을 자유 저장소에서 해제 */
    free(Stack);
}

void AS_Push(ArrayStack* Stack, ElementType Data)
{
    int Position = Stack->Top;

    Stack->Nodes[Position].Data = Data;
    Stack->Top++;
}

ElementType AS_Pop(ArrayStack* Stack)
{
    int Position = --( Stack->Top );

    return Stack->Nodes[Position].Data;
}

ElementType AS_Top(ArrayStack* Stack)
{
    int Position = Stack->Top - 1;

    return Stack->Nodes[Position].Data;
}

int AS_GetSize(ArrayStack* Stack)
{
    return Stack->Top;
}

int AS_IsEmpty(ArrayStack* Stack)
{
    return (Stack->Top == 0);
}
```
Test_ ArrayStack.c
```c

#include "ArrayStack.h"

int main( void )
{
    int i= 0;
    ArrayStack* Stack = NULL;

    AS_CreateStack(&Stack, 10);
    
    AS_Push(Stack, 3);
    AS_Push(Stack, 37);
    AS_Push(Stack, 11);
    AS_Push(Stack, 12);

    printf( "Capacity: %d, Size: %d, Top: %d\n\n", 
        Stack->Capacity, AS_GetSize(Stack), AS_Top(Stack) );

    for ( i=0; i<4; i++ )
    {
        if ( AS_IsEmpty(Stack) )
            break;
        
        printf( "Popped: %d, ", AS_Pop(Stack) );
        
        if ( ! AS_IsEmpty(Stack) )
            printf( "Current Top: %d\n", AS_Top(Stack) );
        else
            printf( "Stack Is Empty.\n" );
    }

    AS_DestroyStack(Stack);

    return 0;
}
```
실행결과
```
Capacity: 10, Size: 4, Top: 12

Popped: 12, Current Top: 11
Popped: 11, Current Top: 37
Popped: 37, Current Top: 3
Popped: 3, Stack Is Empty.
```


\# 참고
[박상현 / 뇌를 자극하는 알고리즘](https://www.hanbit.co.kr/media/books/book_view.html?p_code=B3450156021){:target="_blank"}