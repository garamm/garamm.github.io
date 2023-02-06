---
layout: post
title: "[책/뇌를 자극하는 알고리즘] 9-1. 그래프 / 인접행렬, 인접리스트"
author: 임가람
date: "2021-09-10 22:45:00"
categories: [책, 뇌를 자극하는 알고리즘]
tags: [뇌를 자극하는 알고리즘]
---

## 오일러의 도구
오일러는 7개의 다리를 한 번씩만 건너서 쾨니히스베르크의 모든 지역을 밟는 것이 불가능함을 증명했다.<br>
[쾨니히스베르크의 다리 문제](https://ko.wikipedia.org/wiki/쾨니히스베르크의_다리_문제){:target="_blank"}<br>
오일러는 이 문제를 풀기 위해 그래프를 고안해내고, 이를 도구로 이용하여 증명했다.<br>
오일러는 다리를 단순하게 만들어 각 육지를 정점(Vertex), 각 육지를 잇는 다리를 간선(Edge)로 표시했다. 이렇게 하면 쾨니히스베르크를 4개의 정점과 7개의 간선으로 나타낼 수 있다.<br>
![그래프-쾨니히스베르크의다리문제](/assets/img/posts/2021-09-10-graph-adjacency-matrix-list-1.png){: width="700" height="150" }<br>

이제 7개의 간선들을 한 번씩만 따라 그어 도형을 완성할 수 있는지의 여부만 확인하면 증명할 수 있다.<br>
한붓그리기는 연결되어 있는 선이 홀수개인 점이 아예 없거나, 두 개만 있는 도형 위에서만 가능하다.<br>
쾨니히스베르크의 그래프에서 정점 A, B, C, D 네 개는 모두 홀수개의 선으로 연결되어 있기 때문에 한붓그리기가 불가능하기 때문에 쾨니히스베르크의 모든 육지를 밟는 것은 불가능하다.
<br>

---
## 그래프(Graph)
'정점의 모음'과 이 정점을 잇는 '간선의 모음'과의 결합<br>
![그래프-1](/assets/img/posts/2021-09-10-graph-adjacency-matrix-list-2.png){: width="700" height="150" }<br>
`정점의 집합을 V, 간선의 집합을 E, 그리고 그래프를 G라고 했을 때, G = (V, E) 이다.`<br>
<br>
간선으로 연결되어 있는 두 정점을 가리켜 서로 '인접(adjacent)' 또는 이웃 관계에 있다고 한다.<br>
아래 그림에서는 (A, B), (A, D), (A, E), (B, C), (B, E), (C, D)가 서로 이웃 관계에 있다.<br>

![그래프-2](/assets/img/posts/2021-09-10-graph-adjacency-matrix-list-3.png){: width="700" height="150" }<br>

이렇게 간선을 통해 서로 이웃이 된 각 정점은 그래프 안에서의 길을 형성하기도 하는데 이를 '경로(Path)'라고 한다.<br>
경로는 길이를 가지는데, 길이는 정점과 정점 사이에 있는 간선의 수를 의미한다.<br>
예를 들어 정점 A에서 정점 C까지의 경로는 'A, B, C'이고, 경로의 길이는 2((A, B)와 (B, C))이다.<br>
한편, 어느 경로가 정점 하나를 두 번 이상 거치도록 되어있다면 그 경로를 '사이클(Cycle)'이라고 한다. 위 그림의 사이클은 아래와 같다.<br>

![그래프-3](/assets/img/posts/2021-09-10-graph-adjacency-matrix-list-4.png){: width="700" height="150" }<br>

간선이 방향성을 가지면 방향성 그래프(Directed Graph)가 되고, 방향성이 없으면 무방향성 그래프(Undirected Graph)가 된다.<br>
![그래프-4](/assets/img/posts/2021-09-10-graph-adjacency-matrix-list-5.png){: width="700" height="150" }<br>

그래프에는 '연결성(Connectivity)'이 존재한다. 무방향성 그래프 내의 두 정점 사이에 경로(Path)가 존재하면 "이 두 정점이 연결되어 있다"고 한다.<br>
그리고 그래프 내의 각 정점들이 다은 모든 정점들과 연결되어 있으면 "이 그래프는 연결되었다"라고 한다.<br>
<br>

---
## 그래프의 표현
정접의 집합은 배열이나 링크드 리스트, 어떤 자료구조를 사용해도 표현이 쉽지만, 간선의 집합을 표현하는 것은 쉽지 않다.<br>
정점 사이의 인접 관계를 나타내는 방법에는 아래와 같이 두 가지가 있다.
- 인접 행렬(Adjacency Matrix): 행렬을 이용하는 방식
- 인접 리스트(Adjacency List): 리스트를 이용하는 방식

<br>

### 인접 행렬(Adjacency Matrix)
그래프의 정점의 수는 N이라고 한다면, N*N 크기의 행렬을 만들어 행렬의 각 원소를 한 정점과 또 다른 정점이 인접해 있는 경우(즉, 정점 사이의 간선이 존재하는 경우)에는 1로 표시하고, 그렇지 않으면 0으로 표현한다.<br>
![그래프-인접행렬1](/assets/img/posts/2021-09-10-graph-adjacency-matrix-list-6.png){: width="700" height="150" }<br>

위 행렬에서 (1, 1), (2, 2), (3, 3), (4, 4), (5, 5)를 따라 선을 그으면 인접 행렬이 주 대각선에 대해 대칭(Symmentric)을 이루고 있음을 알 수 있다.<br>
그래프가 무방향성인 경우에 인접 행렬은 이처럼 주 대각선에 대해 대칭을 이루는 것이 특징이다.<br>
![그래프-인접행렬2](/assets/img/posts/2021-09-10-graph-adjacency-matrix-list-7.png){: width="700" height="150" }<br>

방향성 그래프에서는 다음과 같이 인접 행렬을 표현한다.<br>
![그래프-인접행렬3](/assets/img/posts/2021-09-10-graph-adjacency-matrix-list-8.png){: width="700" height="150" }<br>

<br>

### 인접 리스트(Adjacency List)
각 정점을 자신과 인접해 있는 모든 정점의 목록을 리스트로 관리<br>
![그래프-인접리스트](/assets/img/posts/2021-09-10-graph-adjacency-matrix-list-9.png){: width="700" height="150" }<br>

<br>

### 인접 행렬 VS 인접 리스트

||인접 행렬|인접 리스트|
|---|---|---|
|장점|정점 간의 인접 여부를 빠르게 알 수 있다.|정점과 간선의 삽입이 빠르고 인접 관계를 표시하는 리스트에 사용되는 메모리의 양이 적다.|
|단점|인접 관계를 행렬 형태로 저장하기 위해 사용하는 메모리의 양이 `정점의 크기*N`에 이른다.|정점 간의 인접 여부를 알아내기 위해 인접 리스트를 타고 순차 탐색을 해야한다.|

즉 작성하는 프로그램의 목적에 따라 어느 자료구조를 선택할 것인가가 달라진다.<br>
그래프 내의 정점의 수가 많지 않거나 정점끼리의 인접 여부를 빠르게 알아내야 하는 경우 인접 행렬을 사용하고<br>
정점과 간선의 입력이 빈번하게 이루어지고 메모리 효율성을 우선시 한다면 인접 리스트를 사용하면 된다.<br>
<br>

### 인접 리스트 예제 프로그램

Graph.h
```c
#ifndef GRAPH_H
#define GRAPH_H

#include <stdio.h>
#include <stdlib.h>

enum VisitMode { Visited, NotVisited };

typedef int ElementType;

typedef struct tagVertex
{
    ElementType       Data;             /* 데이터를 담는 필드 */
    int               Visited;          /* 그래프 순회 알고리즘에 사용하는 필드로, 방문 여부를 나타냄 */
    int               Index;            /* 정점의 인덱스(ex. 그래프의 첫 번째 정점은 0번, 두 번쨰 정점은 1번) */

    struct tagVertex* Next;             /* 다음 정점을 가리키는 포인터 */
    struct tagEdge*   AdjacencyList;    /* 인접 정점의 목록에 대한 포인터 */
} Vertex;

typedef struct tagEdge
{
    int    Weight;          /* 간선의 가중치로, 최소 신장 트리나 최단 경로 탐색 알고리즘에서 정점 사이의 거리나 비용 등을 표현하기 위해 사용 */
    struct tagEdge* Next;   /* 다음 간선을 가리키는 포인터 */
    Vertex* From;           /* 간선의 시작 정점 */
    Vertex* Target;         /* 간선의 끝 정점 */
} Edge;

typedef struct tagGraph
{
    Vertex* Vertices;       /* 정점 목록에 대한 포인터 */
    int     VertexCount;    /* 정점의 수 */
} Graph;

Graph* CreateGraph();
void   DestroyGraph( Graph* G );

Vertex* CreateVertex( ElementType Data );
void    DestroyVertex( Vertex* V );

Edge*   CreateEdge( Vertex* From, Vertex* Target, int Weight );
void    DestroyEdge( Edge* E );

void   AddVertex( Graph* G, Vertex* V );
void   AddEdge( Vertex* V, Edge* E );
void   PrintGraph ( Graph* G );

#endif
```
Graph.c
```c
#include "Graph.h"

Graph* CreateGraph()
{
    Graph* graph = (Graph*)malloc( sizeof( Graph ) );
    graph->Vertices = NULL;
    graph->VertexCount = 0;

    return graph;
}

void DestroyGraph( Graph* G )
{
    while ( G->Vertices != NULL )
    {
        Vertex* Vertices = G->Vertices->Next;        
        DestroyVertex( G->Vertices );
        G->Vertices = Vertices;        
    }

    free( G );
}

Vertex* CreateVertex( ElementType Data )
{
    Vertex* V = (Vertex*)malloc( sizeof( Vertex ) );
    
    V->Data = Data;
    V->Next = NULL;
    V->AdjacencyList = NULL;
    V->Visited = NotVisited;
    V->Index = -1;

    return V;
}

void DestroyVertex( Vertex* V )
{
    while ( V->AdjacencyList != NULL )
    {
        Edge* Edge = V->AdjacencyList->Next;

        DestroyEdge ( V->AdjacencyList );

        V->AdjacencyList = Edge;
    }

    free( V );    
}

Edge*   CreateEdge( Vertex* From, Vertex* Target, int Weight )
{
    Edge* E   = (Edge*)malloc( sizeof( Edge ) );
    E->From   = From;
    E->Target = Target;
    E->Next   = NULL;
    E->Weight = Weight;

    return E;
}

void    DestroyEdge( Edge* E )
{
    free( E );
}

void AddVertex( Graph* G, Vertex* V )
{
    Vertex* VertexList = G->Vertices;

    if ( VertexList == NULL ) 
    {
        G->Vertices = V;
    } 
    else 
    {
        while ( VertexList->Next != NULL )
            VertexList = VertexList->Next;

        VertexList->Next = V;
    }

    V->Index = G->VertexCount++;
}

void AddEdge( Vertex* V, Edge* E )
{
    if ( V->AdjacencyList == NULL ) 
    {
        V->AdjacencyList = E;
    } 
    else
    {
        Edge* AdjacencyList = V->AdjacencyList;

        while ( AdjacencyList->Next != NULL )
            AdjacencyList = AdjacencyList->Next;

        AdjacencyList->Next = E;
    }
}

void PrintGraph ( Graph* G )
{
    Vertex* V = NULL;
    Edge*   E = NULL;

    if ( ( V = G->Vertices ) == NULL )
        return;

    while ( V != NULL )
    {
        printf( "%c : ", V->Data );        
        
        if ( (E = V->AdjacencyList) == NULL ) {
            V = V->Next;
            printf("\n");
            continue;
        }

        while ( E != NULL )
        {
            printf("%c[%d] ", E->Target->Data, E->Weight );
            E = E->Next;
        }

        printf("\n");

        V = V->Next;    
    }   

    printf("\n");
}
```
Test_Graph.c
```c
#include "Graph.h"

int main( void )
{
    /*  그래프 생성     */
    Graph* G = CreateGraph();
    
    /*  정점 생성 */
    Vertex* V1 = CreateVertex( '1' );
    Vertex* V2 = CreateVertex( '2' );
    Vertex* V3 = CreateVertex( '3' );
    Vertex* V4 = CreateVertex( '4' );
    Vertex* V5 = CreateVertex( '5' );

    /*  그래프에 정점을 추가 */
    AddVertex( G, V1 );
    AddVertex( G, V2 );
    AddVertex( G, V3 );
    AddVertex( G, V4 );
    AddVertex( G, V5 );

    /*  정점과 정점을 간선으로 잇기 */
    AddEdge( V1, CreateEdge(V1, V2, 0) );
    AddEdge( V1, CreateEdge(V1, V3, 0) );
    AddEdge( V1, CreateEdge(V1, V4, 0) );
    AddEdge( V1, CreateEdge(V1, V5, 0) );

    AddEdge( V2, CreateEdge(V2, V1, 0) );
    AddEdge( V2, CreateEdge(V2, V3, 0) );
    AddEdge( V2, CreateEdge(V2, V5, 0) );

    AddEdge( V3, CreateEdge(V3, V1, 0) );
    AddEdge( V3, CreateEdge(V3, V2, 0) );

    AddEdge( V4, CreateEdge(V4, V1, 0) );
    AddEdge( V4, CreateEdge(V4, V5, 0) );

    AddEdge( V5, CreateEdge(V5, V1, 0) );
    AddEdge( V5, CreateEdge(V5, V2, 0) );
    AddEdge( V5, CreateEdge(V5, V4, 0) );

    PrintGraph( G );

    /*  그래프 소멸 */
    DestroyGraph( G );

    return 0;
}
```
실행결과
```
1 : 2[0] 3[0] 4[0] 5[0]
2 : 1[0] 3[0] 5[0]
3 : 1[0] 2[0]
4 : 1[0] 5[0]
5 : 1[0] 2[0] 4[0]
```


<br>

\# 참고<br>
[박상현 / 뇌를 자극하는 알고리즘](https://www.hanbit.co.kr/media/books/book_view.html?p_code=B3450156021){:target="_blank"}<br>