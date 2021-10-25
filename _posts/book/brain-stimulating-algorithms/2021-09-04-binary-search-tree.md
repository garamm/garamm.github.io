---
layout: post
title: "[책/뇌를 자극하는 알고리즘] 6-3. 탐색 / 이진 탐색 트리"
author: 임가람
date: "2021-09-04 10:54:00"
categories: [책, 뇌를 자극하는 알고리즘]
tags: [뇌를 자극하는 알고리즘]
---

## 이진 탐색 트리(Binary Search Tree)
이진 탐색을 위한 이진 트리<br>
이진 탐색은 데이터 집합이 배열인 경우에만 사용할 수 있기 때문에 이진 탐색 트리를 사용한다.<br>
이진 탐색을 위해서 이진 탐색 트리는 "왼쪽 자식 노드는 나보다 작고, 오른쪽 자식 노드는 나보다 크다." 라는 규칙을 따른다.
![이진탐색트리-구조](/assets/img/posts/2021-09-04-binary-search-tree-1.png){: width="700" height="150" }<br>
<br>

---
## 이진 탐색 트리의 기본 연산
이진 탐색 트리에서 각 노드는 '중앙 요소'이다.<br>
위의 첫번째 트리를 보면 루트 노드는 23이고, 23 노드는 전체의 중앙 요소이다.<br>
23노드 의 왼쪽 하위 트리의 루트 노드는 11인데 11 노드도 왼쪽 하위 트리의 중앙 값이다.<br>
이진 탐색 트리를 사용하여 '67'을 찾으면 다음과 같다.<br>
![이진탐색트리-기본연산](/assets/img/posts/2021-09-04-binary-search-tree-2.png){: width="700" height="150" }<br>
<br>

### 이진 탐색 트리의 노드 삽입
아래의 이진 탐색 트리에 14를 삽입한다고 할 때<br>
14는 23보다 작으니 23의 왼쪽 하위 트리에 위치해야 한다.<br>
그리고 11보다는 크고 11의 오른쪽 하위 트리에 위치해야 한다.<br>
그런데 11의 오른쪽 자식 트리가 없으니 거기에 14를 연결하면 된다.<br>
![이진탐색트리-노드삽입](/assets/img/posts/2021-09-04-binary-search-tree-3.png){: width="700" height="150" }<br>
<br>

### 이진 탐색 트리의 노드 삭제
(1) 삭제할 노드가 '잎 노드'인 경우<br>
부모 노드에서 자식 노드의 포인터를 NULL로 만들고 삭제한 노드의 주소를 반환한다.<br>
![이진탐색트리-노드삭제1](/assets/img/posts/2021-09-04-binary-search-tree-4.png){: width="700" height="150" }<br>
<br>

(2) 삭제할 노드가 왼쪽/오른쪽 중 어느 한쪽 자식 노드를 갖고 있는 경우<br>
삭제되는 노드의 자식을 삭제되는 노드의 부모에게 연결해준다.<br>
![이진탐색트리-노드삭제2](/assets/img/posts/2021-09-04-binary-search-tree-5.png){: width="700" height="150" }<br>
<br>

(3) 삭제할 노드가 양쪽 자식 노드를 모두 갖고 있는 경우<br>
삭제된 노드의 오른쪽 하위 트리에서 가장 작은 값을 가진 노드를 삭제된 노드의 위치에 옮겨 놓는다.<br>
![이진탐색트리-노드삭제3](/assets/img/posts/2021-09-04-binary-search-tree-6.png){: width="700" height="150" }<br><br>
옮겨 놓은 최소값 노드가 자식이 없는 경우는 이렇게 작업이 완료된다.<br>
자식이 있는 경우 최소값 노드는 오른쪽 자식만 있다.(해당 노드는 최소값이기 때문에 왼쪽 노드가 없음) 최소값 노드의 오른쪽 자식을 최소값 노드의 원래 부모에게 연결재주면 작업이 완료된다.<br>
![이진탐색트리-노드삭제4](/assets/img/posts/2021-09-04-binary-search-tree-7.png){: width="700" height="150" }<br><br>
<br>

---
## 이진 탐색 트리 예제
BinarySearchTree.h
```c
#ifndef BINARY_SEARCH_TREE_H
#define BINARY_SEARCH_TREE_H

#include <stdio.h>
#include <stdlib.h>

typedef int ElementType;

typedef struct tagBSTNode 
{
    struct tagBSTNode* Left;
    struct tagBSTNode* Right;

    ElementType Data;
} BSTNode;

BSTNode*  BST_CreateNode( ElementType NewData );
void      BST_DestroyNode( BSTNode* Node );
void      BST_DestroyTree( BSTNode* Tree );

BSTNode*  BST_SearchNode( BSTNode* Tree, ElementType Target );
BSTNode*  BST_SearchMinNode( BSTNode* Tree );
void      BST_InsertNode( BSTNode* Tree, BSTNode *Child );
BSTNode*  BST_RemoveNode( BSTNode* Tree,BSTNode* Parent, ElementType Target );
void      BST_InorderPrintTree( BSTNode* Node );

#endif 
```
BinarySearchTree.c
```c
#include "BinarySearchTree.h"

BSTNode* BST_CreateNode( ElementType NewData )
{
    BSTNode* NewNode = (BSTNode*)malloc( sizeof(BSTNode) );
    NewNode->Left    = NULL;
    NewNode->Right   = NULL;
    NewNode->Data    = NewData;

    return NewNode;
}

void BST_DestroyNode( BSTNode* Node )
{
    free(Node);
}

void BST_DestroyTree( BSTNode* Tree )
{
    if ( Tree->Right != NULL )
        BST_DestroyTree( Tree->Right );

    if ( Tree->Left != NULL )
        BST_DestroyTree( Tree->Left );

    Tree->Left = NULL;
    Tree->Right = NULL;

    BST_DestroyNode( Tree );
}

BSTNode*  BST_SearchNode( BSTNode* Tree, ElementType Target )
{
    if ( Tree == NULL )
        return NULL;

    if ( Tree->Data == Target ) /* 목표값이 현재 노드와 같은 경우 */
        return Tree;
    else if ( Tree->Data > Target ) /* 목표값이 현재 노드보다 작은 경우 */
        return BST_SearchNode ( Tree->Left, Target );
    else /* 목표값이 현재 노드보다 큰 경우 */
        return BST_SearchNode ( Tree->Right,  Target );
}


BSTNode*  BST_SearchMinNode( BSTNode* Tree )
{
    if ( Tree == NULL )
        return NULL;

    if ( Tree->Left == NULL ) 
        return Tree;
    else
        return BST_SearchMinNode( Tree->Left );
}

void BST_InsertNode( BSTNode* Tree, BSTNode *Child)
{
    if ( Tree->Data < Child->Data ) /* 새 노드가 현재 노드보다 큰 경우 */
    {
        if ( Tree->Right == NULL )
            Tree->Right = Child;
        else
            BST_InsertNode( Tree->Right, Child );

    } 
    else if ( Tree->Data > Child->Data ) /* 새 노드가 현재 노드보다 작은 경우 */
    {
        if ( Tree->Left == NULL )
            Tree->Left = Child;
        else
            BST_InsertNode( Tree->Left, Child );
    }
}

BSTNode* BST_RemoveNode( BSTNode* Tree,BSTNode* Parent, ElementType Target )
{
    BSTNode* Removed = NULL;

    if ( Tree == NULL )
        return NULL;

    if ( Tree->Data > Target )
        Removed = BST_RemoveNode( Tree->Left, Tree, Target );
    else if ( Tree->Data < Target )
        Removed = BST_RemoveNode( Tree->Right, Tree, Target );
    else /*  목표 값을 찾은 경우. */
    {
        Removed = Tree;

        /*  잎 노드인 경우 바로 삭제 */
        if ( Tree->Left == NULL && Tree->Right == NULL )
        {
            if ( Parent->Left == Tree )
                Parent->Left = NULL;
            else 
                Parent->Right = NULL;
        }
        else
        {
            /*  자식이 양쪽 다 있는 경우 */
            if ( Tree->Left != NULL && Tree->Right != NULL ) 
            {
                /*  최소값 노드를 찾아 제거한 뒤 현재의 노드에 위치시킨다. */
                BSTNode* MinNode = BST_SearchMinNode( Tree->Right );
                /* MinNode에 대해서도 제거 후의 뒤처리가 필요하기 때문에 MinNode에 대해 BST_RemoveNode() 함수를 호출한다.  */
                MinNode = BST_RemoveNode( Tree, NULL, MinNode->Data );
                Tree->Data = MinNode->Data;
            }
            else
            {
                /*  자식이 하나만 있는 경우 */
                BSTNode* Temp = NULL;
                if ( Tree->Left != NULL )
                    Temp = Tree->Left;
                else 
                    Temp = Tree->Right;

                if ( Parent->Left == Tree )
                    Parent->Left  = Temp;
                else 
                    Parent->Right = Temp;
            }
        }
    }

    return Removed;
}

void BST_InorderPrintTree( BSTNode* Node )
{
    if ( Node == NULL )
        return;

    /*  왼쪽 하위 트리 출력 */
    BST_InorderPrintTree( Node->Left );

    /*  루트 노드 출력 */
    printf( "%d ", Node->Data );

    /*  오른쪽 하위 트리 출력 */
    BST_InorderPrintTree( Node->Right );
}
```
```c
#include "BinarySearchTree.h"

int main( void )
{
    /*  노드 생성 */
    BSTNode* Tree = BST_CreateNode(123);
    BSTNode* Node = NULL;

    /*  트리에 노드 추가 */
    BST_InsertNode( Tree, BST_CreateNode(22) );
    BST_InsertNode( Tree, BST_CreateNode(9918) );
    BST_InsertNode( Tree, BST_CreateNode(424) );
    BST_InsertNode( Tree, BST_CreateNode(17) );
    BST_InsertNode( Tree, BST_CreateNode(3) );

    BST_InsertNode( Tree, BST_CreateNode(98) );
    BST_InsertNode( Tree, BST_CreateNode(34) );

    BST_InsertNode( Tree, BST_CreateNode(760) );
    BST_InsertNode( Tree, BST_CreateNode(317) );
    BST_InsertNode( Tree, BST_CreateNode(1) );
    
	Node =  BST_SearchNode(Tree, 17 );
    if(Node != NULL)
        printf("%d \n", Node->Data);
    else
        puts("그런 노드 없어요");

    /*  트리 출력 */
    BST_InorderPrintTree( Tree );
    printf( "\n");

    /*  특정 노드 삭제 */
    printf( "Removing 98...\n");

    Node = BST_RemoveNode( Tree, NULL, 98 );
    BST_DestroyNode( Node );

    BST_InorderPrintTree( Tree );
    printf( "\n");

    /*  새 노드 삽입 */
    printf( "Inserting 111...\n");

    BST_InsertNode( Tree, BST_CreateNode(111) );
    BST_InorderPrintTree( Tree );
    printf( "\n");

    /*  트리 소멸시키기 */
    BST_DestroyTree( Tree );

    return 0;
}
```
실행결과
```
1 3 17 22 34 98 123 317 424 760 9918
Removing 98...
1 3 17 22 34 123 317 424 760 9918
Inserting 111...
1 3 17 22 34 111 123 317 424 760 9918
```

<br>

---
## 이진 탐색 트리의 문제점
아래 그림처럼 이진 탐색 트리가 검색의 효율을 극단적으로 떨어뜨릴 수 있다.
![이진탐색트리-문제점](/assets/img/posts/2021-09-04-binary-search-tree-8.png){: width="700" height="150" }<br>
그래서 이 문제를 해결하기 위해 '레드 블랙 트리'를 사용한다.

<br>

\# 참고<br>
[박상현 / 뇌를 자극하는 알고리즘](https://www.hanbit.co.kr/media/books/book_view.html?p_code=B3450156021){:target="_blank"}<br>