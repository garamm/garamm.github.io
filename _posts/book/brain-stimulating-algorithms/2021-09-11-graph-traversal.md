---
layout: post
title: "[책/뇌를 자극하는 알고리즘] 9-2. 그래프 / 깊이우선탐색(DFS), 너비우선탐색(BFS)"
author: 임가람
date: "2021-09-11 00:38:00"
categories: [책, 뇌를 자극하는 알고리즘]
tags: [뇌를 자극하는 알고리즘]
---

## 그래프 순회
- 깊이 우선 탐색: 그래프를 정렬하는 알고리즘에 사용
- 너비 우선 탐색: 그래프 최단 경로를 찾거나 미로 찾기를 푸는 알고리즘에 사용

<br>

---
## 깊이 우선 탐색(Depth First Search)
길이 나오지 않을 때까지 그래프의 정점을 타고 깊이 들어가다가 더 이상 방문해왔던 정점 말고는 다른 이웃을 갖고 있지 않은 정점을 만나면 뒤로 돌아와 다른 경로로 뻗어 있는 정점을 타고 방문을 재개하는 방식으로 동작
- (1) 시작 정점을 밟은 후 이 정점을 '방문했음'으로 표시한다.
- (2) 이 정점과 이웃하고 있는 정점(인접 정점) 중에 아직 방문하지 않은 곳을 선택하여 이를 시작 정점으로 삼아 다시 1번 단계를 수행한다.
- 정점에 더 이상 방문하지 않은 인접 정점이 없으면 이전 정점으로 돌아가서 단게 2를 수행한다.
- 이전 정점으로 돌아가도 더 이상 방문할 이웃이 없다면 그래프의 모든 정점을 모두 방문했다는 뜻이므로 탐색을 종료한다.

*** 예시 ***<br>
![DFSBFS-예시1](/assets/img/posts/2021-09-11-graph-traversal-1.png){: width="700" height="150" }<br>
먼저, 시작 정점 1을 밟은 후 이 정점을 '방문했음'으로 표시한다.<br>
![DFSBFS-예시2](/assets/img/posts/2021-09-11-graph-traversal-2.png){: width="700" height="150" }<br>
현재 정점의 인접 정점 중의 하나를 선택해 그곳으로 이동한다. 정점 1의 인접 정점은 2와 3이 있는데 정점 2를 먼저 방문하여 정점 2를 '방문했음'으로 표시한다.<br>
![DFSBFS-예시3](/assets/img/posts/2021-09-11-graph-traversal-3.png){: width="700" height="150" }<br>
정점 2의 인접 정점은 4와 5가 있는데 그 중 정점 4를 먼저 방문하여 이 정점을 '방문했음'으로 표시한다.<br>
![DFSBFS-예시4](/assets/img/posts/2021-09-11-graph-traversal-4.png){: width="700" height="150" }<br>
정점 4의 인접 정점도 같은 원리로 방문을 계속해 나간다. 5를 방문하고, 7을 방문한다.<br>
![DFSBFS-예시5](/assets/img/posts/2021-09-11-graph-traversal-5.png){: width="700" height="150" }<br>
정점 7은 더 이상 방문할 인접 정점이 없다. 그러므로 이제 다시 정점 5로 돌아가서 방문하지 않은 인접 정점이 있는지 확인한다. 없으므로 정점 4, 정점 2로 돌아간다. 정점 2도 이웃 정점이 없으니 1로 되돌아간다. 정점 1에는 방문하지 않은 정점 3이 있으므로 정점 3을 방문한다.<br>
![DFSBFS-예시6](/assets/img/posts/2021-09-11-graph-traversal-6.png){: width="700" height="150" }<br>
정점 3에는 인접 정점 4와 6이 있지만 4는 이미 방문했으므로 6을 방문한다. 정점 6에는 새로 방문할 정점이 없으니 다시 뒤로 돌아간다. 정점 3, 정점 1로 돌아가도 새로 방문할 정점이 없으므로 탐색을 종료한다.<br>
![DFSBFS-예시7](/assets/img/posts/2021-09-11-graph-traversal-7.png){: width="700" height="150" }<br>

<br>

---
## 너비 우선 탐색(Breadth First Search)
시작 정점을 지나고 나면 깊이가 1인 모든 정점을 방문하고 그 다음에는 깊이가 2인 모든 정점을 방문한다. 이런 식으로 한 단계씩 깊이를 더해가다가 방문할 곳이 없을 때 탐색을 종료한다.<br>
- (1) 시작 정점을 '방문했음'으로 표시하고 큐에 삽입(Enqueue)한다.
- (2) 큐로부터 정점을 제거(Dequeue)한다. 제거한 정점이 이웃하고 있는 인접 정점 중 아직 방문하지 않은 곳들을 '방문했음'으로 표시하고 큐에 삽입한다.
- (3) 큐가 비게 되면 탐색이 끝난다. 따라서 큐가 빌 때까지 2번 과정을 반복한다.

*** 예시 ***<br>
![DFSBFS-DFS1](/assets/img/posts/2021-09-11-graph-traversal-8.png){: width="700" height="150" }<br>
먼저 시작 정점을 '방문했음'으로 표시하고 큐에 삽입한다.<br>
![DFSBFS-DFS2](/assets/img/posts/2021-09-11-graph-traversal-9.png){: width="700" height="150" }<br>
이제 큐가 빌 때까지 2번 과정을 반복한다. 현재 큐에는 시작 정점 1이 있는데, 이를 큐에서 제거한 다음 인접 정점이 있는지 확인한다.<br>
1에는 아직 방문하지 않은 정점 2와 3이 있으니 두 정점을 방문하고 큐에 삽입한다.<br>
![DFSBFS-DFS3](/assets/img/posts/2021-09-11-graph-traversal-10.png){: width="700" height="150" }<br>
이제 큐에는 두 개의 정점이 들어있는데 우선 전단에 있는 정점을 제거한 후 이 정점의 인접 정점을 조사한다.<br>
전단에 있는 정점 2에는 아직 방문하지 않은 정점 4, 5가 인접하고 있으므로 이들을 방문한 후 큐에 삽입한다.<br>
![DFSBFS-DFS4](/assets/img/posts/2021-09-11-graph-traversal-11.png){: width="700" height="150" }<br>
큐의 전단에 있는 3을 제거하고 이 정점의 인접 정점을 조사한다.<br>
정점 3의 인접 정점은 4, 6인데 정점 4는 이미 방문했으므로 정점 6만 방문하고 큐에 삽입한다.<br>
![DFSBFS-DFS5](/assets/img/posts/2021-09-11-graph-traversal-12.png){: width="700" height="150" }<br>
큐의 전단에 있는 4를 제거하고 인접 정점을 조사한다.<br>
정점 4의 인접 정점은 5, 7인데 정점 5는 이미 방문했으므로 정점 7만 방문하고 큐에 삽입한다.<br>
![DFSBFS-DFS6](/assets/img/posts/2021-09-11-graph-traversal-13.png){: width="700" height="150" }<br>
큐에 남아있는 5, 6, 7 모두 인접 정점이 없으므로 탐색을 종료한다.<br>
<br>

---
## 그래프 순회 예제 프로그램
이 예제는 Graph.h, Graph.c, LinkedQueue.h, LinkedQueue.c, GraphTraversal.h, GraphTraversal.c, Test_GraphTraversal.c 총 7개의 파일로 구성되어 있다.<br>
Graph.h, Graph.c는 [인접 리스트 예제 프로그램](https://garamm.github.io/파일명쓰기){:target="_blank"}을 그대로 사용하고<br>
LinkedQueue.h, LinkedQueue.c는 [링크드 큐 예제 프로그램](https://garamm.github.io/파일명쓰기){:target="_blank"}에서 ElementType을 char* 에서 Vertex*로만 바꿔 사용한다.<br>
<br>
GraphTraversal.h
```c
#ifndef GRAPH_TRAVERSAL_H
#define GRAPH_TRAVERSAL_H

#include "Graph.h"
#include "LinkedQueue.h"

void DFS( Vertex* V );
void BFS( Vertex* V, LinkedQueue* Queue );

#endif
```
GraphTraversal.c
```c
#include "GraphTraversal.h"

void DFS( Vertex* V )
{
    Edge* E = NULL;

    printf("%d ", V->Data);

    V->Visited = Visited; /* 방문한 정점에 '방문했음' 이라고 표시 */

    E = V->AdjacencyList;

    while ( E != NULL ) /* 현재 정점의 모든 인접 정점에 대해 DFS()를 재귀적으로 호출 */
    {
        if ( E->Target != NULL && E->Target->Visited == NotVisited )
            DFS( E->Target ); /* 현재 정점을 시작으로 또 깊이 우선 탐색을 수행 */

        E = E->Next;
    }
}

void BFS( Vertex* V, LinkedQueue* Queue )
{
    Edge* E = NULL;

    printf("%d ", V->Data);
    V->Visited = Visited;
    
    
    LQ_Enqueue( &Queue, LQ_CreateNode( V ) ); /*  시작 정점을 큐에 삽입 */

    while ( !LQ_IsEmpty( Queue ) )              /* 큐가 비어 있는지 검사 */
    {
        Node* Popped = LQ_Dequeue( &Queue );    /* 큐에서 전단을 제거 */
        V = Popped->Data;
        E = V->AdjacencyList;

        while ( E != NULL ) /* 큐에서 꺼낸 정점의 인접 정점을 조사 */
        {
            V = E->Target;

            if ( V != NULL && V->Visited == NotVisited ) /* 미방문 정점만 방문*/
            {
                printf("%d ", V->Data);
                V->Visited = Visited;
                LQ_Enqueue( &Queue, LQ_CreateNode( V ) );
            }

            E = E->Next;
        }
    }
}
```
Test_GraphTraversal.c
```c
#include "Graph.h"
#include "GraphTraversal.h"

int main( void )
{
    int          Mode  = 0;
    Graph* graph = CreateGraph();
    
    Vertex* V1 = CreateVertex( 1 );
    Vertex* V2 = CreateVertex( 2 );
    Vertex* V3 = CreateVertex( 3 );
    Vertex* V4 = CreateVertex( 4 );
    Vertex* V5 = CreateVertex( 5 );
    Vertex* V6 = CreateVertex( 6 );
    Vertex* V7 = CreateVertex( 7 );

    AddVertex( graph, V1 );
    AddVertex( graph, V2 );
    AddVertex( graph, V3 );
    AddVertex( graph, V4 );
    AddVertex( graph, V5 );
    AddVertex( graph, V6 );
    AddVertex( graph, V7 );

    /*  정점과 정점을 간선으로 잇기 */
    AddEdge( V1, CreateEdge(V1, V2, 0) );
    AddEdge( V1, CreateEdge(V1, V3, 0) );

    AddEdge( V2, CreateEdge(V2, V4, 0) );
    AddEdge( V2, CreateEdge(V2, V5, 0) );

    AddEdge( V3, CreateEdge(V3, V4, 0) );
    AddEdge( V3, CreateEdge(V3, V6, 0) );

    AddEdge( V4, CreateEdge(V4, V5, 0) );
    AddEdge( V4, CreateEdge(V4, V7, 0) );

    AddEdge( V5, CreateEdge(V5, V7, 0) );

    AddEdge( V6, CreateEdge(V6, V7, 0) );

    printf( "Enter Traversal Mode (0:DFS, 1:BFS) : " );
    scanf(  "%d", &Mode );

    if ( Mode == 0  ) 
    {
        /*  깊이 우선 탐색 */
        DFS( graph->Vertices );
    }
    else
    {
        LinkedQueue* Queue = LQ_CreateQueue();
    
        /*  너비 우선 탐색 */
        BFS(V1, Queue);
        
        LQ_DestroyQueue( Queue );
    }

    DestroyGraph( graph );

    return 0;
}
```
실행결과
```
>GraphTraversal
Enter Traversal Mode (0:DFS, 1:BFS) : 0
1 2 4 5 7 3 6
>GraphTraversal
Enter Traversal Mode (0:DFS, 1:BFS) : 1
1 2 3 4 5 6 7
```

<br>

\# 참고<br>
[박상현 / 뇌를 자극하는 알고리즘](https://www.hanbit.co.kr/media/books/book_view.html?p_code=B3450156021){:target="_blank"}<br>