---
layout: post
title: "[책/뇌를 자극하는 알고리즘] 4-4. 분리 집합"
author: 임가람
date: "2021-08-22 14:57:00"
categories: [책, 뇌를 자극하는 알고리즘]
tags: [뇌를 자극하는 알고리즘]
---

## 분리 집합(Disjoint Set)
서로 공통된 원소를 갖지 않는 집합을 의미<br>
두개 이상의 집합을 일컬을 때만 사용할 수 있는 개념<br>
분리 집합에는 교집합(Intersection)이 없고 합집합(Union)만 존재한다.<br>
집합 탐색이란 어떤 한 원소가 소속되어 있는 집합을 알아내는 것을 말한다.
<br>

---
## 분리 집합의 표현
일반 트리나 이진 트리는 모두 부모가 가직을 가리키는 포인터를 갖고 있었지만, 분리 집합은 자식이 부모를 가리킨다.<br>
루트 노드는 집합 그 자체이고, 루트 노드 자신을 포함한 트리 내의 모든 노드는 그 집합에 소속 된다.
<br>

---
## 분리 집합의 연산
분리 집합의 목적은 원소가 어느 집합에 귀속되어 있는지를 알기 위함이므로 분리 집합의 원소들을 정렬하거나 순차적으로 접근하게 하지 않는다.

### 합집합
![분리집합-합집합](/assets/img/posts/2021-08-22-disjoint-set-1.png){: width="500" height="150" }
<br>

### 집합 탐색
집합 탐색(Find) 연산은 집합에서 원소를 찾는 것이 아니라, 원소가 속해 있는 집합을 찾는 연산이다.<br>
해당 원소(노드)가 어떤 집합에 속해 있는지 알기 위해서는 이 원소가 속해 있는 트리의 루트 노드를 찾으면 된다.<br>
각 노드는 '부모' 노드를 가리키는 포인터를 갖고 있기 때문에 포인터를 쭉 타고 올라가다가 부모가 NULL인 노드를 만나면 그 노드가 루트 노드 이므로 그것을 반환하면 된다.
<br>

---
## 소스코드

DisjointSet.h
```c
#ifndef DISJOINTSET_H
#define DISJOINTSET_H

#include <stdio.h>
#include <stdlib.h>

typedef struct tagDisjointSet
{
    struct tagDisjointSet* Parent;
    void*                  Data;
} DisjointSet;

void DS_UnionSet( DisjointSet* Set1, DisjointSet* Set2 );
DisjointSet* DS_FindSet( DisjointSet* Set );
DisjointSet* DS_MakeSet( void* NewData );
void DS_DestroySet( DisjointSet* Set );

#endif
```
DisjointSet.c
```c
#include "DisjointSet.h"

void DS_UnionSet( DisjointSet* Set1, DisjointSet* Set2 )
{
    Set2 = DS_FindSet(Set2);
    Set2->Parent = Set1;
}

DisjointSet* DS_FindSet( DisjointSet* Set )
{
    while ( Set->Parent != NULL )
    {
        Set = Set->Parent;
    }

    return Set;
}

DisjointSet* DS_MakeSet( void* NewData )
{
    DisjointSet* NewNode = (DisjointSet*)malloc(sizeof(DisjointSet));
    NewNode->Data   = NewData;
    NewNode->Parent = NULL;

    return NewNode;
}

void DS_DestroySet( DisjointSet* Set )
{
    free( Set );
}
```
Test_DisjointSet.c
```c
#include <stdio.h>
#include "DisjointSet.h"

int main( void )
{
	int a=1, b=2, c=3, d=4;
	DisjointSet* Set1 = DS_MakeSet(&a);
	DisjointSet* Set2 = DS_MakeSet(&b);
	DisjointSet* Set3 = DS_MakeSet(&c);
	DisjointSet* Set4 = DS_MakeSet(&d);

    printf("Set1 == Set2 : %d \n", DS_FindSet( Set1 ) == DS_FindSet( Set2 ) );

	DS_UnionSet( Set1, Set3 );
    printf("Set1 == Set3 : %d \n", DS_FindSet( Set1 ) == DS_FindSet( Set3 ) );

    DS_UnionSet( Set3, Set4 );
    printf("Set3 == Set4 : %d \n", DS_FindSet( Set3 ) == DS_FindSet( Set4 ) );

	return 0;
}
```
실행결과
```
Set1 == Set2 : 0
Set1 == Set3 : 1
Set3 == Set4 : 1
```

<br>

\# 참고<br>
[박상현 / 뇌를 자극하는 알고리즘](https://www.hanbit.co.kr/media/books/book_view.html?p_code=B3450156021){:target="_blank"}<br>