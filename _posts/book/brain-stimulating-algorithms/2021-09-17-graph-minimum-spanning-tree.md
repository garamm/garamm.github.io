---
layout: post
title: "[책/뇌를 자극하는 알고리즘] 9-4. 그래프 / 최소 신장 트리, 프림 알고리즘, 크루스칼 알고리즘"
author: 임가람
date: "2021-09-17 23:20:00"
categories: [책, 뇌를 자극하는 알고리즘]
tags: [뇌를 자극하는 알고리즘]
---

## 최소 신장 트리
그래프는 정접의 집합과 간선의 집합으로 이루어져 있다. 그리고 간선은 정점과 정점의 인접 관계를 설명하는 요소이다.<br>
여기에 가중치(Weight) 속성을 추가한다. 간선이 가중치를 가지면 그래프로 표현할 수 있는 것이 훨씬 다양해진다. 예를 들어 지도상에서 도시를 정점으로 표시하고 도시간의 도로를 간선으로 표시한 다음, 도시 간의 거리를 가중치로 표현할 수 있다.<br>
![최소신장트리1](/assets/img/posts/2021-09-17-graph-minimum-spanning-tree-1.png){: width="700" height="150" }<br>

Spanning Tree(신장 트리)의 Spanning은 떨어져 있는 둘 사이를 다리 등으로 연결한다는 뜻으로 사용된다. 즉 신장 트리는 그래프의 모든 정점을 연결하는 트리이다. 신장 트리는 그래프의 하위 개념이기도 하다.<br>
아래 그림을 보면 그래프의 사이클을 형성하는 간선만 제거하면 트리로 변한다.<br>
![최소신장트리2](/assets/img/posts/2021-09-17-graph-minimum-spanning-tree-2.png){: width="700" height="150" }<br>

최소 신장 트리는 최소 가중치 신장 트리(Minimum Weight Spanning Tree)라고도 부른다. 각 간선이 갖고 있는 가중치의 합이 최소가 되는 신장 트리가 바로 최소 신장 트리이다.<br>
최소 신장 트리 알고리즘은 일반적으로 프림 알고리즘(Prim's Algorithm)과 크루스칼 알고리즘(Kruskal's Algorithm)을 사용해 만든다.<br>
<br>

---
## 프림 알고리즘(Prim's Algorithm)
### 프림 알고리즘이란?
로버트 프림(Robert C. Prim)이 고안한 그래프에서 최소 신장 트리를 만들어 내는 알고리즘<br>
프림 알고리즘을 통해 최소 신장 트리를 만들어 내는 과정
1. 그래프와 최소 신장 트리를 준비한다. 이때의 최소 신장 트리는 노드가 하나도 없는 상태이다.
2. 그래프에서 임의의 정점을 시작 정점으로 선택하여 최소 신장 트리의 루트 노드로 삽입한다.
3. 최소 신장 트리에 삽입되어 있는 정점들과 이 정점들의 모든 인접 정점 사이에 있는 간선의 가중치를 조사한다. 간선 중에 가장 가중치가 작은 것을 골라 이 간선에 연결되어 있는 인접 정점을 최소 신장 트리에 삽입한다. 단, 새로 삽입되는 정점은 최소 신장 트리에 삽입 되어 있는 기존의 노드들과 사이클을 형성해서는 안된다.
4. 3번 과정을 반복하다가 최소 신장 트리가 그래프의 모든 정점을 연결하게 되면 알고리즘을 종료한다.

### 프림 알고리즘 예제
프림 알고리즘을 이용해 아래의 그래프를 최소 신장 트리로 만드는 예제는 다음과 같다.<br>
![프림알고리즘-예제1](/assets/img/posts/2021-09-17-graph-minimum-spanning-tree-3.png){: width="700" height="150" }<br>
우선 시작 정점을 정한다. 어느 것을 골라도 무관하지만 여기서는 B를 시작 정점으로 사용한다. B는 시작 정점으로 선정됨과 동시에 최소 신장 트리의 루트 노드가 된다.<br>
![프림알고리즘-예제2](/assets/img/posts/2021-09-17-graph-minimum-spanning-tree-4.png){: width="700" height="150" }<br>
정점 B에 연결된 간선은 B-A, B-C, B-F 인데 이중에서 가장 가중치가 작은 간선은 B-A 이므로 A를 최소 신장 트리에 추가한다.<br>
![프림알고리즘-예제3](/assets/img/posts/2021-09-17-graph-minimum-spanning-tree-5.png){: width="700" height="150" }<br>
최소 신장 트리에 등록된 노드는 B, A가 있다. 이 노드들에 연결된 간선들은 B-C, B-F, A-E 인데 이중에서 가장 가중치가 작은 간선은 B-C 이므로 C를 최소 신장 트리에 추가한다.<br>
C가 추가됨으로써 최소 신장 트리의 노드는 3개, 조사해야 할 간선은 아래 그림처럼 네개가 되었다.<br>
C-F 간선이 빠진 이유는 C-F 간선이 B-C-F를 통과하는 사이클을 형성하기 때문이다. 사이클이 형성되는 것을 막기 위해서 B-F 또는 C-F 둘 중 하나를 제외해야 하는데, 그중에서도 C-F를 제외한 이유는 B-F가 가진 가중치가 C-F의 것보다 더 작기 때문이다.<br>
![프림알고리즘-예제4](/assets/img/posts/2021-09-17-graph-minimum-spanning-tree-6.png){: width="700" height="150" }<br>
이번 단계에서는 C-D, B-F, C-G, A-E 이 네 간선을 조사한다. 가장 가중치가 작은 간선은 C-D 이므로 D를 최소 신장 트리에 추가한다.<br>
![프림알고리즘-예제5](/assets/img/posts/2021-09-17-graph-minimum-spanning-tree-7.png){: width="700" height="150" }<br>
이제 최소 가중치를 조사할 간선 B-F, C-G, A-E 중에 가중치가 가장 낮은 간선은 B-F이므로 F를 최소 신장 트리에 추가한다.<br>
![프림알고리즘-예제6](/assets/img/posts/2021-09-17-graph-minimum-spanning-tree-8.png){: width="700" height="150" }<br>
최소 신장 트리에 F가 추가됨으로써 A-E 간선은 가중치가 더 작은 F-E 간선에 의해, C-G 간선은 가중치가 더 작은 F-G 간선에 의해 조사 대상에서 제거된다. 그리고 F-H 간선이 조사 대상으로 추가된다.<br>
새로운 조사 대상 간선 셋 중 가장 작은 가중치를 갖는 간선은 F-E 이므로 E를 최소 신장 트리에 추가한다.<br>
![프림알고리즘-예제7](/assets/img/posts/2021-09-17-graph-minimum-spanning-tree-9.png){: width="700" height="150" }<br>
이번엔 F-H 간선이 E-H 간선에 의해 조사 대상에서 제거된다. 그러므로 이번 단계의 조사 대상 간선은 E-H, F-G 이다. 더 작은 가중치의 간선은 E-H 이므로 H를 최소 신장 트리에 추가한다.<br>
![프림알고리즘-예제8](/assets/img/posts/2021-09-17-graph-minimum-spanning-tree-10.png){: width="700" height="150" }<br>
이제 조사 대상 간선이 F-G 하나만 남는다. 경쟁할 후보가 없으므로 이 간선을 선택하고 G를 최소 신장 트리에 추가한다.<br>
![프림알고리즘-예제9](/assets/img/posts/2021-09-17-graph-minimum-spanning-tree-11.png){: width="700" height="150" }<br>
이번에도 조사 대상 간선이 G-I 하나만 남는다. 경쟁할 후보가 없으므로 이 간선을 선택하고 I를 최소 신장 트리에 추가한다.<br>
이제 모든 정점들이 최소 신장 트리에 입력되었으므로 트리 구축을 종료한다.<br>
![프림알고리즘-예제10](/assets/img/posts/2021-09-17-graph-minimum-spanning-tree-12.png){: width="700" height="150" }<br>

<br>

### 프림 알고리즘 구현
프림 알고리즘을 구현하기 위해서는 고려할 사항이 2개가 있다.<br>
(1) 최소 신장 트리의 자료구조로 무엇을 사용할 것인가?<br>
후보 자료구조로는 배열, 링크드 리스트, 드리 등 여러 가지가 있지만 최소 신장 트리는 그래프이므로 이번 예제에서는 그래프를 이용한다.<br>
(2) 조사 대상 간선에서 최소 가중치를 가진 정점을 골라내는 과정에서 발생하는 성능 문제<br>
최소 신장 트리에 정점이 하나 추가될 때마다 그 수가 늘어나거나 줄어드는 조사 대상 간선 집합 속에서 '최소 가중치'를 가진 간선을 찾아내야 한다.<br>
그래프에 정점이 N개가 존재한다고 하면 최소 신장 트리에 정점을 추가하는 작업을 N번 해야하고, 정점을 추가할 때마다 그래프 내의 정점 N개를 순회해야 하므로 N²회 반복을 돌아야 한다.<br>
이 문제를 해결하기 위해서 삽입과 삭제가 빠르고 최소 값을 찾는 데에 거의 비용이 들지 않는 우선순위 큐를 사용하는 것이 적합하다.<br>

<br>

---
## 크루스칼 알고리즘(Kruskal's Algorithm)
### 크루스칼 알고리즘이란?
프림 알고리즘이 최소 신장 트리에 연결된 정점들의 주변 정보를 파악하며 최소 신장 트리를 완성해가는 반면 크루스칼 알고리즘은 그래프 내의 모든 간선들의 가중치 정보를 사전에 파악하고 이 정보를 토대로 최소 신장 트리를 구축해나가는 것이 특징<br>
프림 알고리즘을 통해 최소 신장 트리를 만들어 내는 과정<br>
1. 그래프 내의 모든 간선을 가중치의 오름차순으로 목록으로 만든다.
2. 1번에서 만든 간선의 목록을 차례대로 순환하면서 간선을 최소 신장 트리에 추가한다. 단, 이때 추가된 간선으로 인해 최소 신장 트리 내에 사이클이 형성되면 안 된다.

<br>

### 분리 집합
크루스칼 알고리즘에서는 구현을 위해 '최소 신장 트리에 생기는 사이클을 어떻게 효율적으로 감지할지에 대한 문제'를 해결해야 한다.<br>
깊이 우선 탐색을 사용하여 이미 방문했던 노드를 또 만난다면 사이클이 있다는 뜻이다. 아래 이미지와 같은 방향성 그래프를 보면 A-B-D를 방문하고 A로 다시 돌아와서 C를 타고 내려가면 이미 방문한 D를 만나게 된다. 그러므로 그래프에 사이클이 존재함을 알 수 있다. 이 그래프가 무방향성 그래프라면 깊이 우선 탐색을 통해 A-B-C-D-A 순서로 순회하여 이미 방문한 A를 만남으로써 사이클이 존재함을 알 수 있다.<br>
![분리집합1](/assets/img/posts/2021-09-17-graph-minimum-spanning-tree-13.png){: width="700" height="150" }<br>
그런데 깊이 우선 탐색을 사용하면 탐색 비용이 크다는 단점이 있다. 이에 대한 대안으로 사용 가능한 사이클 탐지 방법은 분리 집합을 이용하는 방법이 있다.<br>
<br>
우선 정점들을 각각의 집합 안에 입력하고, 간선으로 연결되어 있는 정점들에 대해서는 합집합을 수행한다. 이때 간선으로 연결할 두 정점이 같은 집합에 속해 있다면 이 연결은 사이클을 이루게 된다.<br>
예를 들어 아래의 그림에서 정점 A와 D를 간선으로 연결한다고 할 때, 왼쪽 그림에서는 정점 A와 정점 D가 서로 별개의 집합에 속해 있기 때문에 사이클을 이루지 않지만, 오른쪽 그림에서는 두 정점이 같은 집합에 속해있기 때문에 둘을 잇게 되면 사이클이 생긴다.<br>

![분리집합2](/assets/img/posts/2021-09-17-graph-minimum-spanning-tree-14.png){: width="700" height="150" }<br>
<br>
분리 집합 소스코드
DisjointSet.h
```c
#include <stdio.h>
#include <stdlib.h>

#ifndef DISJOINTSET_H
#define DISJOINTSET_H

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
#include "Graph.h"

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
<br>

### 크루스칼 알고리즘 예제
프림 알고리즘 예제에서도 사용했던 그래프를 사용한다.<br>
![크루스칼알고리즘1](/assets/img/posts/2021-09-17-graph-minimum-spanning-tree-3.png){: width="700" height="150" }<br>
우선 그래프 내의 정점 사이에 있는 모든 간선들을 가중치의 오름차순으로 정렬한다. 정렬된 간선의 목록은 다음과 같다.<br>
```
A - B : 35
E - F : 82
E - H : 98
G - I : 106
C - D : 117
F - H : 120
B - C : 126
B - F : 150
F - G : 154
C - F : 162
C - G : 220
A - E : 247
```
그리고 각 정점 별로 분리 집합을 만든다.<br>
![크루스칼알고리즘2](/assets/img/posts/2021-09-17-graph-minimum-spanning-tree-15.png){: width="700" height="150" }<br>
이제 가장 작은 가중치를 갖고 있는 간선(A-B)부터 작업을 시작한다. 정점 A, B는 별개의 분리 집합의 원소이므로 이 둘을 최소 신장 트리에 추가하고 간선으로 연결한다. 그리고 분리 집합 A, B에 대해 합집합을 수행하여 하나의 분리집합으로 만든다.<br>
![크루스칼알고리즘3](/assets/img/posts/2021-09-17-graph-minimum-spanning-tree-16.png){: width="700" height="150" }<br>
그 다음 간선 E-F의 양 종단 정점도 별개의 분리 집합에 속해 있으므로 E-F를 최소 신장 트리에 추가하고 분리 집합 E, F를 합친다.<br>
![크루스칼알고리즘4](/assets/img/posts/2021-09-17-graph-minimum-spanning-tree-17.png){: width="700" height="150" }<br>
그 다음 간선 E-H에서 E는 분리 집합 [E, F]에 속해있고 H는 \[H]에 속해 있으므로 분리 집합 [E, F]와 \[H]에 대해 합집합을 수행한다.<br>
![크루스칼알고리즘5](/assets/img/posts/2021-09-17-graph-minimum-spanning-tree-18.png){: width="700" height="150" }<br>
다음 순서 G-I의 양 종단 정점은 별개의 분리 집합에 속해 있으므로 최소 신장 트리에 추가하고, 분리 집합 G, I를 합집합 연산을 수행한다.<br>
![크루스칼알고리즘6](/assets/img/posts/2021-09-17-graph-minimum-spanning-tree-19.png){: width="700" height="150" }<br>
다음은 간선 F-H이다. 정점 F, H는 이미 같은 집합에 속해 있으므로 이 간선이 추가된다면 사이클이 형성된다는 말이 된다. 따라서 간선 F-H는 제외하고 다음 순위의 간선을 채택한다.<br>
다음 순위의 간선은 C-D인데 이 둘은 서로 다른 집합에 속해 있으므로 최소 신장 트리에 추가하고, 이 두 정점을 하나의 집합으로 만든다.<br>
![크루스칼알고리즘7](/assets/img/posts/2021-09-17-graph-minimum-spanning-tree-20.png){: width="700" height="150" }<br>
다음은 간선 B-C이다. 정점 F, H는 다른 집합에 속해 있으므로 최소 신장 트리에 추가하고, 이 두 정점이 속한 집합에 대해 합집합 연산을 한다.<br>
![크루스칼알고리즘8](/assets/img/posts/2021-09-17-graph-minimum-spanning-tree-21.png){: width="700" height="150" }<br>
다음 순위의 간선은 B-F이다. 정점 B, F는 다른 집합에 속해 있으므로 최소 신장 트리에 추가하고, 이 두 정점이 속한 집합에 대해 합집합 연산을 한다.<br>
![-크루스칼알고리즘9](/assets/img/posts/2021-09-17-graph-minimum-spanning-tree-22.png){: width="700" height="150" }<br>
다음 순위의 간선은 F-G이다. 정점 F, G는 다른 집합에 속해 있으므로 최소 신장 트리에 추가하고, 이 두 정점이 속한 집합에 대해 합집합 연산을 한다.<br>
이제 모든 정점들이 하나의 집합 안에 들어갔으므로 최소 신장 트리가 완성되었다.
![크루스칼알고리즘10](/assets/img/posts/2021-09-17-graph-minimum-spanning-tree-23.png){: width="700" height="150" }<br>
<br>

---
## 최소 신장 트리 예제 프로그램
다음 예제 프로그램에서는 프림 알고리즘과 크루스칼 알고리즘이 모두 포함되어 있기 때문에 파일이 총 9개가 된다.<br>
(DisjointSet.c, DisjointSet.h, PriorityQueue.c, PriorityQueue.h, Graph.c, Graph.h, MST.c, MST.h, Test_MST.c)
DisjointSet.h, DisjointSet.c는 위의 [분리 집합 소스코드](https://garamm.github.io/현재파일명/#위치)를 사용하고,<br>
PriorityQueue.h, PriorityQueue.c는 [우선순위 큐 예제 프로그램](https://garamm.github.io/파일명쓰기){:target="_blank"}를 사용하고,<br>
Graph.h, Graph.c는 [인접 리스트 예제 프로그램](https://garamm.github.io/파일명쓰기){:target="_blank"}를 사용하면 된다.<br>
<br>
MST.h
```c
#ifndef MST_H
#define MST_H

#include "Graph.h"
#include "PriorityQueue.h"
#include "DisjointSet.h"

#define MAX_WEIGHT 36267

void Prim(Graph* G, Vertex* StartVertex, Graph* MST );
void Kruskal(Graph* G, Graph* MST );

#endif
```
MST.c
```c
#include "MST.h"
/* G: 원본 그래프, StartVertex: 그래프의 시작 정점, MST: 최소 신장 트리가 될 그래프 */
void Prim(Graph* G, Vertex* StartVertex, Graph* MST )
{
    int i = 0;

    PQNode         StartNode = { 0, StartVertex };
    PriorityQueue* PQ        = PQ_Create(10);

    Vertex*  CurrentVertex = NULL;
    Edge*    CurrentEdge   = NULL; 

    int*     Weights       = (int*) malloc( sizeof(int) * G->VertexCount );
    Vertex** MSTVertices   = (Vertex**) malloc( sizeof(Vertex*) * G->VertexCount );
    Vertex** Fringes       = (Vertex**) malloc( sizeof(Vertex*) * G->VertexCount );
    Vertex** Precedences   = (Vertex**) malloc( sizeof(Vertex*) * G->VertexCount );

    CurrentVertex = G->Vertices;
    while ( CurrentVertex != NULL )
    {
        Vertex* NewVertex = CreateVertex( CurrentVertex->Data );
        AddVertex( MST, NewVertex);

        Fringes[i]     = NULL;
        Precedences[i] = NULL;
        MSTVertices[i] = NewVertex;        
        Weights[i]     = MAX_WEIGHT;        
        CurrentVertex  = CurrentVertex->Next;
        i++;
    }

    PQ_Enqueue ( PQ, StartNode );

    Weights[StartVertex->Index] = 0;
    
    while( ! PQ_IsEmpty( PQ ) )
    {
        PQNode  Popped;
        
        PQ_Dequeue(PQ, &Popped);
        CurrentVertex = (Vertex*)Popped.Data;
        
        Fringes[CurrentVertex->Index] = CurrentVertex;

        CurrentEdge = CurrentVertex->AdjacencyList;
        while ( CurrentEdge != NULL )
        {            
            Vertex* TargetVertex = CurrentEdge->Target;

            if ( Fringes[TargetVertex->Index] == NULL &&
                CurrentEdge->Weight < Weights[TargetVertex->Index] )
            {
                PQNode NewNode =  { CurrentEdge->Weight, TargetVertex };
                PQ_Enqueue ( PQ, NewNode );

                Precedences[TargetVertex->Index] = CurrentEdge->From;
                Weights[TargetVertex->Index]     = CurrentEdge->Weight;                
            }
            
            CurrentEdge = CurrentEdge->Next;
        }
    }

    for ( i=0; i<G->VertexCount; i++ )
    {
        int FromIndex, ToIndex;

        if ( Precedences[i] == NULL )
            continue;

        FromIndex = Precedences[i]->Index;
        ToIndex   = i;

        AddEdge( MSTVertices[FromIndex], 
            CreateEdge( MSTVertices[FromIndex], MSTVertices[ToIndex],   Weights[i] ) );

        AddEdge( MSTVertices[ToIndex],   
            CreateEdge( MSTVertices[ToIndex],   MSTVertices[FromIndex], Weights[i] ) );
    }

    free( Fringes );
    free( Precedences );
    free( MSTVertices );
    free( Weights );

    PQ_Destroy( PQ );
}

void Kruskal(Graph* G, Graph* MST )
{
    int           i;
    Vertex*       CurrentVertex = NULL;
    Vertex**      MSTVertices   = (Vertex**) malloc( sizeof(Vertex*) * G->VertexCount );
    DisjointSet** VertexSet     = 
                         (DisjointSet**)malloc( sizeof(DisjointSet*) * G->VertexCount );
    
    PriorityQueue* PQ      = PQ_Create(10);

    i = 0;    
    CurrentVertex = G->Vertices;
    while ( CurrentVertex != NULL )
    {
        Edge* CurrentEdge;

        VertexSet[i]   = DS_MakeSet( CurrentVertex );
        MSTVertices[i] = CreateVertex( CurrentVertex->Data );
        AddVertex( MST, MSTVertices[i] );

        CurrentEdge = CurrentVertex->AdjacencyList;
        while ( CurrentEdge != NULL )
        {
            PQNode NewNode = { CurrentEdge->Weight, CurrentEdge };
            PQ_Enqueue( PQ, NewNode );

            CurrentEdge = CurrentEdge->Next;
        }

        CurrentVertex  = CurrentVertex->Next;        
        i++;
    }

    while ( !PQ_IsEmpty( PQ ) )
    {
        Edge*  CurrentEdge;
        int    FromIndex;
        int    ToIndex;
        PQNode Popped;

        PQ_Dequeue( PQ, &Popped );
        CurrentEdge = (Edge*)Popped.Data;

        printf("%c - %c : %d\n", 
            CurrentEdge->From->Data, 
            CurrentEdge->Target->Data, 
            CurrentEdge->Weight );

        FromIndex = CurrentEdge->From->Index;
        ToIndex   = CurrentEdge->Target->Index;

        if ( DS_FindSet( VertexSet[FromIndex] ) != DS_FindSet( VertexSet[ToIndex] ) )
        {
            AddEdge( MSTVertices[FromIndex], 
                     CreateEdge( MSTVertices[FromIndex], 
                                 MSTVertices[ToIndex], 
                                 CurrentEdge->Weight ) );

            AddEdge( MSTVertices[ToIndex], 
                     CreateEdge( MSTVertices[ToIndex], 
                                 MSTVertices[FromIndex], 
                                 CurrentEdge->Weight ) );

            DS_UnionSet( VertexSet[FromIndex], VertexSet[ToIndex] );
        }
    }

    for ( i=0; i<G->VertexCount; i++ )
        DS_DestroySet( VertexSet[i] );

    free( VertexSet );
    free( MSTVertices );
}
```
Test_MST.c
```c
#include "Graph.h"
#include "MST.h"

int main( void )
{
    
    /*  그래프 생성     */
    Graph* graph      = CreateGraph();
    Graph* PrimMST    = CreateGraph();
    Graph* KruskalMST = CreateGraph();

    /*  정점 생성 */
    Vertex* A = CreateVertex( 'A' );
    Vertex* B = CreateVertex( 'B' );
    Vertex* C = CreateVertex( 'C' );
    Vertex* D = CreateVertex( 'D' );
    Vertex* E = CreateVertex( 'E' );
    Vertex* F = CreateVertex( 'F' );
    Vertex* G = CreateVertex( 'G' );
    Vertex* H = CreateVertex( 'H' );
    Vertex* I = CreateVertex( 'I' );

    /*  그래프에 정점을 추가 */
    AddVertex( graph, A );
    AddVertex( graph, B );
    AddVertex( graph, C );
    AddVertex( graph, D );
    AddVertex( graph, E );
    AddVertex( graph, F );
    AddVertex( graph, G );
    AddVertex( graph, H );
    AddVertex( graph, I );

    /*  정점과 정점을 간선으로 잇기 */
    AddEdge( A, CreateEdge(A, B, 35) );
    AddEdge( A, CreateEdge(A, E, 247) );
    
    AddEdge( B, CreateEdge(B, A, 35  ) );
    AddEdge( B, CreateEdge(B, C, 126 ) );
    AddEdge( B, CreateEdge(B, F, 150 ) );

    AddEdge( C, CreateEdge(C, B, 126 ) );
    AddEdge( C, CreateEdge(C, D, 117 ) );
    AddEdge( C, CreateEdge(C, F, 162 ) );
    AddEdge( C, CreateEdge(C, G, 220 ) );
    
    AddEdge( D, CreateEdge(D, C, 117 ) );

    AddEdge( E, CreateEdge(E, A, 247 ) );
    AddEdge( E, CreateEdge(E, F, 82 ) );
    AddEdge( E, CreateEdge(E, H, 98 ) );

    AddEdge( F, CreateEdge(F, B, 150 ) );
    AddEdge( F, CreateEdge(F, C, 162 ) );
    AddEdge( F, CreateEdge(F, E, 82  ) );
    AddEdge( F, CreateEdge(F, G, 154 ) );
    AddEdge( F, CreateEdge(F, H, 120 ) );

    AddEdge( G, CreateEdge(G, C, 220 ) );
    AddEdge( G, CreateEdge(G, F, 154 ) );
    AddEdge( G, CreateEdge(G, I, 106 ) );

    AddEdge( H, CreateEdge(H, E, 98  ) );
    AddEdge( H, CreateEdge(H, F, 120 ) );

    AddEdge( I, CreateEdge(I, G, 106 ) );
    
    /*  정점 B를 시작 정점으로 하는 최소 신장 트리. */
    printf("Prim's Algorithm\n");
    Prim(graph, B, PrimMST);
    PrintGraph ( PrimMST );

    printf("Kruskal's Algorithm...\n");
    Kruskal(graph, KruskalMST);
    PrintGraph ( KruskalMST );
    
    /*  그래프 소멸 */
    DestroyGraph( PrimMST );
    DestroyGraph( KruskalMST );
    DestroyGraph( graph );

    return 0;
}
```
실행결과
```
Kruskal's Algorithm...
A - B : 35
B - A : 35
F - E : 82
E - F : 82
H - E : 98
E - H : 98
G - I : 106
I - G : 106
D - C : 117
C - D : 117
F - H : 120
H - F : 120
B - C : 126
C - B : 126
B - F : 150
F - B : 150
F - G : 154
G - F : 154
C - F : 162
F - C : 162
G - C : 220
C - G : 220
A - E : 247
E - A : 247
A : B[35]
B : A[35] C[126] F[150]
C : D[117] B[126]
D : C[117]
E : F[82] H[98]
F : E[82] B[150] G[154]
G : I[106] F[154]
H : E[98]
I : G[106]
```

<br>

\# 참고<br>
[박상현 / 뇌를 자극하는 알고리즘](https://www.hanbit.co.kr/media/books/book_view.html?p_code=B3450156021){:target="_blank"}<br>



