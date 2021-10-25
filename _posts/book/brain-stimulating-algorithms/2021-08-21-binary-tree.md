---
layout: post
title: "[책/뇌를 자극하는 알고리즘] 4-2. 이진 트리"
author: 임가람
date: "2021-08-21 15:29:00"
categories: [책, 뇌를 자극하는 알고리즘]
tags: [뇌를 자극하는 알고리즘]
---

## 이진 트리(Binary Tree)란
모든 노드가 최대 2개의 자식을 가질 수 있는 트리, 수식을 트리 형태로 표현하여 계산하게 하는 수식 이진 트리(Expression Binary Tree)와 아주 빠른 데이터 검색을 가능하게 하는 이진 탐색 트리(Binary Search Tree)등의 알고리즘에서 사용된다.
<br>

---
## 이진 트리의 여러 형태
### 포화 이진 트리(Full Binary Tree)
잎 노드를 제외한 모든 노드가 자식을 둘씩 가지고 있는 이진 트리, 잎 노드들이 모두 같은 깊이에 존재하는것이 특징
![이진트리-구조](/assets/img/posts/2021-08-21-binary-tree-1.png){: width="700" height="150" }
<br>

### 완전 이진 트리(Complete Binary Tree)
잎 노드들이 트리의 왼쪽부터 차곡차곡 채워진것이 특징
![이진트리-완전이진트리](/assets/img/posts/2021-08-21-binary-tree-2.png){: width="700" height="150" }
아래 그림은 잎 노드들 사이에 이빨이 빠져있기 때문에 완전 이진 트리가 아니다.
![이진트리-완전이진트리2](/assets/img/posts/2021-08-21-binary-tree-3.png){: width="700" height="150" }
<br>

### 높이 균형 트리(Height Balanced Tree)
루트 노드를 기준으로 왼쪽 하위 트리와 오른쪽 하위 트리의 높이가 1 이상 차이나지 않는 이진 트리
![이진트리-높이균형트리](/assets/img/posts/2021-08-21-binary-tree-4.png){: width="700" height="150" }
<br>

### 완전 높이 균형 트리(Completely Height Balanced Tree)
루트 노드를 기준으로 왼쪽 하위 트리와 오른쪽 하위 트리의 높이가 같은 이진 트리
![이진트리-완전높이균형트리](/assets/img/posts/2021-08-21-binary-tree-5.png){: width="700" height="150" }
<br>

---
## 이진 트리의 순회
### 전위 순회(Preorder Traversal)
루트 노드부터 잎 노드까지 아래 방향으로 방문
![이진트리-전위순회](/assets/img/posts/2021-08-21-binary-tree-6.png){: width="700" height="150" }
( A ( B ( C, D ), ( E ( F, G ) ) )
<br>

### 중위 순회(Inorder Traversal)
왼쪽 하위 트리부터 오른쪽 하위 트리 방향으로 방문
![이진트리-중위순회](/assets/img/posts/2021-08-21-binary-tree-7.png){: width="700" height="150" }
( 1 * 2 ) + ( 7 - 8 )
<br>

### 후위 순회(Postorder Traversal)
루트, 왼쪽 하위 트리, 오른쪽 하위 트리 순으로 방문
![이진트리-후위순회](/assets/img/posts/2021-08-21-binary-tree-8.png){: width="700" height="150" }
1 2 * 7 8 - +
<br>

---
## 이진 트리 구현하기
### 노드의 소멸
트리를 파괴할 때는 반드시 잎 노드부터 자유 저장소에서 제거해야 한다.<br>
따라서 잎 노드부터 방문하여 루트 노드까지 거슬러 올라가면서 방문하는 `후위 순회`를 이용하여 이진 트리 전체를 소멸시키면 된다.<br>
![이진트리-노드소멸](/assets/img/posts/2021-08-21-binary-tree-9.png){: width="700" height="150" }
<br>

---
## 소스코드

BinaryTree.h
```c
#ifndef BINARY_TREE_H
#define BINARY_TREE_H

#include <stdio.h>
#include <stdlib.h>

typedef char ElementType;

typedef struct tagSBTNode 
{
    struct tagSBTNode* Left;
    struct tagSBTNode* Right;

    ElementType Data;
} SBTNode;

SBTNode*  SBT_CreateNode( ElementType NewData );
void      SBT_DestroyNode( SBTNode* Node );
void      SBT_DestroyTree( SBTNode* Root );

void      SBT_PreorderPrintTree( SBTNode* Node );
void      SBT_InorderPrintTree( SBTNode* Node );
void      SBT_PostorderPrintTree( SBTNode* Node );

#endif BINARY_TREE_H
```
BinaryTree.c
```c
#include "BinaryTree.h"

SBTNode* SBT_CreateNode( ElementType NewData )
{
    SBTNode* NewNode = (SBTNode*)malloc( sizeof(SBTNode) );
    NewNode->Left    = NULL;
    NewNode->Right   = NULL;
    NewNode->Data    = NewData;

    return NewNode;
}

void SBT_DestroyNode( SBTNode* Node )
{
    free(Node);
}

void SBT_DestroyTree( SBTNode* Node )
{
    if ( Node == NULL )
        return;

    /*  왼쪽 하위 트리 소멸 */
    SBT_DestroyTree( Node->Left );

    /*  오른쪽 하위 트리 소멸 */
    SBT_DestroyTree( Node->Right );

    /*  루트 노드 소멸 */
    SBT_DestroyNode( Node );
}

void SBT_PreorderPrintTree( SBTNode* Node )
{
    if ( Node == NULL )
        return;

    /*  루트 노드 출력 */
    printf( " %c", Node->Data );

    /*  왼쪽 하위 트리 출력 */
    SBT_PreorderPrintTree( Node->Left );

    /*  오른쪽 하위 트리 출력 */
    SBT_PreorderPrintTree( Node->Right );
}

void SBT_InorderPrintTree( SBTNode* Node )
{
    if ( Node == NULL )
        return;
    
    /*  왼쪽 하위 트리 출력 */
    SBT_InorderPrintTree( Node->Left );

    /*  루트 노드 출력 */
    printf( " %c", Node->Data );
    
    /*  오른쪽 하위 트리 출력 */
    SBT_InorderPrintTree( Node->Right );
}

void SBT_PostorderPrintTree( SBTNode* Node )
{
    if ( Node == NULL )
        return;
    
    /*  왼쪽 하위 트리 출력 */
    SBT_PostorderPrintTree( Node->Left );

    /*  오른쪽 하위 트리 출력 */
    SBT_PostorderPrintTree( Node->Right );

    /*  루트 노드 출력 */
    printf( " %c", Node->Data );
}
```
Test_BinaryTree.c
```c
#include "BinaryTree.h"

int main( void )
{
    /*  노드 생성 */
    SBTNode* A = SBT_CreateNode('A');
    SBTNode* B = SBT_CreateNode('B');
    SBTNode* C = SBT_CreateNode('C');
    SBTNode* D = SBT_CreateNode('D');
    SBTNode* E = SBT_CreateNode('E');
    SBTNode* F = SBT_CreateNode('F');
    SBTNode* G = SBT_CreateNode('G');
    
    /*  트리에 노드 추가 */
    A->Left  = B;
    B->Left  = C;
    B->Right = D;

    A->Right = E;
    E->Left  = F;
    E->Right = G;
    
    /*  트리 출력 */
    printf("Preorder ...\n");
    SBT_PreorderPrintTree( A );
    printf("\n\n");

    printf("Inorder ... \n");
    SBT_InorderPrintTree( A );
    printf("\n\n");

    printf("Postorder ... \n");
    SBT_PostorderPrintTree( A );
    printf("\n");

    /*  트리 소멸시키기 */
    SBT_DestroyTree( A );

    return 0;
}
```
실행결과
```
Preorder ...
 A B C D E F G

Inorder ...
 C B D A F E G

Postorder ...
 C D B F G E A
```

<br>

\# 참고<br>
[박상현 / 뇌를 자극하는 알고리즘](https://www.hanbit.co.kr/media/books/book_view.html?p_code=B3450156021){:target="_blank"}<br>