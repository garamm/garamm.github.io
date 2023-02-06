---
layout: post
title: "[책/뇌를 자극하는 알고리즘] 9-5. 그래프 / 최단 경로 탐색, 다익스트라 알고리즘"
author: 임가람
date: "2021-09-18 00:37:00"
categories: [책, 뇌를 자극하는 알고리즘]
tags: [뇌를 자극하는 알고리즘]
---

## 최단 경로 탐색
그래프 내의 한 정점에서 다른 정점으로 이동할 때 가중치 합이 최소값이 되게 만드는 경로를 찾는 알고리즘<br>
<br>

---
## 다익스트라 알고리즘(Dijkstra Algorithm)
### 다익스트라 알고리즘 이란?
동작 방식이 프림 알고리즘과 상당히 비슷하지만 프림 알고리즘이 간선의 길이를 이용해 어떤 간선을 먼저 연결할지를 결정하는데 반해, 다익스트라 알고리즘은 '경로의 길이'를 감안해서 간선을 연결한다.<br>
또한 다익스트라의 최단 경로 탐색 알고리즘은 사이클이 없는 방향성 그래프에 한해서만 사용할 수 있다.<br>
<br>
다익스트라 알고리즘의 동작 방식<br>

1. 각 정점 위에 시작점으로부터 자신에게 이르는 경로의 길이를 저장할 곳을 준비하고 모든 정점 위에 있는 경로의 길이를 $\infty$(무한대)로 초기화한다.
2. 시작 정점의 경로 길이를 0으로 초기화하고(시작 정점에서 시작 정점까지의 길이는 0이기 때문) 최단 경로에 추가한다.
3. 최단 경로에 새로 추가된 정점의 인접 정점들에 대해 경로 길이를 갱신하고 이들을 최단 경로에 추가한다. 만약 추가하려는 인접 정점이 이미 최단 경로 안에 존재한다면 갱신되기 이전의 경로 길이가 새로운 경로의 길이보다 더 큰 경우에 한해, 다른 선행 정점을 지나던 기존의 경로를 현재 정점을 경유하도록 수정한다.
4. 그래프 내의 모든 정점이 최단 경로에 소속될 때까지 3번 과정을 반복한다.
<br>

### 다익스트라 알고리즘 예제
![다익스트라 알고리즘 예제 1](/assets/img/posts/2021-09-18-graph-dijkstra-1.png){: width="700" height="150" }

경로는 시작점과 도착점을 갖는다. 이번 예제에서 시작점은 B로 설정하고 도착점은 '나머지의 모든 정점'으로 한다.<br>
우선 각 정점 별로 경로 길이를 정점 좌측 상단의 상자 안에 표시하고 초기 값을 $\infty$(무한대)로 설정한다. 그리고 시작 정점 B의 경로 길이는 0으로 바꾼다.
![다익스트라 알고리즘 예제 2](/assets/img/posts/2021-09-18-graph-dijkstra-2.png){: width="700" height="150" }

시작 정점인 B에 인접한 정점들을 찾고 간선의 가중치를 조사한다. 정점 A, C, F가 B에 인접한다. B-A를 거치는데 필요한 비용은 35이고 현재 경로 B-A간의 경로 길이인 $\infty$보다 작으므로 A의 경로 길이를 35로 바꾼다. 같은 방법으로 경로 B-C와 B-F도 바꾼다.
![다익스트라 알고리즘 예제 3](/assets/img/posts/2021-09-18-graph-dijkstra-3.png){: width="700" height="150" }

A의 유일한 인접 정점은 E이다. 한 정점 B에서 E로 갈 수 있는 길은 B-A-E가 유일하다. B-A 비용 35와 A-E의 비용 247이 든다. 따라서 35와 247을 더한 282를 경로 길이에 입력한다. 282는 현재 경로 길이인 $\infty$보다 작으므로 입력이 가능하다.<br>
![다익스트라 알고리즘 예제 4](/assets/img/posts/2021-09-18-graph-dijkstra-4.png){: width="700" height="150" }

이번에는 C의 인접 정점을 조사한다. C의 인접 정점은 D, F, G가 있는데 현재 D의 경로 길이는 $\infty$이므로 B-C-D 경로의 비용을 그대로 입력하고 G 역시 경로 길이가 $\infty$이므로 B-C-G의 경로 비용을 입력한다.<br>
정점 F가 갖고 있는 경로 길이를 150으로 B-C-F 경로 비용인 288보다 작다. 따라서 새로 발견한 경로 B-C-F는 B-F보다 비용이 크기 때문에 채택하지 않는다.<br>
![다익스트라 알고리즘 예제 5](/assets/img/posts/2021-09-18-graph-dijkstra-5.png){: width="700" height="150" }

이번에는 정점 F에 인접한 정점을 확인한다. F의 인접 정점은 E, G, H 이다.<br>
E 정점은 기존 경로 길이 282를 갖고 있는데 새로운 경로인 B-F-E의 비용이 232로 더 작다. 따라서 기존 경로 B-A-E를 폐기하고 새로운 경로 B-F-E를 채택하고 경로 길이를 입력한다.<br>
G 정점은 기존 경로 길이 346를 갖고 있는데 새로운 경로인 B-F-G의 비용이 304로 더 작다. 따라서 기존 경로 B-C-G를 폐기하고 새로운 경로 B-F-G를 채택하고 경로 길이를 입력한다.<br>
H의 경로 길이는 무한대이므로 경로 B-F-H의 비용 270을 H의 경로 길이로 입력한다.<br>
![다익스트라 알고리즘 예제 6](/assets/img/posts/2021-09-18-graph-dijkstra-6.png){: width="700" height="150" }

E에게 인접 정점 H가 있지만 B-F-E-H의 비용이 330이므로 현재 경로 길이 270보다 크므로 채택하지 않는다.<br>
이제 정점 G만 아직 방문하지 않은 정점 I를 갖고 있는데, I의 현재 경로 길이는 무한대이므로 경로 B-F-G-I의 비용 410을 입력한다.<br>
![다익스트라 알고리즘 예제 7](/assets/img/posts/2021-09-18-graph-dijkstra-7.png){: width="700" height="150" }

이제 더이상 탐사할 노드가 없으므로 최단 경로 탐색을 종료한다. 아래 그래프에서 각 정점들의 좌상단에 있는 상자 속의 값들은 모두 B에서부터 해당 정점에 이르는 경로상에 있는 모든 간선들의 가중치를 합한것이다.<br>
![다익스트라 알고리즘 예제 8](/assets/img/posts/2021-09-18-graph-dijkstra-8.png){: width="700" height="150" }

<br>

---
# 다익스트라 알고리즘 예제 프로그램
다음 예제는 총 7개의 파일로 구성되어 있다.<br>
Graph.c, Graph.h, PriorityQueue.c, PriorityQueue.h, ShortestPath.c, ShortestPath.h, Test_ShortestPath.c<br>
PriorityQueue.h, PriorityQueue.c는 [우선순위 큐 예제 프로그램](/posts/priority-queue/#힙을-이용한-우선순위-큐의-구현){:target="_blank"}를 사용하고,<br>
Graph.h, Graph.c는 [인접 리스트 예제 프로그램](/posts/graph-adjacency-matrix-list/#인접-리스트-예제-프로그램){:target="_blank"}를 사용하면 된다.<br>
<br>
ShortestPath.h
```c
#ifndef SHORTESTPATH_H
#define SHORTESTPATH_H

#include "Graph.h"
#include "PriorityQueue.h"

#define MAX_WEIGHT 36267

void Dijkstra(Graph* G, Vertex* StartVertex, Graph* MST );

#endif
```
ShortestPath.c
```c
#include "ShortestPath.h"

void Dijkstra(Graph* G, Vertex* StartVertex, Graph* ShortestPath )
{
    int i = 0;

    PQNode         StartNode = { 0, StartVertex };
    PriorityQueue* PQ        = PQ_Create(10);

    Vertex*  CurrentVertex = NULL;
    Edge*    CurrentEdge   = NULL; 

    int*     Weights       = (int*) malloc( sizeof(int) * G->VertexCount );
    Vertex** ShortestPathVertices = 
                             (Vertex**) malloc( sizeof(Vertex*) * G->VertexCount );
    Vertex** Fringes       = (Vertex**) malloc( sizeof(Vertex*) * G->VertexCount );
    Vertex** Precedences   = (Vertex**) malloc( sizeof(Vertex*) * G->VertexCount );

    CurrentVertex = G->Vertices;
    while ( CurrentVertex != NULL )
    {
        Vertex* NewVertex = CreateVertex( CurrentVertex->Data );
        AddVertex( ShortestPath, NewVertex);

        Fringes[i]     = NULL;
        Precedences[i] = NULL;
        ShortestPathVertices[i] = NewVertex;        
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
                Weights[CurrentVertex->Index] + CurrentEdge->Weight < 
                    Weights[TargetVertex->Index] )
            {
                PQNode NewNode =  { CurrentEdge->Weight, TargetVertex };
                PQ_Enqueue ( PQ, NewNode );

                Precedences[TargetVertex->Index] =  CurrentEdge->From;
                Weights[TargetVertex->Index]     =  
                    Weights[CurrentVertex->Index] + CurrentEdge->Weight; 
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

        AddEdge( ShortestPathVertices[FromIndex], 
            CreateEdge( 
                ShortestPathVertices[FromIndex], 
                ShortestPathVertices[ToIndex],   
                Weights[i] ) );
    }

    free( Fringes );
    free( Precedences );
    free( ShortestPathVertices );
    free( Weights );

    PQ_Destroy( PQ );
}
```
Test_ShortestPath.c
```c
#include "Graph.h"
#include "ShortestPath.h"

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
    
    AddEdge( A, CreateEdge(A, E, 247) );

    AddEdge( B, CreateEdge(B, A, 35  ) );
    AddEdge( B, CreateEdge(B, C, 126 ) );
    AddEdge( B, CreateEdge(B, F, 150 ) );
    
    AddEdge( C, CreateEdge(C, D, 117 ) );
    AddEdge( C, CreateEdge(C, F, 162 ) );
    AddEdge( C, CreateEdge(C, G, 220 ) );
    
    AddEdge( E, CreateEdge(E, H, 98 ) );

    AddEdge( F, CreateEdge(F, E, 82  ) );
    AddEdge( F, CreateEdge(F, G, 154 ) );
    AddEdge( F, CreateEdge(F, H, 120 ) );

    AddEdge( G, CreateEdge(G, I, 106 ) );

    /*  정점 B를 시작 정점으로 하는 최소 신장 트리. */
    Dijkstra(graph, B, PrimMST);
    PrintGraph ( PrimMST );
    
    /*  그래프 소멸 */
    DestroyGraph( PrimMST );
    DestroyGraph( KruskalMST );
    DestroyGraph( graph );

    return 0;
}
```
실행결과
```
A : 
B : A[35] C[126] F[150]
C : D[243]
D : 
E : 
F : E[232] G[304] H[270]
G : I[410]
H : 
I : 
```

<br>

\# 참고<br>
[박상현 / 뇌를 자극하는 알고리즘](https://www.hanbit.co.kr/media/books/book_view.html?p_code=B3450156021){:target="_blank"}<br>



