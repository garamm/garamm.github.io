---
layout: post
title: "[책/뇌를 자극하는 알고리즘] 3-1. 큐 / 순환 큐"
author: 임가람
date: "2021-08-17 22:45:00"
categories: [책, 뇌를 자극하는 알고리즘]
tags: [뇌를 자극하는 알고리즘]
---

## 큐(Queue)
- 먼저 들어간 요소가 먼저 나오는 자료구조 (선입선출, FIFO; First In First Out)

<br>

---
## 큐의 주요 기능: 삽입과 제거
큐의 가장 앞 요소를 전단(Front)이라고 하고, 마지막 요소를 후단(Rear)이라고 한다.<br>
큐에서 삽입(Enqueue)은 후단에 노드를 덧붙혀 새로운 후단을 만드는 연산이다.<br>
큐에서 제거(Dequeue)는 전단의 노드를 없애서 전단 뒤에 있는 노드를 새로운 전단으로 만드는 연산이다.
![큐-삽입과제거](/assets/img/posts/2021-08-17-queue-circular-1.png){: width="700" height="150" }

<br>

---
## 순환 큐
큐를 배열로 구현한다면, 용량이 100인 경우 전단을 제거할 때 99번의 자리 이동이 필요하다.<br>
![큐-순환큐1](/assets/img/posts/2021-08-17-queue-circular-2.png){: width="600" height="150" }<br>
그러므로 전단을 가리키는 변수를 도입해서 배열 내의 요소를 옮기지 않고 변경된 전단위 위치만 관리하게 하고, 후단을 가리키는 변수를 도입해서 삽입이 일어날 때마다 변경되는 후단의 위치를 관리한다.<br>
![큐-순환큐2](/assets/img/posts/2021-08-17-queue-circular-3.png){: width="600" height="150" }<br>
그런데 이렇게 하면 제거 연산을 수행 할수록 용량도 줄어들게된다. 그러므로 아래와 같이 시작과 끝을 연결해서 순환하도록 고안한 큐를 ‘순환 큐(Circular Queue)’라고 한다.<br>
![큐-순환큐3](/assets/img/posts/2021-08-17-queue-circular-4.png){: width="600" height="150" }<br>
![큐-순환큐4](/assets/img/posts/2021-08-17-queue-circular-5.png){: width="600" height="150" }
<br>

### 비어 있거나 또는 가득 차 있거나
순환 큐는 비어 있는 상태와 가득 찬 상태를 구분할 수 없다.<br>
![큐-순환큐5](/assets/img/posts/2021-08-17-queue-circular-6.png){: width="600" height="150" }<br>
순환 큐의 후단은 실제의 후단에 1을 더한 값을 가진다. 그러므로 큐 배열을 생성할 때 실제 용량보다 1 크게 만들어서 전단과 후단(실제의 후단) 사이를 비운다. 그러면 큐가 비었을 때 전단과 후단이 같은 곳을 가르키고, 큐가 가득 차있을 때는 후단이 전단보다 1 작은 값을 갖게 된다.<br>
![큐-순환큐5](/assets/img/posts/2021-08-17-queue-circular-7.png){: width="600" height="150" }
<br>

---
## 순환 큐 구현하기
### 순환 큐의 선언
Nodes 포인터가 가리키는 배열은 자유 저장소에 생성된다.<br>
순환 큐는 공백/포화 상태를 구분하기 위한 더미 노드(Dummy Node)를 한 개 더 가지고 있기 때문에 Capacity의 값은 실제 용량보다 하나 작다.<br>
Front는 전단의 위치, Rear는 후단의 위치를 가리킨다. 이 값들은 실제 메모리 주소는 아니고 배열 내의 인덱스이다. Rear는 실제의 후단보다 1 더 큰 값을 갖는다.<br>
![큐-구조체](/assets/img/posts/2021-08-17-queue-circular-8.png){: width="600" height="150" }
<br>

### 순환 큐의 생성과 소멸
생성: 먼저 순환 큐를 자유 저장소에 생성하고, ‘Node의 크기 x (Capacity + 1)’의 크기로 배열을 자유 저장소에 할당한다.<br>
소멸: 배열을 자유 저장소에서 제거한 후, 순환 큐 구조체를 제거한다.
<br>

### 삽입(Enqueue) 연산
후단(Rear)의 값이 용량+1과 같은 값을 갖고 있으면 후단이 배열 끝에 도달했다는 의미이므로 Rear와 Position을 0으로 지정한다.<br>
그렇지 않으면 Rear의 위치를 Position에 저장하고, Rear를 1 증가시킨다.<br>
if ~ else 작업이 끝난 후 Nodes 배열에서 Position이 가리키는 곳에 데이터를 저장한다.
![큐-삽입](/assets/img/posts/2021-08-17-queue-circular-9.png){: width="600" height="150" }
<br>

### 제거(Dequeue) 연산
전단(Front)의 위치를 Position에 저장하고, 함수 종료시 전단의 데이터를 반환할 때 배열의 인덱스로 사용한다.<br>
if ~ else 블록에서는 Front의 값이 Capacity와 같을 때 Front를 0으로 초기화하고, 그렇지 않으면 Front의 값을 1 증가시킨다.(Front == Capacity이면 전단이 배열의 끝에 도달했다는 의미)<br>
![큐-제거](/assets/img/posts/2021-08-17-queue-circular-10.png){: width="600" height="150" }
<br>

### 공백 상태 확인
전단과 후단의 값이 같으면 공백 상태라고 판정한다.
<br>

### 포화 상태 확인
(1) 순환 큐 배열 내에서 전단이 후단 앞에 위치하는 경우<br>
후단과 전단의 차가 큐의 용량과 동일한지 확인<br>
(2) 전단히 후단 뒤에 위치하는 경우<br>
Rear에 1을 더한 값이 Front와 같은지 확인
<br>

---
## 소스코드
CircularQueue.h
```c
#ifndef CIRCULAR_QUEUE_H
#define CIRCULAR_QUEUE_H

#include <stdio.h>
#include <stdlib.h>

typedef int ElementType;

typedef struct tagNode
{
	ElementType Data;
} Node;

typedef struct tagCircularQueue
{
	int Capacity; /* 용량 */
	int Front; /* 전단의 인덱스 */
	int Rear; /* 후단의 인덱스 */
	Node* Nodes; /* 노드 배열 */
} CircularQueue;

void CQ_CreateQueue( CircularQueue** Queue, int Capacity);
void CQ_DestroyQueue( CircularQueue* Queue );
void CQ_Enqueue( CircularQueue* Queue, ElementType Data);
ElementType CQ_Dequeue( CircularQueue* Queue );
int CQ_GetSize( CircularQueue* Queue );
int CQ_IsEmpty( CircularQueue* Queue );
int CQ_IsFull( CircularQueue* Queue );

#endif
```
CircularQueue.c
```c
#include "CircularQueue.h"

void CQ_CreateQueue( CircularQueue** Queue, int Capacity)
{
	/* 큐를 자유저장소에 생성 */
	(*Queue ) = ( CircularQueue*)malloc(sizeof( CircularQueue ));

	/* 입력된 Capacity+1 만큼의 노드를 자유저장소에 생성 */
	(*Queue )->Nodes = (Node*)malloc(sizeof(Node )* ( Capacity+1) );

	(*Queue )->Capacity = Capacity; /* 큐가 수용할 실제 용량을 저장 */
	(*Queue )->Front = 0;
	(*Queue )->Rear = 0;
}

void CQ_DestroyQueue( CircularQueue* Queue )
{
	free(Queue->Nodes);
	free(Queue );
}

void CQ_Enqueue( CircularQueue* Queue, ElementType Data)
{
	int Position=0;
	if(Queue->Rear==Queue->Capacity)
	{
		Position=Queue->Rear;
		Queue->Rear=0;
	}
	else
		Position=Queue->Rear++;
		
	Queue->Nodes[Position].Data=Data;
}

ElementType CQ_Dequeue( CircularQueue* Queue )
{
	int Position = Queue->Front;

	if ( Queue->Front == Queue->Capacity )
		Queue->Front = 0;
	else
		Queue->Front++;

	return Queue->Nodes[Position].Data;
}

int CQ_GetSize( CircularQueue* Queue )
{
	if ( Queue->Front <= Queue->Rear )
		return Queue->Rear - Queue->Front;
	else
		return Queue->Rear + (Queue->Capacity - Queue->Front) + 1;
}

int CQ_IsEmpty( CircularQueue* Queue )
{
	return (Queue->Front == Queue->Rear);
}

int CQ_IsFull( CircularQueue* Queue )
{
	if ( Queue->Front < Queue->Rear )
		return ( Queue->Rear - Queue->Front) == Queue->Capacity;
	else
		return ( Queue->Rear + 1 ) == Queue->Front;
}
```
Test_CircularQueue.c
```c
#include "CircularQueue.h"

int main( void )
{
	int i;
	CircularQueue* Queue;

	CQ_CreateQueue(&Queue, 10);
	CQ_Enqueue( Queue, 1 );
	CQ_Enqueue( Queue, 2 );
	CQ_Enqueue( Queue, 3 );
	CQ_Enqueue( Queue, 4 );

	for ( i=0; i<3; i++)
	{
		printf( "Dequeue: %d, ", CQ_Dequeue( Queue ) );
		printf( "Front:%d, Rear:%d\n", Queue->Front, Queue->Rear );
	}
	i = 100;
	while ( CQ_IsFull( Queue ) == 0 )
	{
		CQ_Enqueue( Queue, i++ );
	}

	printf( "Capacity: %d, Size: %d\n\n",
	Queue->Capacity, CQ_GetSize(Queue ) );

	while ( CQ_IsEmpty( Queue ) == 0)
	{
		printf( "Dequeue: %d, ", CQ_Dequeue( Queue ) );
		printf( "Front:%d, Rear:%d\n", Queue->Front, Queue->Rear );
	}

	CQ_DestroyQueue( Queue );

	return 0;
}
```
실행결과
```
Dequeue: 1, Front: 1, Rear: 4
Dequeue: 2, Front: 2, Rear: 4
Dequeue: 3, Front: 3, Rear: 4
Capacity: 10, Size: 10

Dequeue: 1, Front: 4, Rear: 4
Dequeue: 100, Front: 5, Rear: 2
Dequeue: 101, Front: 6, Rear: 2
Dequeue: 102, Front: 7, Rear: 2
Dequeue: 103, Front: 8, Rear: 2
Dequeue: 104, Front: 9, Rear: 2
Dequeue: 105, Front: 10, Rear: 2
Dequeue: 106, Front: 0, Rear: 2
Dequeue: 107, Front: 1, Rear: 2
Dequeue: 108, Front: 2, Rear: 2
```

<br>

\# 참고<br>
[박상현 / 뇌를 자극하는 알고리즘](https://www.hanbit.co.kr/media/books/book_view.html?p_code=B3450156021){:target="_blank"}<br>