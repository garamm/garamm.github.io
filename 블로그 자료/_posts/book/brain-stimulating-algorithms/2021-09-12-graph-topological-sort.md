---
layout: post
title: "[책/뇌를 자극하는 알고리즘] 9-3. 그래프 / 위상 정렬"
author: 임가람
date: "2021-09-12 15:09:00"
categories: [책, 뇌를 자극하는 알고리즘]
tags: [뇌를 자극하는 알고리즘]
---

## 위상 정렬
- 위상
  - 어떤 사물이 다른 사물과의 관계 속에서 가지는 위치나 상태
- 위상 정렬
  - 간선의 방향에 따라 간선이 뻗어나오는 정점이 앞이 되고, 간선이 들어가는 정점이 뒤가 된다. 이러한 앞/뒤 관계를 정렬하는 것
  - 위상 정렬은 앞/뒤 관계가 있어야 하므로 방향성이 있어야 한다.
  - 사이클이 있으면 시작과 끝을 정의내릴 수 없기 떄문에 사이클이 없어야 한다.
  - 이러한 그래프를 DAG(Directed Acyclic Graph)라고 부른다.

## 위상 정렬의 예시
DAG를 활용하여 위상 정렬을 해본다!<br>
![위상정렬-예시1](/assets/img/posts/2021-09-12-graph-topological-sort-1.png){: width="700" height="150" }<br>

정점은 두 가지 종류의 방향성을 가질 수 있는데 정점으로 들어가는 간선을 진입 간선(Incomming Edge), 그 다른 하나는 진출 간선(Outgoing Edge)라고 한다.<br>
![위상정렬-예시2](/assets/img/posts/2021-09-12-graph-topological-sort-2.png){: width="700" height="150" }<br>

위상 정렬은 다음과 같은 과정으로 진행된다.<br>
1. 리스트를 하나 준비한다.
2. 그래프에서 진입 간선이 없는 정점을 리스트에 추가하고, 해당 정점 자신과 진출 간선을 제거한다.
3. 모든 정점에 대해 2번을 반복하고, 그래프 내에 정점이 남아 있지 않으면 정렬을 종료한다. 이때 리스트에는 위상 정렬된 그래프가 저장된다.

위의 DAG 그래프를 활용하여 위상정렬 진행해보면<br>
1번에 따라 리스트를 하나 준비하고 2번에 따라 진입 간선이 없는 정점 A, B중 하나를 골라 시작한다.<br>
이번 예시에서는 B를 골라 리스트에 추가하고 B와 진출 간선을 모두 제거한다.<br>
![위상정렬-예시3](/assets/img/posts/2021-09-12-graph-topological-sort-3.png){: width="700" height="150" }<br>
B를 제거하고 진입 간선이 없는 정점을 찾는다. 정점 A와 E가 진입 간선이 없으므로 둘 중 하나를 E를 골라 리스트에 추가하고 E와 진출 간선을 제거한다.<br>
![위상정렬-예시4](/assets/img/posts/2021-09-12-graph-topological-sort-4.png){: width="700" height="150" }<br>
진입 간선이 없는 정점이 A 하나 뿐이므로 A를 리스트에 추가하고 정점 A와 진출 간선을 모두 제거한다.<br>
![위상정렬-예시5](/assets/img/posts/2021-09-12-graph-topological-sort-5.png){: width="700" height="150" }<br>
이번에는 C와 D중 D를 골라 리스트에 추가하고 정점과 진출 간선을 모두 제거한다.<br>
![위상정렬-예시6](/assets/img/posts/2021-09-12-graph-topological-sort-6.png){: width="700" height="150" }<br>
진입 간선이 없는 C와 G 중에서 G를 리스트에 추가하고 G와 진출 간선을 모두 제거한다.<br>
![위상정렬-예시7](/assets/img/posts/2021-09-12-graph-topological-sort-7.png){: width="700" height="150" }<br>
진입 간선이 없는 C를 리스트에 추가하고 C와 진출 간선을 모두 제거한다.<br>
![위상정렬-예시8](/assets/img/posts/2021-09-12-graph-topological-sort-8.png){: width="700" height="150" }<br>
진입 간선이 없는 F를 리스트에 추가하고 F와 진출 간선을 모두 제거한다.<br>
![위상정렬-예시9](/assets/img/posts/2021-09-12-graph-topological-sort-9.png){: width="700" height="150" }<br>
마지막으로 남은 정점 H를 리스트에 추가하면 위상 정렬이 종료된다. 완성된 리스트는 위상 정렬한 결과이다.<br>
![위상정렬-예시10](/assets/img/posts/2021-09-12-graph-topological-sort-10.png){: width="700" height="150" }<br>

<br>

---
## 깊이 우선 탐색을 이용한 위상 정렬
위상 정렬은 깊이 우선 탐색을 이용할 수 있다. 깊이 우선 탐색을 이용한 위상 정렬 알고리즘은 다음과 같다.<br>
- (1) 리스트를 하나 준비한다.
- (2) 그래프에서 진입 간선이 없는 정점에 대해 깊이 우선 탐색을 시행하고, 탐색 중에 더 이상 옮겨갈 수 있는 인접 정점이 없는 정점을 만나면 이 정점을 리스트의 새로운 '헤드'로 입력한다.
- (3) 2번을 반복하다가 더 이상 방문할 정점이 없으면 깊이 우선 탐색을 종료한다. 깊이 우선 탐색이 끝난 후 리스트에는 위상 정렬된 그래프가 남는다.

### 깊이 우선 탐색을 이용한 위상 정렬의 예시
DAG를 활용하여 위상 정렬을 해본다!<br>
![위상정렬-예시11](/assets/img/posts/2021-09-12-graph-topological-sort-1.png){: width="700" height="150" }<br>
리스트를 준비한 다음, 진입 간선이 없는 정점을 찾아 깊이 우선 탐색을 시작한다. 진입 간선이 없는 정점 A, B중 A를 선택하여 시작한다.<br>
A - C - F - H를 타고 그래프 안쪽으로 탐색을 하면 H에서는 더이상 옮겨갈 수 있는 인접 정점이 없기 때문에 H를 리스트의 '헤드'로 삽입한다.<br>
![위상정렬-예시12](/assets/img/posts/2021-09-12-graph-topological-sort-11.png){: width="700" height="150" }<br>
H에서 뒤로 돌아오면 F를 만난다. F는 유일하게 H를 인접 정점으로 가지고 있었는데 H는 이미 방문했으니 더 이상 방문할 정점이 없다. 그러므로 F를 리스트의 헤드로 삽입하고 뒤로 돌아간다.<br>
![위상정렬-예시13](/assets/img/posts/2021-09-12-graph-topological-sort-12.png){: width="700" height="150" }<br>
정점 C는 유일한 인접 정점이 F뿐이었는데 이미 방문했으니 C를 리스트의 헤드로 삽입하고 뒤로 돌아간다.<br>
![위상정렬-예시14](/assets/img/posts/2021-09-12-graph-topological-sort-13.png){: width="700" height="150" }<br>
정점 A는 인접 정점 C와 D를 가지고 있는데 C는 이미 방문했으니 D를 타고 그래프의 깊숙이 들어간다.<br>
D를 거쳐 G에 도달하면 G의 유일한 인접 정점이었던 H가 이미 방문 상태이므로 G는 방문할 수 있는 인접 정점이 없기 떄문에 마찬가지로 리스트의 헤드로 추가하고 뒤로 돌아간다.<br>
![위상정렬-예시15](/assets/img/posts/2021-09-12-graph-topological-sort-14.png){: width="700" height="150" }<br>
D의 인접 정점 F, G도 이미 방문했기 때문에 D를 리스트의 헤드로 추가하고 뒤로 돌아간다.<br>
![위상정렬-예시16](/assets/img/posts/2021-09-12-graph-topological-sort-15.png){: width="700" height="150" }<br>
A도 더이상 방문할 인접 정점이 없기 떄문에 리스트의 헤드에 삽입한다.<br>
![위상정렬-예시17](/assets/img/posts/2021-09-12-graph-topological-sort-16.png){: width="700" height="150" }<br>
이제 B의 인접 정점 C와 E중 C는 이미 방문했기 때문에 E를 방문한다. E의 유일한 인접정점인 G가 이미 방문 상태이므로 E를 리스트의 헤드로 추가하고 뒤로 돌아간다.<br>
![위상정렬-예시18](/assets/img/posts/2021-09-12-graph-topological-sort-17.png){: width="700" height="150" }<br>
B는 더 이상 방문할 수 있는 인접 정점이 없으므로 리스트의 헤드로 삽입한다. 깊이 우선 탐색은 종료되었고, 리스트가 그래프의 위상 정렬 결과이다.<br>
![위상정렬-예시19](/assets/img/posts/2021-09-12-graph-topological-sort-18.png){: width="700" height="150" }<br>

<br>

---
## 위상 정렬 예제 프로그램
아래 예제 프로그램에서는 Graph.h, Graph.c, LinkedList.h, LinkedList.c, TopologicalSort.h, TopologicalSort.c, Test_TopologicalSort.c 총 7개의 파일로 구성되어 있다.<br>
Graph.h, Graph.c는 [인접 리스트 예제 프로그램](/posts/graph-adjacency-matrix-list/#인접-리스트-예제-프로그램){:target="_blank"}을 그대로 사용하고<br>
LinkedList.h, LinkedList.c는 [링크드 리스트 예제 프로그램](/posts/linkedlist/#예제-코드){:target="_blank"}에서 ElementType을 int* 에서 Vertex*로만 바꿔 사용한다.<br>
<br>
TopologicalSort.h
```c
#include "Graph.h"
#include "LinkedList.h"

#ifndef TOPOLOGICALSORT_H
#define TOPOLOGICALSORT_H

void TopologicalSort( Vertex* V, Node** List );
void TS_DFS( Vertex* V, Node** List );

#endif
```
TopologicalSort.c
```c
#include "TopologicalSort.h"

void TopologicalSort( Vertex* V, Node** List )
{
    while ( V != NULL && V->Visited == NotVisited )
    {
        TS_DFS( V, List );

        V = V->Next;
    }
}

void TS_DFS( Vertex* V, Node** List )
{
    Node* NewHead = NULL;
    Edge* E = NULL;

    V->Visited = Visited;

    E = V->AdjacencyList;

    while ( E != NULL )
    {
        if ( E->Target != NULL && E->Target->Visited == NotVisited )
            TS_DFS( E->Target, List );

        E = E->Next;
    }

    printf("%c\n", V->Data );

    NewHead = SLL_CreateNode( V );
    SLL_InsertNewHead( List, &NewHead );
}
```
Test_TopologicalSort.c
```c
#include "Graph.h"
#include "TopologicalSort.h"

int main( void )
{
    Node* SortedList  = NULL;
    Node* CurrentNode = NULL;

    /*  그래프 생성     */
    Graph* graph = CreateGraph();
    
    /*  정점 생성 */
    
    Vertex* A = CreateVertex( 'A' );
    Vertex* B = CreateVertex( 'B' );
    Vertex* C = CreateVertex( 'C' );
    Vertex* D = CreateVertex( 'D' );
    Vertex* E = CreateVertex( 'E' );
    Vertex* F = CreateVertex( 'F' );
    Vertex* G = CreateVertex( 'G' );
    Vertex* H = CreateVertex( 'H' );
    
    /*  그래프에 정점을 추가 */
    AddVertex( graph, A );
    AddVertex( graph, B );
    AddVertex( graph, C );
    AddVertex( graph, D );
    AddVertex( graph, E );
    AddVertex( graph, F );
    AddVertex( graph, G );
    AddVertex( graph, H );

    /*  정점과 정점을 간선으로 잇기 */
    AddEdge( A, CreateEdge( A, C, 0 ) );
    AddEdge( A, CreateEdge( A, D, 0 ) );

    AddEdge( B, CreateEdge( B, C, 0 ) );
    AddEdge( B, CreateEdge( B, E, 0 ) );

    AddEdge( C, CreateEdge( C, F, 0 ) );
    
    AddEdge( D, CreateEdge( D, F, 0 ) );
    AddEdge( D, CreateEdge( D, G, 0 ) );

    AddEdge( E, CreateEdge( E, G, 0 ) );

    AddEdge( F, CreateEdge( F, H, 0 ) );
    
    AddEdge( G, CreateEdge( G, H, 0 ) );

    /*  위상 정렬 */
    TopologicalSort( graph->Vertices, &SortedList );

 
    printf("Topological Sort Result : ");

    CurrentNode = SortedList;

    while ( CurrentNode != NULL )
    {
        printf("%C ", CurrentNode->Data->Data );
        CurrentNode = CurrentNode->NextNode;
    }
    printf("\n");
    

    /*  그래프 소멸 */
    DestroyGraph( graph );

    return 0;
}
```
실행결과
```
H
F
C
G
D
A
E
B
Topological Sort Result : B E A D G C F H
```
<br>

\# 참고<br>
[박상현 / 뇌를 자극하는 알고리즘](https://www.hanbit.co.kr/media/books/book_view.html?p_code=B3450156021){:target="_blank"}<br>



